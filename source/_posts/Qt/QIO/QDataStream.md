# QDataStream

QDataStream类提供二进制数据到QIODevice的序列化。

## 详细说明

数据流是编码信息的二进制流，它与主机计算机的操作系统，CPU或字节顺序无关，都是100％的。 例如，运行Windows的Sun SPARC可以读取Windows下PC写入的数据流。

您还可以使用数据流来读取/写入未编码的原始二进制数据。 如果要“解析”输入流，请参见QTextStream。

QDataStream类实现C ++基本数据类型的序列化，例如char，short，int，char *等。更复杂的数据的序列化是通过将数据分解为基本单元来实现的。

数据流与QIODevice紧密协作。 QIODevice表示一种输入/输出介质，可以从中读取数据或将数据写入其中。 QFile类是I / O设备的示例。

示例（将二进制数据写入流）：

 

```c++
QFile file("file.dat");
file.open(QIODevice::WriteOnly);
QDataStream out(&file);   // we will serialize the data into the file
out << QString("the answer is");   // serialize a string
out << (qint32)42;        // serialize an integer
```

示例（从流中读取二进制数据）：

```c++
QFile file("file.dat");
file.open(QIODevice::ReadOnly);
QDataStream in(&file);    // read the data serialized from the file
QString str;
qint32 a;
in >> str >> a;           // extract "the answer is" and 42
```

写入流的每个项目均以预定义的二进制格式写入，该格式根据项目的类型而有所不同。 支持的Qt类型包括QBrush，QColor，QDateTime，QFont，QPixmap，QString，QVariant等。 有关支持数据流的所有Qt类型的完整列表，请参见序列化Qt数据类型。

对于整数，最好始终将其转换为Qt整数类型进行写入，然后再读回相同的Qt整数类型。 这样可以确保获得所需大小的整数，并使您与编译器和平台的差异隔离。

枚举可以通过QDataStream进行序列化，而无需手动定义流运算符。 枚举类使用声明的大小进行序列化。 

举一个例子，一个char *字符串被写为一个32位整数，该整数等于包括'\ 0'字节的字符串的长度，其后是包括'\ 0'字节的字符串的所有字符。 读取char *字符串时，将读取4个字节以创建32位长度值，然后读取char *字符串的许多字符，包括'\ 0'终止符。

初始I / O设备通常在构造函数中设置，但可以使用setDevice（）进行更改。 如果到达数据末尾（或没有设置I / O设备），则atEnd（）将返回true。

版本控制

QDataStream的二进制格式自Qt 1.0以来已经发展，并且可能会继续发展以反映Qt中所做的更改。 输入或输出复杂类型时，确保使用相同版本的流（version（））进行读写非常重要。 如果需要向前和向后兼容性，则可以在应用程序中对版本号进行硬编码：

 `stream.setVersion(QDataStream::Qt_4_0);`

如果要生成新的二进制数据格式，例如由应用程序创建的文档的文件格式，则可以使用QDataStream以可移植格式写入数据。 通常，您将编写一个简短的标头，其中包含一个魔术字符串和一个版本号，以便为将来的扩展留出空间。 例如：

```
QFile file("file.xxx");
file.open(QIODevice::WriteOnly);
QDataStream out(&file);

// Write a header with a "magic number" and a version
out << (quint32)0xA0B0C0D0;
out << (qint32)123;

out.setVersion(QDataStream::Qt_4_0);

// Write the data
out << lots_of_interesting_data;
```

Then read it in with:

然后阅读：

```c++
QFile file("file.xxx");
file.open(QIODevice::ReadOnly);
QDataStream in(&file);

// Read and check the header
quint32 magic;
in >> magic;
if (magic != 0xA0B0C0D0)
    return XXX_BAD_FILE_FORMAT;

// Read the version
qint32 version;
in >> version;
if (version < 100)
    return XXX_BAD_FILE_TOO_OLD;
if (version > 123)
    return XXX_BAD_FILE_TOO_NEW;

if (version <= 110)
    in.setVersion(QDataStream::Qt_3_2);
else
    in.setVersion(QDataStream::Qt_4_0);

// Read the data
in >> lots_of_interesting_data;
if (version >= 120)
    in >> data_new_in_XXX_version_1_2;
in >> other_interesting_data;
```

您可以选择序列化数据时要使用的字节顺序。 默认设置为big endian（MSB在前）。 将其更改为Little Endian会破坏可移植性（除非读者也更改为Little Endian）。 除非您有特殊要求，否则我们建议保留此设置。

## 读写原始二进制数据

