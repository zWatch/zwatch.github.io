# Spirit

## include

Spirit包含五个子库以及一个常见支持类的“支持”模块放置在这：

1. Classic
2. Qi
3. Karma
4. Lex
5. Phoenix
6. Support

The home directory:

```txt
BOOST_ROOT/boost/spirit/home
```

is the *real* home of Spirit. It is the place where the various sub-libraries actually exist. The home directory contains:

```txt
[classic]   [karma]     [lex]
[phoenix]   [qi]        [support]
```

### [Syntax Diagram](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/syntax_diagram.html)

在下一节中，我们将处理解析表达语法（PEG）[3]，这是扩展Backus-Naur形式（EBNF）[4]的变体，但具有不同的解释。 使用语法图更容易理解PEG。 语法图以图形方式表示语法。 Niklaus Wirth [5]在“ Pascal用户手册”中广泛使用了它。 语法图与流程图相似，因此程序员很容易理解。 图和函数的同构性使其非常适合表示递归下降解析器，而递归下降解析器本质上是相互递归的函数。

### 元素[Elements](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/syntax_diagram.html#spirit.abstracts.syntax_diagram.elements)

All diagrams have one entry and one exit point. Arrows connect all possible paths through the grammar from the entry point to the exit point.

所有图（语法图）都有一个入口和一个出口点。 箭头通过语法将所有可能的路径从入口点连接到出口点。

![start_stop](E:\nyj\Zwatch\zwatch.github.io\source\start_stop.png)

Terminals are represented by round boxes. Terminals are atomic and usually represent plain characters, strings or tokens.

终端由圆形框表示。 终端是原子的，通常代表纯字符，字符串或标记。

![terminal](E:\nyj\Zwatch\zwatch.github.io\source\terminal.png)

非终端（没有终止符）用方框表示。 图使用命名的非终端进行了模块化。 可以将复杂图分解为一组非终端。 非终端也可以进行递归操作（即非终端可以调用自身）。

![non-terminal](E:\nyj\Zwatch\zwatch.github.io\source\non-terminal.png)

### 构造[Constructs](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/syntax_diagram.html#spirit.abstracts.syntax_diagram.constructs)

The most basic composition is the Sequence. B follows A:

最基本的组成是序列。 B跟随A：

![sequence](E:\nyj\Zwatch\zwatch.github.io\source\sequence.png)

此后，有序选择将称为替代。 在PEG中，有序选择和替代方法并不完全相同。 PEG允许选择一个或多个分支可以成功的歧义。 在PEG中，如果有歧义，第一个总是获胜。

![alternative](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/alternative.png)

The optional (zero-or-one):

![optional](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/optional.png)

Now, the loops. We have the zero-or-more and one-or-more:

![kleene](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/kleene.png)

![plus](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/plus.png)

请注意，就像在PEG中一样，这些循环会贪婪地运行。 如果在端点之前还有另一个“ A”，它将总是失败，因为前面的循环已经用尽了所有“ A”，并且仅剩一点。 这是PEG与通用上下文无关文法（CFG）之间的关键区别。 对于语法图，此行为非常明显，因为它们类似于流程图。

### 谓词[Predicates](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/syntax_diagram.html#spirit.abstracts.syntax_diagram.predicates)

Now, the following are Syntax Diagram versions of PEG predicates. These are not traditionally found in Syntax Diagrams. These are special extensions we invented to closely follow PEGs.

现在，以下是PEG谓词的语法图版本。 传统上在语法图中找不到这些。 这些是我们发明的特殊扩展，旨在紧跟PEG。

First, we introduce a new element, the Predicate:

> ![predicate](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/predicate.png)

This is similar to the conditionals in flow charts where the 'No' branch is absent and always signals a failed parse.

这类似于流程图中的条件，其中不存在“否”分支，并始终表示解析失败。

我们有两个版本的谓词，“与”谓词和“非”谓词：

