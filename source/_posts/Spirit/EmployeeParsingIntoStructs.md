# [Employee - Parsing into structs](https://www.boost.org/doc/libs/1_73_0/libs/spirit/doc/html/spirit/qi/tutorials/employee___parsing_into_structs.html)

在Spirit General列表中，这是一个常见的问题：如何解析结果并将其放入C ++结构中？ 当然，在这一点上，您已经知道使用语义动作的各种方法。 有很多方法可以剥猫皮。 Spirit2完全归功于它，使它变得更加容易。 下一个示例演示了Spirit2的某些功能，这些功能使此操作变得容易。 在此过程中，您将了解：

- More about attributes 有关属性的更多信息
- Auto rules 自动规则
- Some more built-in parsers 一些内置的解析器
- Directives 指令


首先，让我们创建一个代表员工的结构：


```C++
struct employee
{
    int age;
    std::string surname;
    std::string forename;
    double salary;
};
```

然后，我们需要告诉Boost.Fusion我们的员工结构，使其成为语法可以利用的一等Fusion公民。 如果您还不了解Fusion，那么它是一个Boost库，用于处理异构数据集合（通常称为元组）。 Spirit广泛使用Fusion作为其基础架构的一部分。

在Fusion看来，结构只是元组的一种形式。 您可以将任何结构改编为完全符合条件的融合元组：

```C++
#define BOOST_FUSION_ADAPT_STRUCT(__struct_name, ...)
BOOST_FUSION_ADAPT_STRUCT(
    client::employee,
    (int, age)
    (std::string, surname)
    (std::string, forename)
    (double, salary)
)
```

现在，我们将为员工编写一个解析器。 输入将采用以下形式：

```C++
employee{ age, "surname", "forename", salary }
```

Here goes:

```C++
template <typename Iterator>
struct employee_parser : qi::grammar<Iterator, employee(), ascii::space_type>
{
    employee_parser() : employee_parser::base_type(start)
    {
        using qi::int_;
        using qi::lit;
        using qi::double_;
        using qi::lexeme;
        using ascii::char_;

        quoted_string %= lexeme['"' >> +(char_ - '"') >> '"'];

        start %=
            lit("employee")
            >> '{'
            >>  int_ >> ','
            >>  quoted_string >> ','
            >>  quoted_string >> ','
            >>  double_
            >>  '}'
            ;
    }

    qi::rule<Iterator, std::string(), ascii::space_type> quoted_string;
    qi::rule<Iterator, employee(), ascii::space_type> start;
};
```