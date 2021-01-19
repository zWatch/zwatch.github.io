```c++
using addFunction = void(Class::*)(Args...);
std::map<int, addFunction> addWhat=
{
    {12, &Class::add12},
    {13, &Class::add13},
    {14, &Class::add14},
    {15, &Class::add15},    
};
auto _add = addWhat[ac];
(class->*_add)(args...);
```

相较于Switch，优点在可以动态增删，条理清晰

缺点在于需要实例化一个map，同时参数方面可能需要匿名函数以及`std::bind`等来补充。

部分情况下可以使用静态的 `static std::map<int, func> addWhat`

其实类似于工厂，但是不只可以用来作为工厂创建对象，因为存在匿名函数的捕获特性，也可以用来将杂乱的逻辑整理的很清晰。

工作中的实际例子

```c++

auo handle1 = [=, &arg]()
{
	...
}
auo handle2 = [=, &arg, &handle1]()
{
	...
}
...
//在这之前全是定义，以下才是流程
Database.tra...()//开启事务
handleX();
Database.commit();//结束事务

//同理，也可以使用锁等结构
try
{
	lock(Mutex1, Mutex2)...
    handleX()        
}
catch(Exception const& e)
{
    unlock(Mutex...);
    throw e;
}
unlock(Mutex...);
//像这样基本就不会忘记锁，
//也不会因为添加很多函数而不停的修改其参数列表
//如果写了很多逻辑后，发现有匿名函数能够复用的话，也很容易将其拷贝出来，直接根据报错来找参数和定义，不需要耗费心神。
```

