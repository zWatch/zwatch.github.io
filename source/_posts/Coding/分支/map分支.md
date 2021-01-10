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

