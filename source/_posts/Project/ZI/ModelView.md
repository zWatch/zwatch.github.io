qt modelview需要增加Move，将部分数据移动。



```c++
//QModelIndex->
struct 临时Index
{
	weak_ptr<Model> model;
	int pos;
}

struct 持久化Index
{
	weak_ptr<Model> model;
	shared_ptr<int> pos;//使用智能指针pos是为了能在Model里改变它
}








```

