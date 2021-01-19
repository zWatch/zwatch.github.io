//应用意义不大，在对性能敏感的地方可能有用

指如下用法

```c++
Something obj = createObj(Arg)
```

此时，并不是调用函数，构造，返回临时的Something实例，赋值

而是只调用了构造函数。

这一优化要求：

调用者和函数定义在同一个模块(DLL或exe)下

函数中的返回值必须满足一下条件

​	

```c++
//1 只返回同一实例，如
long createObj( int a)
{
	long ret = 0;
	if(a%2)
	{	
		ret = 1321%a;	
		return ret
	}	
	return ret;
}

//以下这个就不行（必须是大对象，小对象会被其他优化）
long createObj( int a)
{
	long ret = 0;
	if(a%2)
	{	
		long ret2 = 1321%a;	
		return ret2;
	}	
	return ret;
}
```

RVO优化其实也就是将形如`auto obj=createObj()` 中obj的初始化放到了函数里

