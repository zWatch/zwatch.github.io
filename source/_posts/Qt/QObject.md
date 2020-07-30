#基础属性
#Event
` virtual bool event(QEvent* e) `
当你识别并处理该事件时返回true，其它不会再得到该事件；  
确保你调用了父类函数如果你没处理这个事件。  

`bool QObject::eventFilter(QObject *watched, QEvent *event)`
`void installEventFilter(QObject *filterObj)`
`void QObject::removeEventFilter(QObject *obj)`
事件筛选器，可以过滤某些事件，如果需要过滤返回true。需要注册到类才能生效
比如`installEventFilter(this)`参数也可以是别的类。

#Children/Parent
```C++
template <typename T> T QObject::findChild(const QString &name = QString(), Qt::FindChildOptions options = Qt::FindChildrenRecursively) const`
`QList<T> findChildren(const QString &name = QString(),Qt::FindChildOptions options = Qt::FindChildrenRecursively) const`
`QList<T> findChildren(const QRegularExpression &re, Qt::FindChildOptions options = Qt::FindChildrenRecursively) const
```

```C++
QObject* parent() const
```
筛选孩子（不是子类），而是添加到该类之下的称为该实例的孩子。
分别为选中一个最接近的，筛选所有同名的，按照正则进行筛选。  
根据FindChildOptions决定是否递归所有子类。
```C++
enum FindChildOptions{
    Qt::FindDirectChildrenOnly = 0x0,
    //Looks only at the direct children of the object.
    Qt::FindChildrenRecursively = 0x1,
    //Looks at all children of the object (recursive search).
}
```

#Signal/Slot
```C++
connect(const QObject *sender, const char *signal, const QObject *receiver, const char *method, Qt::ConnectionType type)
connect(const QObject *sender, const char *signal, const char *method, Qt::ConnectionType type) const
connect(const QObject *sender, PointerToMemberFunction signal, const QObject *receiver, PointerToMemberFunction method, Qt::ConnectionType type)
connect(const QObject *sender, PointerToMemberFunction signal, Functor functor)
connect(const QObject *sender, PointerToMemberFunction signal, const QObject *context, Functor functor, Qt::ConnectionType type)
disconnect(const QObject *sender, const char *signal, const QObject *receiver, const char *method)
disconnect(const char *signal, const QObject *receiver, const char *method) const
disconnect(const QObject *sender, PointerToMemberFunction signal, const QObject *receiver, PointerToMemberFunction method)
```
这些函数都是线程安全的。
``
`bool blockSignals(bool block)`阻塞信号，处理destroy外的所有信号不会发送如果block为true
`bool signalsBlocked() const` 信号是否阻塞

```C++
enum ConnectionType{
    Qt::AutoConnection = 0, //（默认）如果接收器位于发出信号的线程中， 则使用Qt::DirectConnection。 否则，将使用Qt::QueuedConnection。 连接类型是在信号发出时确定的。
    Qt::DirectConnection = 1, //当信号发出时，插槽立即被调用。插槽在信令线程中执行。
    Qt::QueuedConnection = 2, //当控件返回到接收方线程的事件循环时调用该插槽。槽在接收器的线程中执行。
    Qt::BlockingQueuedConnection = 3, //与Qt:：QueuedConnection相同，只是在插槽返回之前发信号的线程会阻塞。如果接收器与发信器不在同一个线程中，则不能使用此连接，否则应用程序将死锁。
    Qt::UniqueConnection = 0x80, //这是一个标志，可以使用位“或”与上述任何一种连接类型组合。设置Qt:：UniqueConnection时，如果连接已存在（即相同信号已连接到同一对对象的同一插槽），则QObject:：connect（）将失败。这个标志是在qt4.6中引入的。
}

```

```C++
protected:
    /*与某个信号相连的槽的数量*/
    int QObject::receivers(const char *signal) const
    /*只能在槽里调用，否则返回null，在当前上下文中该指针可用。当对象销毁或解除链接时指针不可用。
    Waring：当链接类型是DirectConnection时此对象的线程与发信器的线程不同时此函数返回值无效。
    */
    QObject *QObject::sender() const
```



#计时器
```C++
int QObject::startTimer(int interval, Qt::TimerType timerType = Qt::CoarseTimer)
void killTimer(int id)
virtual void QObject::timerEvent(QTimerEvent *event)
```
启动一个计时器并返回计时器标识符，无法启动计时器返回0.
在调用killTimer之前将每经过interval毫秒将发生一个计时器事件。
如果interval为0，每当没有更多的窗口系统事件要处理时，计时器事件都会发生。
当计时器事件发生时，会调用timerevent函数。
如果有多个计时器事件在运行，使用`QTimerEvent::timerId()`可以找出是哪个计时器被激活。



#Meta
```C++
virtual const QMetaObject* metaObject() const
```

```C++
QVariant property(const char *name) const

void setObjectName(const QString &name)

bool 
setProperty(const char *name, const QVariant &value)

```


#线程安全
```C++
//thread safe: false
void moveToThread(QThread *targetThread)

QThread* thread() const

```
对象会收到`QEvent::ThreadChange`事件，但在这是post的新事件会在新线程里处理。
如果参数为nullptr，则该实例会停止一切事件循环。

#一些判断
```C++
bool isWidgetType() const
bool isWindowType() const
bool inherits(const char* className) const //该类是否继承自某些类，（自己也算继承自己）
```


#调试
```C++
void dumpObjectInfo() const //Dumps information about signal connections, etc. for this object to the debug output.
void dumpObjectTree() const //Dumps a tree of children to the debug output.
```

