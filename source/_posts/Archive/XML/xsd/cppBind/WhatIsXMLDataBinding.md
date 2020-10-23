# What is XML Data Binding?

## Business Overview

XML Data Binding simplifies the process of reading, processing, and writing XML data from your application. All the necessary code is automatically generated from your XML vocabulary description (XML Schema) and is specific to your problem domain.
XML数据绑定简化了从应用程序读取，处理和写入XML数据的过程。 所有必需的代码都是从XML词汇表描述（XML Schema）自动生成的，并且特定于您的问题领域。

## Technical Overview

The technical side of XML Data Binding is straightforward: instead of manually extracting the data stored in XML using APIs such as DOM or SAX, automate this task by presenting the data as C++ class objects which model the vocabulary you are working with. For example, instead of looking for an element named `"age"` and then converting its text content to an `int`, you simply call a function called `age` which returns the age as an integer.
XML数据绑定的技术方面很简单：与其使用DOM或SAX之类的API手动提取XML中存储的数据，不如通过将数据显示为C ++类对象（可对正在使用的词汇表进行建模）来自动执行此任务。 例如，您无需调用名为“ age”的元素，然后将其文本内容转换为“ int”，而只需调用一个名为“ age”的函数即可，该函数将年龄作为整数返回。

# Why Use XML Data Binding?为什么要使用XML数据绑定？

## Business Reasons

- **Faster time to market.** Instead of having engineers spend months or years on the boiler plate code, let them concentrate on what makes your product unique—the business logic.
  **加快产品上市速度。**不用让工程师花数月或数年时间来制作样板代码，而应让他们专注于使您的产品与众不同的事物-业务逻辑。
- **Minimize risks.** The generated code is more reliable and has fewer bugs thanks to the repeated and widespread use. Reduction in the amount and simplification of the business logic code engineers have to write by hand further reduces the risks.
  **降低风险**。 由于重复使用和广泛使用，生成的代码更可靠，错误更少。 减少数量和简化业务逻辑代码工程师必须手工编写的代码进一步降低了风险。
- **Reduce costs.** Automatic code generation is significantly cheaper than manual development. Furthermore, simplified business logic implementation means lower maintenance and development costs.
  **降低成本。**自动代码生成比手动开发便宜得多。 此外，简化的业务逻辑实施意味着更低的维护和开发成本。

## Technical Reasons

- **Ease of use.** The generated code provides parsing and serialization functions that automatically converts the object model to/from XML. As a result, you are shielded from the intricacies of reading and writing XML.
  **易于使用。**生成的代码提供了解析和序列化功能，可自动将对象模型转换为XML。 因此，您可以避免读写XML的复杂性。
- **Natural representation.** You work with the XML data using your domain vocabulary instead of generic elements, attributes, and text.
  **自然表示。**您可以使用域词汇而不是通用元素，属性和文本来处理XML数据。
- **Static typing.** The generated object model is statically typed which helps catch errors at compile-time rather than at run-time.
  **静态类型。**生成的对象模型是静态类型的，这有助于在编译时而不是运行时捕获错误。
- **Concise code.** Thanks to the object representation, your business logic implementation is simpler and thus easier to read and understand.
  **简洁的代码。**由于有了对象表示，您的业务逻辑实现更加简单，因此更易于阅读和理解。
- **Maintainability.** Automatic code generation minimizes the effort needed to adapt your application to changes in the document structure. Thanks to static typing, the C++ compiler will pin-point the places in your code that need to be changed.
  **可维护性。**自动代码生成可最大程度地减少使应用程序适应文档结构更改所需的工作。 多亏了静态类型，C ++编译器将查明代码中需要更改的位置。

# Why Use CodeSynthesis XSD?为什么要使用CodeSynthesis XSD？

## Business Reasons