![and_predicate](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/and_predicate.png)

![not_predicate](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/not_predicate.png)

And-Predicate尝试谓词P，如果P成功则成功，否则失败。 非谓词则相反。 它尝试谓词P，如果P成功则失败，否则成功。 两种版本都可以进行前瞻，但无论P成功与否，都不会消耗任何输入。

## 解析表达式语法[Parsing Expression Grammar](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/parsing_expression_grammar.html)

解析表达语法（PEG）[6]是扩展Backus-Naur形式（EBNF）[7]的派生词，具有不同的解释，旨在表示递归下降解析器。 PEG可以直接表示为递归下降解析器。

像EBNF一样，PEG是一种形式语法，用于根据用于识别该语言字符串的一组规则来描述一种形式语言。 与EBNF不同，PEG具有精确的解释。 每个PEG语法只有一个有效的解析树（请参阅抽象语法树）。

### 顺序[Sequences](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/parsing_expression_grammar.html#spirit.abstracts.parsing_expression_grammar.sequences)

Sequences are represented by juxtaposition like in EBNF:

```txt
a b
```

The PEG expression above states that, in order for this to succeed, `b` must follow `a`. Here's the syntax diagram:

上面的PEG表达式指出，要使此操作成功，“ b”必须跟在“ a”之后。 这是语法图：

![sequence](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/sequence.png)

Here's a trivial example:

这是一个简单的例子：

```txt
'x' digit
```

which means the character `x` must be followed by a digit.

这意味着字符x后面必须有一个数字。

### 备择方案[Alternatives](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/parsing_expression_grammar.html#spirit.abstracts.parsing_expression_grammar.alternatives)

Alternatives are represented in PEG using the slash:

替代方案在PEG中使用斜杠表示：

```txt
a / b
```

备选方案允许选择。 上面的表达式为：尝试匹配a。 如果a成功，则成功，否则请尝试匹配b。 这与通常只匹配a或b的EBNF解释有些偏差。 这是语法图：

![alternative](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/images/alternative.png)

PEG允许在替代方案中产生歧义。 在上面的表达式中，a或b都可以匹配输入字符串。 但是，只有第一个匹配替代项才有效。 如前所述，只能有一个有效的解析树。

### 循环[Loops](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/parsing_expression_grammar.html#spirit.abstracts.parsing_expression_grammar.loops)

再次，与EBNF一样，PEG使用正则表达式Kleene星号和加号循环：

![kleene](E:\nyj\Zwatch\zwatch.github.io\source\kleene.png)

![plus](E:\nyj\Zwatch\zwatch.github.io\source\plus.png)

第一个称为Kleene星，匹配零个或多个其主题a。 第二个加号匹配一个或多个主题a。

Unlike EBNF, PEGs have greedy loops. It will match as much as it can until its subject fails to match without regard to what follows. The following is a classic example of a fairly common EBNF/regex expression failing to match in PEG:

与EBNF不同，PEG具有贪婪环。 它会尽可能地匹配，直到其主题不匹配而无视后续内容。 以下是一个相当常见的EBNF / regex表达无法在PEG中匹配的经典示例：

```txt
alnum* digit
```

在PEG中，数字会吃掉尽可能多的字母数字字符，而剩下的就不多了。 因此，尾随数字将一无所获。 循环在递归下降代码中简单地实现为for / while循环，从而使循环极为有效。 那是绝对的优势。 另一方面，那些熟悉EBNF和正则表达式行为的人可能会发现该行为是主要的陷阱。 PEG提供了其他两种机制来规避这一问题。 不久我们将看到更多其他机制。

### 区别[Difference](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/abstracts/parsing_expression_grammar.html#spirit.abstracts.parsing_expression_grammar.difference)

在某些情况下，您可能希望限制某个表达式。 您可以将PEG表达式视为潜在的无限字符串集的匹配项。 差异运算符允许您限制此集合：

The expression reads: match `a` but not `b`.
