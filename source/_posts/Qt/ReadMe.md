# 学习QT

只写不符合思维习惯的，需要记忆的内容。

接下来需要看的

QEventLoop
QMetaObject
QAbstractEventDispatcher
QIODevice
Model/View Tutorial(先看这个再看Programming)
Model/View Programming

## Implicit Sharing

Many C++ classes in Qt use implicit data sharing to maximize resource usage and minimize copying. Implicitly shared classes are both safe and efficient when passed as arguments, because only a pointer to the data is passed around, and the data is copied only if and when a function writes to it, i.e., copy-on-write.
QT使用了隐式数据共享来保证最大程度的利用资源和减少复制开销。作为参数传递时，隐式共享类即安全又高效，因为仅传递了指向数据的指针，并且仅当函数写入数据时才复制数据。
