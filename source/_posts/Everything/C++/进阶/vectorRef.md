vector\<string\> a;
vector\<string\&\> b; 被视为不同的类型

```c++
vector<string> a;
vector<string&> b;
//a=b; //error
//b=a; //error too

//这样可以
template<typename T>
bool copyVector(vector<T> dest, vector<T&> source)
{
    for(auto& iter: source)
    {
        dest.push_back(iteer);
    }
}




```

