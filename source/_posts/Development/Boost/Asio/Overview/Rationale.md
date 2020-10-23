### [Rationale](https://think-async.com/Asio/asio-1.18.0/doc/asio/overview/rationale.html)

Most programs interact with the outside world in some way, whether it be via a file, a network, a serial cable, or the console. Sometimes, as is the case with networking, individual I/O operations can take a long time to complete. This poses particular challenges to application development.
大多数程序都以某种方式与外界交互，无论是通过文件，网络，串行电缆还是控制台。 有时，与网络情况一样，单个I / O操作可能需要很长时间才能完成。 这对应用程序开发提出了特殊的挑战。

Asio provides the tools to manage these long running operations, without requiring programs to use concurrency models based on threads and explicit locking.
Asio提供了用于管理这些长时间运行的操作的工具，而无需程序使用基于线程和显式锁定的并发模型。

The Asio library is intended for programmers using C++ for systems programming, where access to operating system functionality such as networking is often required. In particular, Asio addresses the following goals:
Asio库供使用C ++进行系统编程的程序员使用，该系统通常需要访问诸如网络之类的操作系统功能。 特别是，Asio解决了以下目标：

- **Portability.** The library should support a range of commonly used operating systems, and provide consistent behaviour across these operating systems.**可移植性。**库应支持一系列常用操作系统，并在这些操作系统之间提供一致的行为。
- **Scalability.** The library should facilitate the development of network applications that scale to thousands of concurrent connections. The library implementation for each operating system should use the mechanism that best enables this scalability.**可伸缩性。**该库应促进可扩展到数千个并发连接的网络应用程序的开发。 每个操作系统的库实现应使用最能实现此可伸缩性的机制。
- **Efficiency.** The library should support techniques such as scatter-gather I/O, and allow programs to minimise data copying.**效率。**库应支持分散收集I / O等技术，并允许程序最大程度地减少数据复制。
- **Model concepts from established APIs, such as BSD sockets.** The BSD socket API is widely implemented and understood, and is covered in much literature. Other programming languages often use a similar interface for networking APIs. As far as is reasonable, Asio should leverage existing practice.**来自已建立的API的模型概念，例如BSD套接字。**BSD套接字API得到了广泛的实现和理解，并且在许多文献中都有涉及。 其他编程语言通常将类似的接口用于网络API。 在合理的范围内，Asio应该利用现有做法。
- **Ease of use.** The library should provide a lower entry barrier for new users by taking a toolkit, rather than framework, approach. That is, it should try to minimise the up-front investment in time to just learning a few basic rules and guidelines. After that, a library user should only need to understand the specific functions that are being used.**使用方便。**该库应采用工具包而非框架方法，从而为新用户提供一个较低的入门障碍。 就是说，它应该在学习一些基本规则和准则的情况下，设法及时减少前期投资。 之后，库用户只需要了解所使用的特定功能。
- **Basis for further abstraction.** The library should permit the development of other libraries that provide higher levels of abstraction. For example, implementations of commonly used protocols such as HTTP.**进一步抽象的基础。**库应允许开发提供更高抽象级别的其他库。 例如，常用协议（例如HTTP）的实现。

Although Asio started life focused primarily on networking, its concepts of asynchronous I/O have been extended to include other operating system resources such as serial ports, file descriptors, and so on.
尽管Asio最初的生活主要集中在网络上，但其异步I/O的概念已扩展到包括其他操作系统资源，例如串行端口，文件描述符等。

Copyright © 2003-2020 Christopher M. KohlhoffDistributed under the Boost Software License, Version 1.0. (See accompanying file LICENSE_1_0.txt or copy at [here](http://www.boost.org/LICENSE_1_0.txt))
