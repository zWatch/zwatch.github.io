# C++/Tree: In-Memory XML Data Binding

The C++/Tree mapping generates C++ classes that represent data types defined in XML Schema, a set of parsing functions that convert XML documents to a tree-like in-memory object model, and a set of serialization functions that convert the object model back to XML. For an introduction to the C++/Tree mapping, refer to the [Hello World Example](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/#2) from the [C++/Tree Mapping Getting Started Guide](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/).
C ++ / Tree映射生成C ++类，这些类表示XML Schema中定义的数据类型，将XML文档转换为树状内存中对象模型的一组解析函数以及将对象模型转换为以下对象的一组序列化函数： XML。 有关C ++ / Tree映射的介绍，请参阅[Hello World示例](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/#2) 中的 [C ++ / Tree映射入门指南](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/)。

# Features

- Robust, clean and compact C++ standard library-based language mapping; you don't need to learn much if you know how to use `std::string` and `std::vector`.
  健壮，干净和紧凑的基于C ++标准库的语言映射； 如果您知道如何使用`std :: string`和`std :: vector`，则无需学习太多。
- Support for C++11 and C++98/03. 支持C ++ 11和C ++ 98/03。
- Configurable base character type: `char` or `wchar_t`. 可配置的基本字符类型：“ char”或“ wchar_t”。
- Configurable application character encoding (UTF-8, ISO-8859-1, UTF-16, UTF-32, local code page).可配置的应用程序字符编码（UTF-8，ISO-8859-1，UTF-16，UTF-32，本地代码页）。
- Natural mapping for all W3C XML Schema built-in types: `xsd:boolean` to `bool`, `xsd:string` to `std::string`, etc.所有W3C XML Schema内置类型的自然映射：xsd：boolean到bool，xsd：string到std：string等。
- Serialization of the object model back to DOM and XML with support for streaming.
  通过流支持将对象模型序列化回DOM和XML。
- Generation of documentation comments in the Doxygen format that include documentation extracted from schemas.
  生成Doxygen格式的文档注释，其中包括从架构中提取的文档。
- Automatic generation of `std::ostream` insertion operators (`operator<<`, useful for debugging and tracing).
  自动生成“ std :: ostream”插入运算符（“ operator <<”，对于调试和跟踪很有用）。
- Automatic generation of comparison operators.
  自动生成比较运算符
- Customization of the generated code including XML Schema built-in types.
  定制包括XML Schema内置类型在内的生成代码。
- Mapping of `xsd:enumeration` to C++ `enum`.
  xsd：enumeration到C ++ enum的映射。
- Customizable XML Schema namespace to C++ namespace mapping.-可自定义的XML模式名称空间到C ++名称空间的映射。
- Customizable identifier naming convention in the generated code.
  生成的代码中的可自定义标识符命名约定。
- Support for statically-typed ID/IDREF cross-referencing.
  支持静态类型的ID / IDREF交叉引用。
- Support for XML Schema polymorphism (`xsi:type` and substitution groups).
  支持XML模式多态性（`xsi：type`和替换组）。
- Support for accessing/modifying wildcard content (`xsd:any/xsd:anyAttribute`) as DOM fragments which can also be used to create/serialize object models.
  支持作为DOM片段访问/修改通配符内容（`xsd：any / xsd：anyAttribute`），也可以用于创建/序列化对象模型。
- Support for accessing/modifying `xsd:anyType` and `xsd:anySimpleType` as DOM and text fragments, respectively.
  支持将xsd：anyType和xsd：anySimpleType分别访问/修改为DOM和文本片段。
- Support for preserving strict content order, including mixed content.
  支持保留严格的内容顺序，包括混合内容。
- Support for uniform parsing and serialization of XML documents with varying root elements.
  支持使用不同的根元素统一解析和序列化XML文档。
- Support for default and fixed values.
  支持默认值和固定值。
- Support for stream-oriented, partially in-memory XML processing.
  支持面向流的部分内存XML处理。
- Option to maintain association with underlying DOM nodes.
- Support for locating object model nodes with XPath queries.
- Extensible, high-performance serialization to compact binary formats for storage or over-the-wire transfer (RPC XDR, ACE CDR streams, and Boost serialization are supported out of the box, custom formats and APIs can be easily added).
- Automatic morphing of anonymous types into named ones with support for name customization.
- Support for schema importing, inclusion and chameleon-style inclusion.
- Support for the file-per-schema and file-per-type compilation models.
- Support for `make` dependency generation.

The following diagram shows the main integration areas of the C++/Tree mapping and related technologies:

![C++/Tree Mapping](https://www.codesynthesis.com/products/xsd/c++/tree/cxx-tree.png)

# Documentation

| [C++/Tree Mapping Getting Started Guide](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/) | An introduction to the C++/Tree mapping with examples. Also available in [PDF](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/cxx-tree-guide.pdf) and [PostScript](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/guide/cxx-tree-guide.ps). |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [C++/Tree Mapping User Manual](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/manual/) | A comprehensive description of the C++/Tree mapping, including the parsing and serialization mechanisms. Also available in [PDF](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/manual/cxx-tree-manual.pdf) and [PostScript](https://www.codesynthesis.com/projects/xsd/documentation/cxx/tree/manual/cxx-tree-manual.ps). |
| [XSD Compiler Command Line Manual](https://www.codesynthesis.com/projects/xsd/documentation/xsd.xhtml) | Compiler's command line interface documentation (available compiler options, etc). |
| [C++/Tree Mapping FAQ](http://wiki.codesynthesis.com/Tree/FAQ) | Answers to frequently asked questions about the C++/Tree mapping. |
| [Schema Compilation Checklist](http://wiki.codesynthesis.com/XSD/Schema_compilation_checklist) | A checklist-like run through the most commonly used XSD command line options. |
| [C++/Tree Mapping Customization Guide](http://wiki.codesynthesis.com/Tree/Customization_guide) | A guide to customizing generated and XML Schema built-in types. |
| [Using XSD with Microsoft Visual Studio](http://wiki.codesynthesis.com/Using_XSD_with_Microsoft_Visual_Studio) | Discusses various ways of integrating the XSD compiler with the Microsoft Visual Studio IDE as well as other Visual Studio-specific topics. |
| [C++/Tree Mapping Wiki Page](http://wiki.codesynthesis.com/Tree) | A resource page for the C++/Tree mapping on [Code Synthesis Wiki](http://wiki.codesynthesis.com/). |
| [XSD Wiki Page](http://wiki.codesynthesis.com/XSD)           | A resource page for XSD on [Code Synthesis Wiki](http://wiki.codesynthesis.com/). |

# Support

We provide free, best-effort technical support for XSD via the [xsd-users mailing list](https://www.codesynthesis.com/mailman/listinfo/xsd-users). Simply send an email to this mailing list with the description of a bug or a problem that you encountered. Please follow the [Posting Guidelines](https://www.codesynthesis.com/support/posting-guidelines.xhtml) to receive a prompt reply.

We also offer priority support on a commercial basis. Visit our [support page](https://www.codesynthesis.com/support/) for more information.

# Resources

| [XSD project page](https://www.codesynthesis.com/projects/xsd/) | Source code, build instructions and other information for XSD compiler developers. |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [XSD contributions](https://www.codesynthesis.com/download/xsd/contrib/) | Various third-party contributions to XSD.                    |
| [XML Data Binding in C++](http://www.artima.com/cppsource/xml_data_binding.html) | An article in the C++ Source journal introducing XML Data Binding in C++. |
| [XML Schema Part 0: Primer](http://www.w3.org/TR/xmlschema-0/) | An easily approachable description of the XML Schema facilities. It is oriented towards quickly understanding how to create schemas using the W3C XML Schema language. |