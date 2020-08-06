# 属性[Attributes](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/attributes.html)



## 原始组件的属性

```C++
int value = 0;
std::string str("123");
std::string::iterator strbegin = str.begin();
qi::parse(strbegin, str.end(), int_, value);   // value == 123


int value = 123;
std::string str;
std::back_insert_iterator<std::string> out(str);
karma::generate(out, int_, value);                // str == "123"
```

## 复合成分的属性[Attributes of Compound Components](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/attributes/compound_attributes.html)

*Spirit.Qi* and *Spirit.Karma* implement well defined attribute type propagation rules for all compound parsers and generators, such as sequences, alternatives, Kleene star, etc. The main attribute propagation rule for a sequences is for instance:

Spirit.Qi和Spirit.Karma为所有复合解析器和生成器实现了定义良好的属性类型传播规则，例如序列，替代项，Kleene star等。序列的主要属性传播规则例如为：

| Library | Sequence attribute propagation rule    |
| ------- | -------------------------------------- |
| Qi      | `a: A, b: B --> (a >> b): tuple<A, B>` |
| Karma   | `a: A, b: B --> (a << b): tuple<A, B>` |

内容为：

给定a和b是解析器（生成器），并且A是a的属性类型，而B是b的属性类型，则a >> b（a << b）的属性类型将是tuple <A，B >。

**[注意]**
元组<A，B>表示法可用于任何保留类型A和B的融合序列的占位符表达，例如boost :: fusion :: tuple <A，B>或std :: pair <A，B>（ 有关更多信息，请参见Boost.Fusion。

如您所见，为了使类型与复合表达式的属性类型兼容，它必须

- either be convertible to the attribute type,

- or it has to expose certain functionalities, i.e. it needs to conform to a concept compatible with the component.

  

- 可以转换为属性类型，
- 或者它必须公开某些功能，即它需要符合与组件兼容的概念。

每个复合组件都实现自己的一组属性传播规则。 有关不同复合生成器如何使用属性的完整列表，请参阅解析器复合属性规则（[Parser Compound Attribute Rules](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/quick_reference/compound_attribute_rules.html)）和生成器复合属性规则部分（[Generator Compound Attribute Rules](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/karma/quick_reference/compound_attribute_rules.html)）。

### 序列解析器和生成器的属性

序列需要一种属性类型来公开融合序列的概念，其中该融合序列的所有元素必须与组成序列的相应元素兼容。 例如，表达式：

| Library | Sequence expression  |
| ------- | -------------------- |
| Qi      | `double_ >> double_` |
| Karma   | `double_ << double_` |

与具有两种类型的任何融合序列均兼容，其中两种类型均必须与double兼容。 融合序列的第一个元素必须与第一个double_的属性兼容，并且融合序列的第二个元素必须与第二个double_的属性兼容。 如果我们假设有一个std :: pair <double，double>的实例，我们可以直接使用上面的表达式来完成这两个操作，解析输入以填充属性：

```C++
// the following parses "1.0 2.0" into a pair of double
std::string input("1.0 2.0");
std::string::iterator strbegin = input.begin();
std::pair<double, double> p;
qi::phrase_parse(strbegin, input.end(),
    qi::double_ >> qi::double_,       // parser grammar 
    qi::space,                        // delimiter grammar
    p);                               // attribute to fill while parsing
```

and generate output for it:

```
// the following generates: "1.0 2.0" from the pair filled above
std::string str;
std::back_insert_iterator<std::string> out(str);
karma::generate_delimited(out,
    karma::double_ << karma::double_, // generator grammar (format description)
    karma::space,                     // delimiter grammar
    p);                               // data to use as the attribute 
```

（其中将karma :: space生成器用作分隔符，从而允许自动跳过/插入所有原语之间的分隔符）。

Tip
仅对于序列：Spirit.Qi和Spirit.Karma公开了一组主要可用于序列的API函数。 与scanf和printf系列的功能非常相似，这些功能允许分别传递序列中每个元素的属性。 使用Qi的解析或Karma的generate（）的相应重载，可以将以上表达式重写为：

```C++
double d1 = 0.0, d2 = 0.0;
qi::phrase_parse(begin, end, qi::double_ >> qi::double_, qi::space, d1, d2);
karma::generate_delimited(out, karma::double_ << karma::double_, karma::space, d1, d2);
```

其中第一个属性用于第一个double_，第二个属性用于第二个double_。

### 备用解析器和生成器的属性

替代解析器和生成器都是关于-替代-的。 为了存储来自不同替代方案的可能不同的结果（属性）类型，我们使用数据类型Boost.Variant。 这些组件的主要属性传播规则是：

```
a: A, b: B --> (a | b): variant<A, B>
```

备选方案具有第二个非常重要的属性传播规则：

```
a: A, b: A --> (a | b): A
```

通常可以大大简化事情。 如果替代方案的所有子表达式都公开相同的属性类型，则整个替代方案也将公开完全相同的属性类型。

## 有关复合组件的跟多信息[More About Attributes of Compound Components](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/attributes/more_compound_attributes.html)

在解析输入或生成输出时，通常需要将一些常量元素与可变部分组合在一起。 例如，让我们看一个解析或格式化复数的示例，该复数被写为（real，imag），其中real和imag是代表我们复数的实部和虚部的变量。 这可以通过编写以下代码来实现：

