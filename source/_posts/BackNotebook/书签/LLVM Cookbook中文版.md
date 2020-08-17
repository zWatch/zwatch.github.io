最新书签教程 链接: https://pan.baidu.com/s/1Qmbrff5ptamPexy4pEMT-Q 提取码: jsfh 在线转pdf功能，生成的pdf会自动加载书签

第1章 LLVM设计与使用 1
概述 1
模块化设计 2
交叉编译Clang/LLVM 6
将C源码转换为LLVM汇编码 8
将LLVM IR转换为bitcode 9
将LLVM bitcode转换为目标平台汇编码 12
将LLVM bitcode转回为LLVM汇编码 14
转换LLVM IR 15
链接LLVM bitcode 18
执行LLVM bitcode 19
使用C语言前端——Clang 20
使用GO语言前端 24
使用DragonEgg 25
第2章 实现编译器前端 29
概述 29
定义TOY语言 30
实现词法分析器 32
定义抽象语法树 35
实现语法分析器 38
解析简单的表达式 39
解析二元表达式 42
为解析编写驱动 45
对TOY语言进行词法分析和语法分析 47
为每个AST类定义IR代码生成方法 48
为表达式生成IR代码 49
为函数生成IR代码 51
增加IR优化支持 55
第3章 扩展前端并增加JIT支持 57
概述 57
处理条件控制结构——if/then/else结构 58
生成循环结构 64
处理自定义二元运算符 71
处理自定义一元运算符 77
增加JIT支持 83
第4章 准备优化 87
概述 87
多级优化 88
自定义LLVM Pass 89
使用opt工具运行自定义Pass 92
在新的Pass中调用其他Pass 93
使用Pass管理器注册Pass 96
实现一个分析Pass 99
实现一个别名分析Pass 102
使用其他分析Pass 105
第5章 实现优化 109
概述 109
编写无用代码消除Pass 110
编写内联转换Pass 115
编写内存优化Pass 119
合并LLVM IR 121
循环的转换与优化 123
表达式重组 126
IR向量化 127
其他优化Pass 134
第6章 平台无关代码生成器 139
概述 139
LLVM IR指令的生命周期 140
使用GraphViz可视化LLVM IR控制流图 143
使用TableGen描述目标平台 150
定义指令集 151
添加机器码描述 152
实现MachineInstrBuider类 156
实现MachineBasicBlock类 157
实现MachineFunction类 159
编写指令选择器 160
合法化SelectionDAG 166
优化SelectionDAG 173
基于DAG的指令选择 179
基于SelectionDAG的指令调度 186
第7章 机器码优化 191
概述 191
消除机器码公共子表达式 192
活动周期分析 203
寄存器分配 209
插入头尾代码 215
代码发射 219
尾调用优化 221
兄弟调用优化 225
第8章 实现LLVM后端 227
概述 227
定义寄存器和寄存器集合 228
定义调用约定 230
定义指令集 231
实现栈帧lowering 232
打印指令 236
选择指令 240
增加指令编码 244
子平台支持 246
多指令lowering 249
平台注册 251
第9章 LLVM项目最佳实践 265
概述 265
LLVM中的异常处理 265
使用sanitizer 271
使用LLVM编写垃圾回收器 273
将LLVM IR转换为JavaScript 279
使用Clang静态分析器 281
使用bugpoint 282
使用LLDB 286
使用LLVM通用Pass 291