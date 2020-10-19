在js语言里，callback很容易实现，编码也很少。而在cpp中，要考虑类型等因素，可以使用如下的定义形式

```c++
using ReadOnceCallback = void(void* handle, int errcode);

```

这样相较于原始的指针定义 `typedef void (* ReadOnceCallback)(void*, int)` 

