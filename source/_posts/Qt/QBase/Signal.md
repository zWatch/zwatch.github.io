# 信号

## 连接

```C++
connect(C_selsct, SIGNAL(colorSelected(const QColor&)),
            this, SLOT(onSelectColorForFlag(const QColor&)));
```

信号连接，信号和槽要用宏包起来，不要加发送者或接受者的类型，要写上函数签名（不带返回值），不带参数名。
