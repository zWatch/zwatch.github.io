#QT_NO_NARROWING_CONVERSIONS_IN_CONNECT
禁止在链接中缩小参数类型（比如float到int）

#Q_CLASSINFO(Name, Value)
```C++
class MyClass : public QObject
 {
     Q_OBJECT
     Q_CLASSINFO("Author", "Pierre Gendron")
     Q_CLASSINFO("URL", "http://www.my-organization.qc.ca")

 public:
     ...
 };
```
提供额外的信息。
#拷贝/移动禁止
```C++
Q_DISABLE_COPY(Class)
Q_DISABLE_COPY_MOVE(Class)
Q_DISABLE_MOVE(Class)
```
#Q_ENUM(...)
必须放在enum定义之后。
```C++
class MyClass : public QObject
{
    Q_OBJECT
public:
    MyClass(QObject *parent = nullptr);
    ~MyClass();

    enum Priority { High, Low, VeryHigh, VeryLow };
    Q_ENUM(Priority)
    void setPriority(Priority priority);
    Priority priority() const;
};
```
只能在类里使用，在命名空间中请使用Q_ENUM_NS
#Q_FLAG(...)
同Q_ENUM不过他是给标志位使用的。
```C++
class QItemSelectionModel : public QObject
{
    Q_OBJECT

public:
    ...
    enum SelectionFlag {
        NoUpdate       = 0x0000,
        Clear          = 0x0001,
        Select         = 0x0002,
        Deselect       = 0x0004,
        Toggle         = 0x0008,
        Current        = 0x0010,
        Rows           = 0x0020,
        Columns        = 0x0040,
        SelectCurrent  = Select | Current,
        ToggleCurrent  = Toggle | Current,
        ClearAndSelect = Clear | Select
    };

    Q_DECLARE_FLAGS(SelectionFlags, SelectionFlag)
    Q_FLAG(SelectionFlags)
    ...
}
```
同样有Q_FLAG_NS


#Q_GADGET
Q_OGJECT的轻量级版本。只支持Q_ENUM, Q_PROPERTY and Q_INVOKABLE。不支持信号和槽

#Q_INVOKABLE
将此宏应用于成员函数的声明，以允许通过元对象系统调用它们`QMetaObject::invokeMethod()`。宏写在返回类型之前，如下例所示：
```C++
class Window : public QWidget
 {
     Q_OBJECT

 public:
     Window();
     void normalMethod();
     Q_INVOKABLE void invokableMethod();
 };
```

#Q_NAMESPACE
同Q_OBJECT


#Q_PROPERTY(...)
此宏用于在继承QObject的类中声明属性。属性的行为类似于类数据成员，但它们具有可通过元对象系统访问的附加功能。

#Q_REVISION
```C++
class Window : public QWidget
 {
     Q_OBJECT
     Q_PROPERTY(int normalProperty READ normalProperty)
     Q_PROPERTY(int newProperty READ newProperty REVISION 1)

 public:
     Window();
     int normalProperty();
     int newProperty();
 public slots:
     void normalMethod();
     Q_REVISION(1) void newMethod();
 };
 ```
 
 #Q_SET_OBJECT_NAME(Object)
 给OBJECT设置名字