- **Open-source.** CodeSynthesis XSD is dual-licensed under the GPL and a proprietary license. Even if you choose to use the proprietary license you still get full source code, including the compiler. This means no vendor lock-in as well as the ability to customize the compiler in-house or by contracting a third party. See [Licensing](https://www.codesynthesis.com/products/xsd/license.xhtml) for more information.
  **开源。** CodeSynthesis XSD在GPL和专有许可下具有双重许可。 即使您选择使用专有许可证，您仍然可以获得完整的源代码，包括编译器。 这意味着没有供应商锁定，也没有内部或通过与第三方签订合同来定制编译器的能力。 有关更多信息，请参见[许可](https://www.codesynthesis.com/products/xsd/license.xhtml)。
- **Cross-platform.** No matter which platforms and compilers you are using or will be using in the future CodeSynthesis XSD supports them. See [Supported Platforms and Compilers](https://www.codesynthesis.com/products/xsd/platforms.xhtml) for details.
  **跨平台。**无论您正在使用或将来使用哪种平台和编译器，CodeSynthesis XSD都将支持它们。 有关详细信息，请参见[支持的平台和编译器](https://www.codesynthesis.com/products/xsd/platforms.xhtml)。
- **Proven.** CodeSynthesis XSD has been successfully used in the industries with the most stringent quality and reliability requirements, including aerospace, defense, and telecommunications. See [CodeSynthesis XSD Customers](https://www.codesynthesis.com/products/xsd/customers.xhtml) list for more information.
  **经过验证。**CodeSynthesis XSD已成功用于质量和可靠性要求最严格的行业，包括航空航天，国防和电信。 有关更多信息，请参见[CodeSynthesis XSD客户](https://www.codesynthesis.com/products/xsd/customers.xhtml)列表。
- **Exceptional support.** Support is provided by the same full-time engineering team that does ongoing maintenance and development. This ensures that you get the best advise from the source. Check our [support mailing list archives](https://www.codesynthesis.com/pipermail/xsd-users/) for real support stories.
  由进行日常维护和开发的同一个专职工程团队提供支持。 这样可以确保您从源头获得最佳建议。 查看我们的[支持邮件列表档案](https://www.codesynthesis.com/pipermail/xsd-users/)，以获得真实的支持案例。
- **XML and C++ expertise.** Tap into our XML processing and cross-platform C++ knowledge to help you develop your application.
  利用我们的XML处理和跨平台C ++知识来帮助您开发应用程序。

## Technical Reasons

- **Stream-oriented processing model.** Unlike other products, CodeSynthesis XSD supports event-driven, stream-oriented XML Data Binding in addition to the traditional in-memory model. This allows you, for example, to handle XML documents that are too large to fit into memory or perform immediate processing as parts of the document become available.
  **面向流的处理模型。**与其他产品不同，CodeSynthesis XSD除支持传统的内存模型外，还支持事件驱动的，面向流的XML数据绑定。 例如，这使您可以处理太大而无法放入内存的XML文档，或者在文档的某些部分可用时立即执行处理。
- **Elegant and portable C++ mappings.** We took great care in designing our XML Schema to C++ mappings to ensure that they are portable and enjoyable to work with. See what real users [have to say](https://www.codesynthesis.com/products/xsd/quotes.xhtml) about CodeSynthesis XSD.
  **优雅且可移植的C ++映射。**我们在设计从XML Schema到C ++的映射时要格外小心，以确保它们可移植且令人愉快。 查看真正的用户[必须说](https://www.codesynthesis.com/products/xsd/quotes.xhtml)关于CodeSynthesis XSD的内容。
- **Comprehensive XML Schema feature coverage.** CodeSynthesis XSD includes complete support of the W3C XML Schema specification for validation and supports all important and widely used features for data binding. This is further verified by a large body of [complex, real-world schemas](http://wiki.codesynthesis.com/Schemas) that were successfully used with XSD.
  **全面的XML Schema功能覆盖。** CodeSynthesis XSD完全支持W3C XML Schema规范以进行验证，并支持所有重要且广泛使用的数据绑定功能。 XSD已成功使用大量的[复杂的，现实世界的模式](http://wiki.codesynthesis.com/Schemas)，这进一步证明了这一点。
- **High performance and small footprint.** CodeSynthesis XSD uses one of the fastest validating XML parsers for the in-memory mapping. The stream-oriented mapping goes one step further and performs highly-optimized validation and event dispatching in the generated code which results in 2-10 times faster parsing and validation compared to general-purpose validating XML parsers.
  **高性能和占用空间小。** CodeSynthesis XSD使用最快的验证XML解析器之一进行内存中映射。 面向流的映射更进一步，并在生成的代码中执行高度优化的验证和事件分发，与通用验证XML解析器相比，解析和验证速度快2到10倍。
- **Easy integration.** CodeSynthesis XSD can be seamlessly used with a large number of libraries, including the C++ standard library, Boost, XDR, and ACE, as well as build systems and IDEs, including Visual Studio, QtCreator, Eclipse, make, and CMake.

# Case Study

[RUAG Electronics](http://www.ruag.com/ruag/juice?pageID=132102), a subsidiary of the [RUAG Holding](http://www.ruag.com/), is a European expert in high-precision technologies for the aerospace and defense sectors as well as for the automotive, semiconductor and mechanical engineering industries. The company has selected CodeSynthesis XSD for its [Simulation & Training](http://www.ruag.com/ruag/juice?pageID=130859) systems.

The company's needs for XML Data Binding amounted to 50,000 lines of the generated code. By using CodeSynthesis XSD, RUAG Electronics saved an estimated 24 person-months of effort, at least EUR 85,000 in development costs, and reduced time to market by at least 12 months.