| Library | Sequence expression                        |
| ------- | ------------------------------------------ |
| Qi      | `'(' >> double_ >> ", " >> double_ >> ')'` |
| Karma   | `'(' << double_ << ", " << double_ << ')'` |

幸运的是，文字（例如'（'和“，”）不会公开任何属性（实际上，它们确实公开了特殊类型的unused_type，但是在这种情况下，unused_type被解释为组件完全不公开任何属性） 理解这些文字不消耗传递给该组成序列的融合序列的任何元素是非常重要的，正如它们所说，它们只是不公开任何属性，也不产生（消耗）任何数据。 以下示例显示了这一点：

```c++
// the following parses "(1.0, 2.0)" into a pair of double
std::string input("(1.0, 2.0)");
std::string::iterator strbegin = input.begin();
std::pair<double, double> p;
qi::parse(strbegin, input.end(),
    '(' >> qi::double_ >> ", " >> qi::double_ >> ')', // parser grammar 
    p);                                               // attribute to fill while parsing
```

这是等效的Spirit.Karma代码段：

```c++
// the following generates: (1.0, 2.0)
std::string str;
std::back_insert_iterator<std::string> out(str);
generate(out,
    '(' << karma::double_ << ", " << karma::double_ << ')', // generator grammar (format description)
    p);                                                     // data to use as the attribute 
```

其中作为要生成的数据传入的对中的第一个元素仍与第一double_关联，而第二个元素与第二double_生成器关联。

这种行为应该很熟悉，因为它符合其他输入和输出格式库（例如scanf，printf或boost :: format）处理其可变部分的方式。 在这种情况下，您可以考虑将Spirit.Qi和Spirit.Karma的原始组件（例如上述double_）用作属性值的类型安全占位符。



类似于上面提供的尖端，本实施例可以使用Spirit的multi-attribute API函数改写为：

```
double d1 = 0.0, d2 = 0.0;
qi::parse(begin, end, '(' >> qi::double_ >> ", " >> qi::double_ >> ')', d1, d2);
karma::generate(out, '(' << karma::double_ << ", " << karma::double_ << ')', d1, d2);
```

它提供了清晰舒适的语法，与`printf` or `boost::format`.公开的基于占位符的语法更相似。

让我们从更正式的角度来看一下。 如果涉及到将未使用的类型作为其属性公开的生成器，则序列属性传播规则定义一种特殊的行为（请参见生成器复合属性规则）：

| Library | Sequence attribute propagation rule |
| ------- | ----------------------------------- |
| Qi      | `a: A, b: Unused --> (a >> b): A`   |
| Karma   | `a: A, b: Unused --> (a << b): A`   |

内容为：

给定a和b是解析器（生成器），并且A是a的属性类型，而unused_type是b的属性类型，那么a >> b（a << b）的属性类型也将是A。 无论暴露了unused_type的元素位于什么位置，该规则都适用。

一旦涉及文字，此规则是理解序列中的属性处理的关键。 就像在属性传播过程中具有“ unused_type”属性的元素“消失”一样。 值得注意的是，不仅对于序列，而且对于任何复合成分都是如此。 例如，对于替代组件，相应的规则是：

```
a: A, b: Unused --> (a | b): A
```

同样，可以简化表达式的整体属性类型。

## 规则和文法的属性[Attributes of Rules and Grammars](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/attributes/nonterminal_attributes.html)

非终结符在解析器中是众所周知的，在非终结符中，非终结符被用作从更简单的解析器构建更复杂的解析器的主要手段。 解析器世界中的非终结符与命令式编程语言中的函数非常相似。 它们可用于封装特定输入序列的解析器表达式。 定义后，无论何时需要识别封装的输入，非终结符都可以在更复杂的表达式中用作“普通”解析器。 Spirit.Qi中的解析器非终结符可以接受参数（继承的属性），并且通常返回值（合成的属性）。

在定义特定的语法或规则时，必须明确指定继承属性和综合属性的类型（Spirit Repository还具有符合相似接口的子规则）。 例如，以下代码声明了一个Spirit.Qi规则，该规则公开一个int作为其综合属性，同时希望将一个double用作其继承属性（有关更多信息，请参见关于Spirit.Qi规则的部分）：

```
qi::rule<Iterator, int(double)> r;
```

在生成器的世界中，非终结符与解析器的世界一样有用。生成器非终结符封装了特定数据类型的格式描述，每当我们需要为该数据类型发出输出时，就会以与预定义的Spirit.Karma生成器原语相似的方式调用相应的非终结符。 Spirit.Karma非终结符与Spirit.Qi非终结符非常相似。生成器的非终结符也可以接受参数，我们也称这些继承的属性。主要区别在于它们不像语法分析器那样公开综合属性，但是它们需要特殊的消耗属性。通常，消费属性是生成器从其创建输出的值。即使未从生成器“返回”消耗的属性，我们仍选择使用与Spirit.Qi中相同的函数样式声明语法。下面的示例声明了Spirit.Karma规则，该规则消耗了double且不期望任何其他继承的属性。

```
karma::rule<OutputIterator, double()> r;
```

非终端解析器和生成器的继承属性通常在调用过程中传递给组件。 这些是解析器或生成器可以接受的参数，它们可以用于根据调用它们的上下文来对组件进行参数化。