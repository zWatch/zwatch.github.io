idea来自qt，以及entt的service_locator

```c++
template<typename _To, typename _From>
custom_cast<_To*>(_From*)exception(false)

template<typename _To, typename _From>
custom_cast<_To&>(_From&, std::err_code&)exception(false)

template<typename _To, typename _From>
dynamic_custom_cast<std::shared_ptr<_To>>(std::shared_ptr<_From>) exception(true)

template<typename _To, typename _From>
dynamic_custom_cast<std::shared_ptr<_To>>(std::shared_ptr<_From>, std::err_code&) exception(false)
```

自定义转换，采用注册函数的形式来为用户提供自定义的转换函数。以参数的type\_index(type\_id(\_To)), type\_index(type\_id(\_From)) 作为下标。

刚好在CPP20中将operator[]中的逗号表达式废弃，以后可能会加入多维下标或其他的自定义下标。

当然现在还没想到有什么用。