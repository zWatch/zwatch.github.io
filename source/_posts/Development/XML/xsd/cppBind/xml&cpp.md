from: https://www.codesynthesis.com/products/xsd/

# XSD: XML Data Binding for C++

CodeSynthesis XSD is an open-source, cross-platform W3C XML Schema to C++ data binding compiler. 
CodeSynthesis XSD是一个开放源代码，跨平台的W3C XML Schema到C ++数据绑定编译器。
Provided with an XML instance specification (XML Schema), it generates C++ classes that represent the given vocabulary as well as XML parsing and serialization code. 
 提供XML实例规范（XML Schema）后，它会生成代表给定词汇表的C ++类以及XML解析和序列化代码。 
You can then access the data stored in XML using types and functions that semantically correspond to your application domain rather than dealing with the intricacies of reading and writing XML:
然后，您可以使用语义上与应用程序域相对应的类型和函数来访问XML中存储的数据，而不必处理读写XML的复杂性：

```cpp
auto_ptr<Contact> c = contact ("c.xml");
cout << c->name () << ", "
     << c->email () << ", "
     << c->phone () << endl;
```

```html
<contact>
  <name>John Doe</name>
  <email>j@doe.com</email>
  <phone>555 12345</phone>
</contact>
```

The process of extracting the data from a direct representation of XML (such as DOM or SAX) and presenting it as a hierarchy of objects or events that correspond to a document vocabulary is called XML Data Binding. An XML Data Binding compiler accomplishes this by establishing a mapping or binding between XML Schema and a target programming language. For more information on why use XML Data Binding and CodeSynthesis XSD, see [Reasons to Use](https://www.codesynthesis.com/products/xsd/reasons.xhtml).
从XML的直接表示形式（例如DOM或SAX）提取数据并将其表示为与文档词汇表相对应的对象或事件层次结构的过程称为 "XML数据绑定"。 '"XML数据绑定编译器"'通过在XML Schema与目标编程语言之间建立映射或绑定来实现此目的。 有关为什么使用XML数据绑定和CodeSynthesis XSD的更多信息，请参见[使用原因](https://www.codesynthesis.com/products/xsd/reasons.xhtml)。

XSD supports two XML Schema to C++ mappings: in-memory [C++/Tree](https://www.codesynthesis.com/products/xsd/c++/tree/) and stream-oriented [C++/Parser](https://www.codesynthesis.com/products/xsd/c++/parser/). The C++/Tree mapping represents the information stored in XML documents as a tree-like, in-memory object model. C++/Parser is a new, SAX-like mapping which represents the data stored in XML as a hierarchy of vocabulary-specific parsing events. The following table summarizes key advantages of the two C++ bindings: 
XSD支持两种XML Schema到C ++的映射：内存[C ++ / Tree](https://www.codesynthesis.com/products/xsd/c++/tree/)和面向流的[C ++ / Parser](https:/ /www.codesynthesis.com/products/xsd/c++/parser/)。 "C ++ / Tree映射" 将存储在XML文档中的信息表示为树状的内存中对象模型。 "C ++ / Parser" 是一种类似于SAX的新映射，它将XML中存储的数据表示为特定词汇的解析事件的层次结构。 下表总结了两个C ++绑定的主要优点：

| C++/Tree                                                     | C++/Parser                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Ready to use type system for in-memory representation<br />现成的类型系统用于内存表示 | Fit into existing type system by constructing your own in-memory representation<br />通过构建自己的内存表示形式来适应现有的类型系统 |
| Complete XML document view and referential integrity<br />完整的XML文档视图和参照完整性 | Perform immediate processing as parts of the document become available (streaming)<br />在文档的一部分可用时（流式处理）立即执行处理 |
| Optional association with underlying DOM nodes<br />与基础DOM节点的可选关联 | Handle XML documents that are too large to fit into memory<br />处理太大而无法放入内存的XML文档 |
| Additional features: serialization back to DOM or XML, support for ID/IDREF cross-referencing, etc.<br />附加功能：序列化回DOM或XML，支持ID / IDREF交叉引用等。 | Small footprint, including code size and runtime memory consumption<br />占用空间小，包括代码大小和运行时内存消耗 |

Compared to APIs such as DOM and SAX, XML data binding allows you to access the data in XML documents using your domain vocabulary instead of generic elements, attributes, and text. Static typing helps catch errors at compile-time rather than at run-time. Automatic code generation saves time and minimizes the effort needed to adapt your applications to changes in the document structure.
与DOM和SAX等API相比，XML数据绑定使您可以使用域词汇而不是通用元素，属性和文本来访问XML文档中的数据。 静态类型有助于在编译时而不是运行时捕获错误。 自动代码生成可以节省时间，并最大程度地减少使应用程序适应文档结构更改所需的工作。

The following two examples show the amount and complexity of code needed to access the information in the above XML using generic C++ APIs compared to C++ bindings:
以下两个示例显示了与C ++绑定相比，使用通用C ++ API访问上述XML中的信息所需的代码量和复杂性：

```cpp
// DOM

DOMElement* c = ...
DOMNodeList* l;

l = c->getElementsByTagName ("name");
DOMNode* name = l->item (0);

l = c->getElementsByTagName ("email");
DOMNode* email = l->item (0);

l = c->getElementsByTagName ("phone");
DOMNode* phone = l->item (0);

cout << name->getTextContent () << ", "
     << email->getTextContent () << ", "
     << phone->getTextContent () << endl;
```

```c++
// XML Binding: C++/Tree

Contact c = ...

cout << c.name () << ", "
     << c.email () << ", "
     << c.phone () << endl;
```



```cpp
// SAX

class ContactParser: ...
{
  virtual void
  endElement (const string& name)
  {
    if (name == "name")
      cout << ", "
    else if (name == "email")
      cout << ", "
    else if (name == "phone")
      cout << endl;
  }

  virtual void
  characters (const string& s)
  {
    cout << s;
  }
};
```

```c++
// XML Binding: C++/Parser

class ContactParser: ...
{
  virtual void
  name (const string& n)
  {
    cout << n << ", ";
  }

  virtual void
  email (const string& e)
  {
    cout << e << ", ";
  }

  virtual void
  phone (const string& p)
  {
    cout << p << endl;
  }
};
```

# Features

- **Elegant C++ Mappings and Portable Generated Code** - We took great care in designing our XML Schema to C++ mappings to ensure that they are simple, intuitive and enjoyable to work with. We also made sure that our generated code is portable across a wide range of operating systems and C++ compilers. See [Supported Platforms and Compilers](https://www.codesynthesis.com/products/xsd/platforms.xhtml) for more information.
  优雅的C ++映射和可移植生成的代码 - 我们在设计XML Schema到C ++映射时非常谨慎，以确保它们简单，直观且易于使用。 我们还确保生成的代码可移植到各种操作系统和C ++编译器中。 有关更多信息，请参见支持的平台和编译器。

- **In-Memory and Stream-Oriented Processing Models** - Unlike other products, XSD supports event-driven, stream-oriented XML data binding in addition to the in-memory model.
  **在内存里和面向流的处理模型** - 与其他产品不同，XSD除了支持内存中模型外，还支持事件驱动的，面向流的XML数据绑定。

- **Comprehensive XML Schema Feature Coverage** - XSD includes complete support of the W3C XML Schema specification for validation and supports all important and widely used features for data binding. Many [complex, real-world schemas](http://wiki.codesynthesis.com/Schemas) have been successfully compiled by XSD.
  全面的XML Schema功能覆盖范围-XSD完全支持W3C XML Schema规范以进行验证，并支持所有重要且广泛使用的数据绑定功能。 XSD已成功编译了许多复杂的实际模式。

- Easy Integration 轻松整合

   

  \- The following features make it easy to start using XSD in your application:
  以下功能使您可以轻松地在应用程序中开始使用XSD：

  - Header-only runtime library 仅标头的运行时库
  - Customization of the generated C++ code 自定义生成的C ++代码
  - Industry-standard underlying XML parsers: Xerces-C++ and Expat 行业标准的基础XML解析器：Xerces-C ++和Expat
  - Generated classes are compatible with a wide range of algorithms from the C++ standard library and Boost
    生成的类与C ++标准库和Boost中的多种算法兼容
  - Binary serialization to XDR, Boost, and ACE CDR streams
    二进制序列化到XDR，Boost和ACE CDR流
  - Uniform compiler interface across all supported platforms
    跨所有支持平台的统一编译器接口
  - Easy [integration](http://wiki.codesynthesis.com/XSD) with existing build systems and IDEs: Visual Studio, QtCreator, Eclipse, GNU make, CMake, etc.
    与现有的构建系统和IDE轻松进行[集成](http://wiki.codesynthesis.com/XSD)：Visual Studio，QtCreator，Eclipse，GNU make，CMake等。

- Open-Source 开源

   

  \- The compiler and the runtime library are available with full source code under the terms of the GPL. We also made a special exception to the terms and conditions of the GPL which allows you to use the runtime library and the generated code in a wide range of open-source software. See
  根据GPL的条款，编译器和运行时库随完整的源代码一起提供。 我们还对GPL的条款和条件作了特殊的例外，该条款允许您在各种开源软件中使用运行时库和生成的代码。 看到

   

  XSD Licensing Information 许可信息

   

  for details. With the open-source model come the following benefits:
  有关详细信息。 使用开源模型可以带来以下好处：

  - No vendor lock-in 没有供应商锁定
  - You have the ability to customize the compiler in-house
  - Additional testing and feedback from the open-source community. For example, using bug reports we built a large repository of real-world schemas which we use for regression testing
  - Build as much of your application as necessary before making a commitment

  Download

   

  and try our complete product for as long as necessary (no registration required).

- **Simple Proprietary Licensing** - We offer affordable and convenient proprietary licenses for customers who wish to stay closed-source. See [XSD Licensing Information](https://www.codesynthesis.com/products/xsd/license.xhtml) for details.

- **Cross-Platform** - Available for 6 major operating systems on 7 different CPU architectures and tested with 16 C++ compiler variants. See [Supported Platforms and Compilers](https://www.codesynthesis.com/products/xsd/platforms.xhtml) for details.

- **Community and Priority Support** - We provide free, best-effort community technical support via the [xsd-users mailing list](https://www.codesynthesis.com/mailman/listinfo/xsd-users). We also offer priority support on a commercial basis. Visit our [Support page](https://www.codesynthesis.com/support/) for more information.

