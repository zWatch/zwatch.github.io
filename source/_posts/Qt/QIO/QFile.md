The `QFile` class provides an interface for reading from and writing to files. [More...](#details)

`QFile`是一种用于读写文本、二进制文件和资源的I/O设备。`QFile`可以单独使用，或者更方便地与`QTextStream`或`QDataStream`一起使用。

**坑爹**:

  文件名通常在构造函数中传递，但可以随时使用`setFileName()`进行设置。无论操作系统如何，`QFile`都希望文件分隔符为“/”。不支持使用其他分隔符（例如“\”）。

  可以使用exists()检查文件是否存在，并使用remove()删除文件。（更高级的文件系统相关操作由`QFileInfo`和`QDir`提供。）

  该文件用`open()`打开，用`close()`关闭，然后用`flush()`刷新。 通常使用`QDataStream`或`QTextStream`读写数据，但是您也可以调用`QIODevice`继承的函数`read()`，`readLine()`，`readAll()`，`write()`。 `QFile`还继承了`getChar()`，`putChar()`和`ungetChar()`，它们一次只工作一个字符。



文件的大小由`size()`返回。 您可以使用`pos()`获取当前文件位置，或使用`seek()`移至新文件位置。 如果到达文件末尾，则`atEnd()`返回true。

## Reading Files Directly

​    下面的示例逐行读取文本文件：

```C++
      QFile file("in.txt");
      if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
         return;

     while (!file.atEnd()) {
         QByteArray line = file.readLine();
         process_line(line);
     }
```

传递给`open()`的`QIODevice :: Text`标志告诉Qt将Windows样式的行终止符（“ \ r \ n”）转换为C ++样式的终止符（“ \ n”）。

 **默认情况下，`QFile`假定为二进制，即，它不对文件中存储的字节执行任何转换。**

## Using Streams to Read Files



下一个示例使用QTextStream逐行读取文本文件：

```C++
     QFile  file("in.txt");
     if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
         return;

     QTextStream in(&file);
     while (!in.atEnd()) {
         QString line = in.readLine();
         process_line(line);
     }
```



`QTextStream`负责将存储在磁盘上的8位数据转换为16位`Unicode QString`。 默认情况下，它假定使用用户系统的本地8位编码（例如，在大多数基于unix的操作系统上为UTF-8；有关详细信息，请参见`QTextCodec :: codecForLocale()`。 可以使用`QTextStream :: setCodec()`进行更改。

要编写文本，我们可以使用`operator <<()`，将其重载以在左侧获取`QTextStream`并在右侧获取各种数据类型（包括QString）：

```C++
QFile file("out.txt");
if (!file.open(QIODevice::WriteOnly | QIODevice::Text))
    return;

QTextStream out(&file);
out << "The magic number is: " << 49 << "\n";
```

[QDataStream](qdatastream.html) is similar, in that you can use operator<<() to write data and operator>>() to read it back. See the class documentation for details.

QDataStream与之类似，您可以使用`operator <<()`写入数据，并使用`operator >>()`读回数据。 有关详细信息，请参见类文档。

使用QFile，QFileInfo和QDir通过Qt访问文件系统时，可以使用Unicode文件名。 在Unix上，这些文件名被转换为8位编码。 如果要使用标准C ++ API（`<cstdio>`或`<iostream>`）或平台特定的API来访问文件而不是QFile，则可以使用`encodeName()`和`encodeName()`函数在Unicode文件名和8-bit文件名之间进行转换。

**NOTE**

在Unix上，有一些特殊的系统文件（例如/ proc中的文件），对于这些文件，`size()`总是返回0，但是您仍然可以从该文件中读取更多数据； 数据是直接响应您调用`read()`而生成的。 但是，在这种情况下，您不能使用`atEnd()`确定是否还有更多数据要读取（因为`atEnd()`对于声称大小为0的文件将返回true）。 相反，您应该重复调用`readAll()`，或重复调用`read()`或`readLine()`直到无法读取更多数据为止。 下一个示例使用QTextStream逐行读取/ proc / modules：

```C++
QFile file("/proc/modules");
if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
    return;

QTextStream in(&file);
QString line = in.readLine();
while (!line.isNull()) {
    process_line(line);
    line = in.readLine();
} 
```

## Signals

**NOTE**

与其他QIODevice实现（例如QTcpSocket）不同，QFile不会发出aboutToClose()，bytesWritten()或readyRead()信号。 此实现细节表示QFile不适合读写某些类型的文件，例如Unix平台上的设备文件。

## Platform Specific Issues

**NOTE**

在类似Unix的系统和Windows上，文件权限的处理方式有所不同。 在类Unix系统上的不可写目录中，无法创建文件。 在Windows上，情况并非总是如此，例如，“我的文档”目录通常是不可写的，但仍可以在其中创建文件。

Qt对文件权限的理解是有限的，这尤其会影响QFile :: setPermissions()函数。 在Windows上，Qt将仅设置旧版只读标志，并且仅在未传递Write *标志时设置该标志。 Qt不会操纵访问控制列表（ACL），这使得该功能对NTFS卷几乎没有用。 对于使用VFAT文件系统的USB记忆棒，它可能仍然有用。 POSIX ACL也不被操纵。

## Member Type Documentation

## 成员函数文档

### `bool QFile::copy(const QString& newName)`

将当前由fileName()指定的文件复制到名为newName的文件中。 如果成功，则返回true；否则，返回false。 否则返回false。
请注意，如果已经存在名称为newName的文件，则copy()返回false（即QFile不会覆盖它）。

源文件在复制之前已关闭。(不太理解这句话)

### `[static] bool QFile::copy(const QString& fileName, const QString & newName)`

这是一个重载函数。
将文件fileName复制到newName。 如果成功，则返回true；否则，返回false。 否则返回false。
如果已经存在名称为newName的文件，则`copy()`返回false（即QFile不会覆盖它）。

See also [rename](qfile.html#rename)().

### `[static] QString QFile::decodeName(const QByteArray& localFileName)`

这与使用localFileName的`QFile :: encodeName()`相反。

### `[static] QByteArray QFile::encodeName(const QString& fileName)`

将fileName转换为由用户的语言环境确定的本地8位编码。 这足以满足用户选择的文件名。 硬编码到应用程序中的文件名只能使用7位ASCII文件名字符。

### `bool QFile::link(const QString& linkName)`

`[static] bool QFile::link(const QString& fileName, const QString& linkName)`

创建一个名为linkName的链接，该链接指向fileName()当前指定的文件。 什么是链接取决于底层文件系统（在Windows上是快捷方式还是在Unix上是符号链接）。 如果成功，则返回true；否则，返回false。 否则返回false。

(应该是软连接)

此功能不会覆盖文件系统中已经存在的实体； 在这种情况下，link()将返回false并将error()设置为返回RenameError。

**Note:** To create a valid link on Windows, *linkName* must have a .lnk file extension.

### `bool QFile::moveToTrash()`

`[static] bool QFile::moveToTrash(const QString& fileName, QString *pathInTrash = nullptr)`

将fileName()指定的文件移动到垃圾箱。 如果成功，则返回true，并将fileName()设置为可在回收站中找到文件的路径； 否则返回false。

**Note：**	在系统API不报告垃圾桶中文件路径的系统上，一旦移动了文件，pathInTrash将设置为空字符串。 在没有垃圾桶的系统上，此函数始终返回false。

### `[override virtual] bool QFile::open(QIODevice::OpenMode mode)`

重新实现：`QIODevice :: open（QIODevice :: OpenMode mode）`。
使用OpenMode模式打开文件，如果成功，则返回true；否则，返回true。 否则为假。
模式必须为`QIODevice :: ReadOnly`，`QIODevice :: WriteOnly`或`QIODevice :: ReadWrite`。 它还可能具有其他标志，例如`QIODevice :: Text`和`QIODevice :: Unbuffered`。

### `bool QFile::open(FILE *fh, QIODevice::OpenMode mode, QFileDevice::FileHandleFlags handleFlags = DontCloseHandle)`



在给定模式下打开现有文件句柄fh。 handleFlags可用于指定其他选项。 如果成功，则返回true；否则，返回false。 否则返回false。



**这个说的也好模糊啊**

Example:

```C++
 #include <stdio.h>
 void printError(const char* msg)
 {
     QFile file;
     file.open(stderr, QIODevice::WriteOnly);
     file.write(msg, qstrlen(msg));        // write to stderr
     file.close();
 }
```

使用此函数打开QFile时，`close()`的行为由`AutoCloseHandle`标志控制。 如果指定了`AutoCloseHandle`，并且此函数成功执行，则调用`close()`将关闭采用的句柄。 否则，`close()`实际上不会关闭文件，而只会刷新它。

**WARING**

- 如果fh未引用常规文件（例如stdin，stdout或stderr），则可能无法执行seek()。 在这些情况下，size()返回0。 有关更多信息，请参见QIODevice :: isSequential()。
- 由于此函数是在不指定文件名的情况下打开文件，因此无法将此QFile与QFileInfo一起使用。

**Note for the Windows Platform**

访问文件和其他随机访问设备时，必须以二进制模式打开fh（即，模式字符串必须包含“ b”，例如“ rb”或“ wb”）。 如果将`QIODevice :: Text`传递给mode，Qt将转换行尾字符。 顺序设备（例如stdin和stdout）不受此限制的影响。

您需要启用对控制台应用程序的支持，以便在控制台上使用stdin，stdout和stderr流。 为此，请将以下声明添加到应用程序的项目文件中：

 `CONFIG += console`



### `bool QFile::open(int fd, QIODevice::OpenMode mode, QFileDevice::FileHandleFlags handleFlags = DontCloseHandle)`

//以给定模式打开现有文件描述符fd。 handleFlags可用于指定其他选项。 如果成功，则返回true；否则，返回false。 否则返回false。

使用此函数打开QFile时，`close()`的行为由`AutoCloseHandle`标志控制。 如果指定了`AutoCloseHandle`，并且此函数成功执行，则调用`close()`将关闭采用的句柄。 否则，`close()`实际上不会关闭文件，而只会刷新它。

使用此功能打开的QFile会自动设置为原始模式。 这意味着文件输入/输出功能很慢。 如果遇到性能问题，则应尝试使用其他打开功能之一。

**Waring：**如果fd不是常规文件，例如，它是0（stdin），1（stdout）或2（stderr），则可能无法执行seek()。 在这些情况下，size()返回0。有关更多信息，请参见QIODevice :: isSequential()。

**Waring：**由于此函数在未指定文件名的情况下打开文件，因此您不能将此QFile与QFileInfo一起使用。