您可能希望直接从数据流读取/写入自己的原始二进制数据。 可以使用readRawData（）将数据从流中读取到预分配的char *中。 同样，可以使用writeRawData（）将数据写入流中。 请注意，数据的任何编码/解码都必须由您完成。

一对类似的函数是readBytes（）和writeBytes（）。 它们与原始副本不同，如下所示：readBytes（）读取一个Quint32，将其作为要读取的数据的长度，然后将该字节数读取到预分配的char *中； writeBytes（）写入一个quint32，其中包含数据的长度，后跟数据。 请注意，数据的任何编码/解码（长度quint32除外）都必须由您完成。

## 读写Qt集合类

Qt容器类也可以序列化为QDataStream。 这些包括QList，QLinkedList，QVector，QSet，QHash和QMap。 流运算符被声明为类的非成员。

## 读写其他Qt类

除了此处记录的重载流运算符之外，您可能要序列化为QDataStream的任何Qt类都将具有适当的流运算符，这些流运算符声明为该类的非成员：

```c++
QDataStream &operator<<(QDataStream &, const QXxx &);
QDataStream &operator>>(QDataStream &, QXxx &);
```

例如，以下是声明为QImage类的非成员的流运算符：

```c++
QDataStream & operator<< (QDataStream& stream, const QImage& image);
QDataStream & operator>> (QDataStream& stream, QImage& image);
```

若要查看您喜欢的Qt类是否定义了类似的流运算符，请查看该类的文档页面的“相关非成员”部分。

## Using Read Transactions

当数据流在异步设备上运行时，数据块可以到达任意时间点。 QDataStream类实现了一种事务处理机制，该机制提供了使用一系列流运算符自动读取数据的功能。 例如，您可以通过使用连接到readyRead（）信号的插槽中的事务来处理套接字的不完整读取：

```c++
in.startTransaction();
QString str;
qint32 a;
in >> str >> a; // try to read packet atomically

if (!in.commitTransaction())
    return;     // wait for more data
```

如果没有收到完整的数据包，此代码会将流恢复到初始位置，此后您需要等待更多数据到达。

## Member Type Documentation

### enum QDataStream::ByteOrder

用于读取/写入数据的字节顺序。



| Constant                  | Value                  | Description                               |
| ------------------------- | ---------------------- | ----------------------------------------- |
| QDataStream::BigEndian    | QSysInfo::BigEndian    | Most significant byte first (the default) |
| QDataStream::LittleEndian | QSysInfo::LittleEndian | Least significant byte first              |

### enum QDataStream::FloatingPointPrecision

