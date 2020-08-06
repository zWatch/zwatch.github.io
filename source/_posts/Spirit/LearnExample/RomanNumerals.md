# [![Boost C++ Libraries](https://www.boost.org/gfx/space.png)](https://www.boost.org/)

...one of the most highly regarded and expertly designed C++ library projects in the world.— [Herb Sutter](http://www.gotw.ca/) and [Andrei Alexandrescu](http://en.wikipedia.org/wiki/Andrei_Alexandrescu), [C++ Coding Standards](http://safari.awprofessional.com/?XmlId=0321113586)

[![Prev](https://www.boost.org/doc/libs/1_73_0/doc/src/images/prev.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/number_list_attribute___one_more__with_style.html)[![Up](https://www.boost.org/doc/libs/1_73_0/doc/src/images/up.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials.html)[![Home](https://www.boost.org/doc/libs/1_73_0/doc/src/images/home.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/index.html)[![Next](https://www.boost.org/doc/libs/1_73_0/doc/src/images/next.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/employee___parsing_into_structs.html)

#### [Roman Numerals](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/roman_numerals.html)

This example demonstrates:

- symbol table
- rule
- grammar

###### [Symbol Table](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/roman_numerals.html#spirit.qi.tutorials.roman_numerals.symbol_table)

The symbol table holds a dictionary of symbols where each symbol is a sequence of characters (a `char`, `wchar_t`, `int`, enumeration etc.) . The template class, parameterized by the character type, can work efficiently with 8, 16, 32 and even 64 bit characters. Mutable data of type T are associated with each symbol.

符号表包含符号字典，其中每个符号都是字符序列（“ char”，“ wchar_t”，“ int”，枚举等）。 通过字符类型参数化的模板类可以有效处理8位，16位，32位甚至64位字符。 类型T的可变数据与每个符号关联。

Traditionally, symbol table management is maintained separately outside the BNF grammar through semantic actions. Contrary to standard practice, the Spirit symbol table class `symbols` is a parser. An object of which may be used anywhere in the EBNF grammar specification. It is an example of a dynamic parser. A dynamic parser is characterized by its ability to modify its behavior at run time. Initially, an empty symbols object matches nothing. At any time, symbols may be added or removed, thus, dynamically altering its behavior.

传统上，符号表管理是通过语义动作在BNF语法之外单独维护的。 与标准做法相反，Spirit符号表类`symbols`是解析器。 可以在EBNF语法规范中的任何地方使用其对象。 这是动态解析器的一个示例。 动态解析器的特点是能够在运行时修改其行为。 最初，空符号对象不匹配任何内容。 任何时候都可以添加或删除符号，从而动态更改其行为。

Each entry in a symbol table has an associated mutable data slot. In this regard, one can view the symbol table as an associative container (or map) of key-value pairs where the keys are strings.

符号表中的每个条目都有一个关联的可变数据插槽。 在这方面，可以将符号表视为键-值对的关联容器（或映射），其中键是字符串。

The symbols class expects two template parameters. The first parameter specifies the character type of the symbols. The second specifies the data type associated with each symbol: its attribute.

Symbols类需要两个模板参数。 第一个参数指定符号的字符类型。 第二个参数指定与每个符号关联的数据类型：其属性。

Here's a parser for roman hundreds (100..900) using the symbol table. Keep in mind that the data associated with each slot is the parser's attribute (which is passed to attached semantic actions).

这是一个使用符号表的罗马数百（100..900）解析器。 请记住，与每个插槽关联的数据是解析器的属性（传递给附加的语义动作）。

```
struct hundreds_ : qi::symbols<char, unsigned>
{
    hundreds_()
    {
        add
            ("C"    , 100)
            ("CC"   , 200)
            ("CCC"  , 300)
            ("CD"   , 400)
            ("D"    , 500)
            ("DC"   , 600)
            ("DCC"  , 700)
            ("DCCC" , 800)
            ("CM"   , 900)
        ;
    }

} hundreds;
```



Here's a parser for roman tens (10..90):

最后，对于那些（1..9）：



```
struct tens_ : qi::symbols<char, unsigned>
{
    tens_()
    {
        add
            ("X"    , 10)
            ("XX"   , 20)
            ("XXX"  , 30)
            ("XL"   , 40)
            ("L"    , 50)
            ("LX"   , 60)
            ("LXX"  , 70)
            ("LXXX" , 80)
            ("XC"   , 90)
        ;
    }

} tens;
```



and, finally, for ones (1..9):



```
struct ones_ : qi::symbols<char, unsigned>
{
    ones_()
    {
        add
            ("I"    , 1)
            ("II"   , 2)
            ("III"  , 3)
            ("IV"   , 4)
            ("V"    , 5)
            ("VI"   , 6)
            ("VII"  , 7)
            ("VIII" , 8)
            ("IX"   , 9)
        ;
    }

} ones;
```



Now we can use `hundreds`, `tens` and `ones` anywhere in our parser expressions. They are all parsers.

现在我们可以在解析器表达式中的任何地方使用“数百”，“十”和“一个”。 它们都是解析器。

###### [Rules](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/roman_numerals.html#spirit.qi.tutorials.roman_numerals.rules)

Up until now, we've been inlining our parser expressions, passing them directly to the `phrase_parse` function. The expression evaluates into a temporary, unnamed parser which is passed into the `phrase_parse` function, used, and then destroyed. This is fine for small parsers. When the expressions get complicated, you'd want to break the expressions into smaller easier-to-understand pieces, name them, and refer to them from other parser expressions by name.

到目前为止，我们一直在内嵌解析器表达式，将它们直接传递给phrase_parse函数。 该表达式求值到一个临时的，未命名的解析器，该解析器传递到phrase_parse函数中，使用后销毁。 这对于小型解析器来说很好。 当表达式变得复杂时，您希望将表达式分解为更易于理解的较小部分，命名它们，并按名称从其他解析器表达式中引用它们。

A parser expression can be assigned to what is called a "rule". There are various ways to declare rules. The simplest form is:

可以将解析器表达式分配给所谓的“规则”。 有多种方法可以声明规则。 最简单的形式是：

```
rule<Iterator> r;
```

At the very least, the rule needs to know the iterator type it will be working on. This rule cannot be used with `phrase_parse`. It can only be used with the `parse` function -- a version that does not do white space skipping (does not have the skipper argument). If you want to have it skip white spaces, you need to pass in the type skip parser, as in the next form:

至少，规则需要知道它将要处理的迭代器类型。 该规则不能与phrase_parse一起使用。 它只能与parse函数一起使用-该版本不跳过空格（不具有skipper参数）。 如果要让它跳过空格，则需要传入类型跳过解析器，如下所示：

```
rule<Iterator, Skipper> r;
```

Example:

```
rule<std::string::iterator, space_type> r;
```

This type of rule can be used for both `phrase_parse` and `parse`.

For our next example, there's one more rule form you should know about:

这种类型的规则可用于phrase_parse和parse。

对于我们的下一个示例，您应该了解另外一个规则表：

```
rule<Iterator, Signature> r;
```

or

```
rule<Iterator, Signature, Skipper> r;
```

| ![[Tip]](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/tip.png) | Tip  |
| ------------------------------------------------------------ | ---- |
| All rule template arguments after Iterator can be supplied in any order.迭代器之后的所有规则模板参数均可按任何顺序提供。 |      |

The Signature specifies the attributes of the rule. You've seen that our parsers can have an attribute. Recall that the `double_` parser has an attribute of `double`. To be precise, these are *synthesized* attributes. The parser "synthesizes" the attribute value. Think of them as function return values.

签名指定规则的属性。 您已经看到我们的解析器可以具有一个属性。 回想一下，“ double_”解析器的属性为“ double”。 确切地说，这些是“综合”属性。 解析器“合成”属性值。 将它们视为函数返回值。

There's another type of attribute called "inherited" attribute. We won't need them for now, but it's good that you be aware of such attributes. You can think of them as function arguments. And, rightly so, the rule signature is a function signature of the form:

还有另一种属性，称为“继承”属性。 我们暂时不需要它们，但是您最好知道这些属性。 您可以将它们视为函数参数。 而且，正确的是，规则签名是以下形式的函数签名：

```
result(argN, argN,..., argN)
```

After having declared a rule, you can now assign any parser expression to it. Example:

声明规则后，您现在可以为其分配任何解析器表达式。 例：

```
r = double_ >> *(',' >> double_);
```

###### [Grammars](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/roman_numerals.html#spirit.qi.tutorials.roman_numerals.grammars)

A grammar encapsulates one or more rules. It has the same template parameters as the rule. You declare a grammar by:

语法封装了一个或多个规则。 它具有与规则相同的模板参数。 您通过以下方式声明语法：

1. deriving a struct (or class) from the `grammar` class template从`grammar`类模板派生结构（或类）
2. declare one or more rules as member variables声明一个或多个规则(rule)作为成员变量
3. initialize the base grammar class by giving it the start rule (its the first rule that gets called when the grammar starts parsing) 通过给它一个开始规则来初始化基础语法类（它是语法开始解析时被调用的第一个规则）
4. initialize your rules in your constructor 在构造函数中初始化规则

The roman numeral grammar is a very nice and simple example of a grammar:

罗马数字语法是语法的一个非常好的简单示例：



```
template <typename Iterator>
struct roman : qi::grammar<Iterator, unsigned()>
{
    roman() : roman::base_type(start)
    {
        using qi::eps;
        using qi::lit;
        using qi::_val;
        using qi::_1;
        using ascii::char_;

        start = eps             [_val = 0] >>
            (
                +lit('M')       [_val += 1000]
                ||  hundreds    [_val += _1]
                ||  tens        [_val += _1]
                ||  ones        [_val += _1]
            )
        ;
    }

    qi::rule<Iterator, unsigned()> start;
};
```



Things to take notice of:

注意事项：

- The grammar and start rule signature is `unsigned()`. It has a synthesized attribute (return value) of type `unsigned` with no inherited attributes (arguments).语法和开始规则签名为`unsigned（）`。 它具有类型为unsigned的综合属性（返回值），没有继承的属性（自变量）。
- We did not specify a skip-parser. We don't want to skip in between the numerals.我们没有指定跳过解析器。 我们不想在数字之间跳过。
- `roman::base_type` is a typedef for `grammar<Iterator, unsigned()>`. If `roman` was not a template, you could simply write: base_type(start) ``roman :: base_type''是grammar <Iterator，unsigned（）>的类型定义。 如果`roman`是不是一个模板，你可以简单地写：base_type（start）
- It's best to make your grammar templates such that they can be reused for different iterator types.
- `_val` is another [Boost.Phoenix](https://www.boost.org/doc/libs/1_73_0/libs/phoenix/doc/html/index.html) placeholder representing the rule's synthesized attribute.
- `eps` is a special spirit parser that consumes no input but is always successful. We use it to initialize `_val`, the rule's synthesized attribute, to zero before anything else. The actual parser starts at `+lit('M')`, parsing roman thousands. Using `eps` this way is good for doing pre and post initializations.
- The expression `a || b` reads: match a or b and in sequence. That is, if both `a` and `b` match, it must be in sequence; this is equivalent to `a >> -b | b`, but more efficient.

###### [Let's Parse!](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/roman_numerals.html#spirit.qi.tutorials.roman_numerals.let_s_parse_)



```
bool r = parse(iter, end, roman_parser, result);

if (r && iter == end)
{
    std::cout << "-------------------------\n";
    std::cout << "Parsing succeeded\n";
    std::cout << "result = " << result << std::endl;
    std::cout << "-------------------------\n";
}
else
{
    std::string rest(iter, end);
    std::cout << "-------------------------\n";
    std::cout << "Parsing failed\n";
    std::cout << "stopped at: \": " << rest << "\"\n";
    std::cout << "-------------------------\n";
}
```



`roman_parser` is an object of type `roman`, our roman numeral parser. This time around we are using the no-skipping version of the parse functions. We do not want to skip any spaces! We are also passing in an attribute, `unsigned result`, which will receive the parsed value.

The full cpp file for this example can be found here: [../../example/qi/roman.cpp](https://www.boost.org/doc/libs/1_73_0/libs/spirit/example/qi/roman.cpp)

|      | Copyright © 2001-2011 Joel de Guzman, Hartmut KaiserDistributed under the Boost Software License, Version 1.0. (See accompanying file LICENSE_1_0.txt or copy at [http://www.boost.org/LICENSE_1_0.txt](https://www.boost.org/LICENSE_1_0.txt)) |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

------

[![Prev](https://www.boost.org/doc/libs/1_73_0/doc/src/images/prev.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/number_list_attribute___one_more__with_style.html)[![Up](https://www.boost.org/doc/libs/1_73_0/doc/src/images/up.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials.html)[![Home](https://www.boost.org/doc/libs/1_73_0/doc/src/images/home.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/index.html)[![Next](https://www.boost.org/doc/libs/1_73_0/doc/src/images/next.png)](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/employee___parsing_into_structs.html)