The precision of floating point numbers used for reading/writing the data. This will only have an effect if the version of the data stream is [Qt_4_6](qdatastream.html#Version-enum) or higher.

用于读取/写入数据的浮点数的精度。 仅当数据流的版本为Qt_4_6或更高版本时，此选项才有效。

Warning: The floating point precision must be set to the same value on the object that writes and the object that reads the data stream.

警告：在写入的对象和读取数据流的对象上，浮点精度必须设置为相同的值。



| Constant                     | Value | Description                                                  |
| ---------------------------- | ----- | ------------------------------------------------------------ |
| QDataStream::SinglePrecision | 0     | All floating point numbers in the data stream have 32-bit precision.数据流中的所有浮点数都具有32位精度。 |
| QDataStream::DoublePrecision | 1     | All floating point numbers in the data stream have 64-bit precision.数据流中的所有浮点数都具有64位精度。 |

### enum QDataStream::Status

This enum describes the current status of the data stream.

该枚举描述了数据流的当前状态。



| Constant                     | Value | Description                                                  |
| ---------------------------- | ----- | ------------------------------------------------------------ |
| QDataStream::Ok              | 0     | The data stream is operating normally.                       |
| QDataStream::ReadPastEnd     | 1     | The data stream has read past the end of the data in the underlying device.数据流已读取了底层设备中数据的末尾。 |
| QDataStream::ReadCorruptData | 2     | The data stream has read corrupt data.数据流读取倒损坏的数据。 |
| QDataStream::WriteFailed     | 3     | The data stream cannot write to the underlying device.数据流无法写入基础设备。 |

### Member Function Documentation

#### `QDataStream::QDataStream(const QByteArray& a)`

构造一个对字节数组a进行操作的只读数据流。 如果要写入字节数组，请使用`QDataStream（QByteArray *，int）`。
由于QByteArray不是QIODevice子类，因此在内部创建QBuffer来包装字节数组。

#### `QDataStream::QDataStream(QByteArray *a, QIODevice::OpenMode mode)`

构造对字节数组a进行操作的数据流。 该模式描述了设备的使用方式。

另外，如果您只想从字节数组中读取数据，则可以使用`QDataStream（const QByteArray＆）`。

#### `QDataStream::~QDataStream()`

析构函数将不会影响当前的I / O设备，除非它是内部I / O设备（例如QBuffer），它处理构造函数中传递的QByteArray，在这种情况下，内部I / O设备将被销毁。

#### `void QDataStream::abortTransaction()`

Aborts a read transaction.
This function is commonly used to discard the transaction after higher-level protocol errors or loss of stream synchronization.
If called on an inner transaction, aborting is delegated to the outermost transaction, and subsequently started inner transactions are forced to fail.
For the outermost transaction, discards the restoration point and any internally duplicated data of the stream. Will not affect the current read position of the stream.

中止读取的事务。
此功能通常用于在更高级别的协议错误或流同步丢失后丢弃事务。
如果在内部事务上调用，则中止将委派给最外部的事务，随后启动的内部事务将被强制失败。
对于最外部的事务，丢弃流的恢复点和任何内部重复的数据。 不会影响流的当前读取位置。

See also [startTransaction](qdatastream.html#startTransaction)(), [commitTransaction](qdatastream.html#commitTransaction)(), and [rollbackTransaction](qdatastream.html#rollbackTransaction)().

#### `bool QDataStream::atEnd() const`

如果I/O设备已到达结束位置（流或文件的末尾）或未设置I/O设备，则返回true；否则，返回true。 否则返回false。

#### `QDataStream::ByteOrder QDataStream::byteOrder() const`

返回当前字节顺序设置-大端或小端BigEndian or LittleEndian.。

See also [setByteOrder](qdatastream.html#setByteOrder)().

#### `bool QDataStream::commitTransaction()`

完成读取事务。 如果在事务处理过程中未发生任何读取错误，则返回true；否则返回true。 否则返回false。
如果在内部事务上调用，则提交将推迟到最外面的commitTransaction（），rollbackTransaction（）或abortTransaction（）调用发生为止。
否则，如果流状态指示正在读取数据的末尾，则此函数将流数据恢复到startTransaction（）调用的点。 发生这种情况时，您需要等待更多数据到达，然后再开始新事务。 如果数据流已读取损坏的数据或任何内部事务已中止，则此函数将中止该事务。
此功能在Qt 5.7中引入。

See also [startTransaction](qdatastream.html#startTransaction)(), [rollbackTransaction](qdatastream.html#rollbackTransaction)(), and [abortTransaction](qdatastream.html#abortTransaction)().

#### `void QDataStream::rollbackTransaction()`

恢复读取的事务。
当在提交事务之前检测到不完整的读取时，通常使用此功能回滚事务。
如果在内部事务上调用，则将还原委派给最外部的事务，随后启动的内部事务将被强制失败。
对于最外部的事务，将流数据还原到startTransaction（）调用的点。 如果数据流已读取损坏的数据或任何内部事务已中止，则此函数将中止该事务。
如果前面的流操作成功，则将数据流的状态设置为

#### `void QDataStream::startTransaction()`

在流上启动新的读取事务。

在读取操作序列中定义一个可恢复的点。 对于顺序设备，读取的数据将在内部复制，以便在读取不完整的情况下进行恢复。 对于随机访问设备，此功能保存流的当前位置。 调用`commitTransaction（）`，`rollbackTransaction（）`或`abortTransaction（）`完成当前事务。

事务启动后，对该函数的后续调用将使事务递归。 内部事务充当最外部事务的代理（即，向最外部事务报告读取操作的状态，这可以恢复流的位置）。

Note: Restoring to the point of the nested startTransaction() call is not supported.

**注意：**不支持还原到嵌套的startTransaction（）调用。

当事务期间发生错误时（包括内部事务失败），将从数据流中读取的操作暂停（所有后续读取操作均返回空/零值），并且随后的内部事务被强制失败。 启动新的最外部事务将从此状态恢复。 此行为使得不必分别对每个读取操作进行错误检查。

#### `QDataStream &QDataStream::writeBytes(const char *s, uint len)`

将长度说明符len和缓冲区s写入流，并返回对该流的引用。
len被序列化为quint32，后跟来自s的len个字节。 请注意，数据未编码。

#### `int QDataStream::writeRawData(const char *s, int len)`

将s的len个字节写入流中。 返回实际写入的字节数，如果错误则返回-1。 数据未编码。