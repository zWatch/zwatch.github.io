from https://support.microsoft.com/zh-cn/help/815065/what-is-a-dll

# 什么是 DLL？

## 概要

------

本文说明什么是动态链接库 (DLL) 以及在使用 DLL 时可能发生的各种问题。

然后，本文说明在开发您自己的 DLL 时应该考虑的一些高级问题。 在说明什么是 DLL 的过程中，本文将说明动态链接方法、DLL 依赖性、DLL 入口点、导出 DLL 函数以及 DLL 故障排除工具。

本文最后将从较高的层次对 DLL 与 Microsoft .NET Framework 程序集作一比较。

## 简介

------

对于“适用范围”一节中列出的 Microsoft Windows 操作系统，操作系统的大量功能是由动态链接库 (DLL) 提供的。 另外，当您在这些 Windows 操作系统之一上运行某一程序时，该程序的很多功能可能是由 DLL 提供的。 例如，某些程序可能包含很多不同的模块，而该程序的每个模块都包含在 DLL 中并从中分发。

使用 DLL 有助于促进代码的模块化、代码重用、内存的有效使用和减少所占用的磁盘空间。 因此，操作系统和程序能够更快地加载和运行，并且在计算机中占用较少的磁盘空间。

当程序使用 DLL 时，一个称为依赖性的问题可能导致该程序无法运行。 当程序使用 DLL 时，就会创建一个依赖项。 如果其他程序改写和损坏了该依赖项，原来的那个程序就可能无法成功运行。

在引入 Microsoft .NET Framework 之后，大多数依赖性问题都已经通过使用程序集消除了。

## 更多信息

------

### 什么是 DLL？

DLL 是一个包含可由多个程序同时使用的代码和数据的库。 例如，在 Windows 操作系统中，Comdlg32 DLL 执行与对话框有关的常见函数。 因此，每个程序都可以使用该 DLL 中包含的功能来实现“打开”



对话框。 这有助于促进代码重用和内存的有效使用。

通过使用 DLL，程序可以实现模块化，由相对独立的组件组成。 例如，一个计帐程序可以按模块来销售。 可以在运行时将各个模块加载到主程序中（如果安装了相应模块）。 因为模块是彼此独立的，所以程序的加载速度更快，而且模块只在相应的功能被请求时才加载。

此外，可以更为容易地将更新应用于各个模块，而不会影响该程序的其他部分。 例如，您可能具有一个工资计算程序，而税率每年都会更改。 当这些更改被隔离到 DLL 中以后，您无需重新生成或安装整个程序就可以应用更新。

下表说明了 Windows 操作系统中的一些作为 DLL 实现的文件：

- ActiveX 控件 (.ocx) 文件
  ActiveX 控件的一个示例是日历控件，它使您可以从日历中选择日期。
- 控制面板 (.cpl) 文件
  .cpl 文件的一个示例是位于控制面板中的项。 每个项都是一个专用 DLL。
- 设备驱动程序 (.drv) 文件
  设备驱动程序的一个示例是控制打印到打印机的打印机驱动程序。

### DLL 的优点

下表说明了当程序使用 DLL 时提供的一些优点：

- 使用较少的资源
  当多个程序使用同一个函数库时，DLL 可以减少在磁盘和物理内存中加载的代码的重复量。 这不仅可以大大影响在前台运行的程序，而且可以大大影响其他在 Windows 操作系统上运行的程序。
- 推广模块式体系结构
  DLL 有助于促进模块式程序的开发。 这可以帮助你开发要求提供多个语言版本的大型程序或要求具有模块式体系结构的程序。 模块式程序的一个示例是具有多个可以在运行时动态加载的模块的计帐程序。
- 简化部署和安装
  当 DLL 中的函数需要更新或修复时，部署和安装 DLL 不要求重新建立程序与该 DLL 的链接。 此外，如果多个程序使用同一个 DLL，那么多个程序都将从该更新或修复中获益。 当您使用定期更新或修复的第三方 DLL 时，此问题可能会更频繁地出现。

### DLL 依赖项

当某个程序或 DLL 使用其他 DLL 中的 DLL 函数时，就会创建依赖项。 因此，该程序就不再是独立的，并且如果该依赖项被损坏，该程序就可能遇到问题。 例如，如果发生下列操作之一，则该程序可能无法运行：

- 依赖 DLL 升级到新版本。
- 修复了依赖 DLL。
- 依赖 DLL 被其早期版本覆盖。
- 从计算机中删除了依赖 DLL。

这些操作通常称为 DLL 冲突。 如果没有强制实现向后兼容性，则该程序可能无法成功运行。

下表说明了为了帮助最大限度地减少依赖性问题而在 Microsoft Windows 2000 和较高版本的 Windows 操作系统中引入的更改：

- Windows 文件保护
  在 Windows 文件保护中，操作系统禁止未经授权的代理更新或删除系统 DLL。 因此，当程序安装操作尝试删除或更新被定义为系统 DLL 的 DLL 时，Windows 文件保护将寻找有效的数字签名。
- 专用 DLL
  通过专用 DLL 可以使程序避免遭受对共享 DLL 进行的更改。 专用 DLL 使用版本特定信息或空 .local 文件来强制要求程序所使用的 DLL 的版本。 要使用专用 DLL，请在程序根文件夹中找到 DLL。 然后，对于新程序，请向该 DLL 中添加版本特定信息。 对于旧程序，请使用空 .local 文件。 每个方法都告诉操作系统使用位于程序根文件夹中的专用 DLL。

### DLL 故障排除工具

可以使用多个工具来帮助您解决 DLL 问题。 以下是其中的部分工具。

#### Dependency Walker

Dependency Walker 工具可以递归扫描以寻找程序所使用的所有依赖 DLL。 当在 Dependency Walker 中打开程序时，Dependency Walker 会执行下列检查：

- Dependency Walker 检查是否丢失 DLL。
- Dependency Walker 检查是否存在无效的程序文件或 DLL。
- Dependency Walker 检查导入函数和导出函数是否匹配。
- Dependency Walker 检查是否存在循环依赖性错误。
- Dependency Walker 检查是否存在由于针对另一不同操作系统而无效的模块。

通过使用 Dependency Walker，您可以记录程序使用的所有 DLL。 这可能有助于避免和更正将来可能发生的 DLL 问题。 当安装 Microsoft Visual Studio 6.0 时，Dependency Walker 将位于以下目录中：

**驱动器**\Program Files\Microsoft Visual Studio\Common\Tools

#### DLL Universal Problem Solver

DLL Universal Problem Solver (DUPS) 工具用于审核、比较、记录和显示 DLL 信息。 下表说明了组成 DUPS 工具的实用工具：

- Dlister.exe
  该实用工具枚举计算机中的所有 DLL，并且将此信息记录到一个文本文件或数据库文件中。
- Dcomp.exe
  该实用工具比较在两个文本文件中列出的 DLL，并产生包含差异的第三个文本文件。
- Dtxt2DB.exe
  该实用工具将通过使用 Dlister.exe 实用工具和 Dcomp.exe 实用工具创建的文本文件加载到 dllHell 数据库中。
- DlgDtxt2DB.exe
  该实用工具提供 Dtxt2DB.exe 实用工具的图形用户界面 (GUI) 版本。

有关 DUPS 工具的更多信息，请单击下面的文章编号，以查看 Microsoft 知识库中相应的文章：

[247957 ](https://support.microsoft.com/help/247957)使用 DUPS.exe 解决 DLL 兼容性问题
 

#### DLL 帮助数据库

DLL 帮助数据库帮助您查找由 Microsoft 软件产品安装的特定版本的 DLL。 

### DLL 开发

本节介绍您在开发自己的 DLL 时应该考虑的问题和要求。

#### DLL 的类型

当您在应用程序中加载 DLL 时，可以使用两种链接方法来调用导出的 DLL 函数。 这两种链接方法是加载时动态链接和运行时动态链接。

##### 加载时动态链接

在加载时动态链接中，应用程序像调用本地函数一样对导出的 DLL 函数进行显式调用。 要使用加载时动态链接，请在编译和链接应用程序时提供头文件 (.h) 和导入库文件 (.lib)。 当您这样做时，链接器将向系统提供加载 DLL 所需的信息，并在加载时解析导出的 DLL 函数的位置。

##### 运行时动态链接

在运行时动态链接中，应用程序调用

 

LoadLibrary

 

函数或

 

LoadLibraryEx

 

函数以在运行时加载 DLL。 成功加载 DLL 后，可以使用

 

GetProcAddress

 

函数获得要调用的导出的 DLL 函数的地址。 在使用运行时动态链接时，无需使用导入库文件。

下面的列表说明了有关何时使用加载时动态链接以及何时使用运行时动态链接的应用程序条件：

- 启动性能
  如果应用程序的初始启动性能很重要，则应使用运行时动态链接。
- 易用性
  在加载时动态链接中，导出的 DLL 函数类似于本地函数。 这使您可以方便地调用这些函数。
- 应用程序逻辑
  在运行时动态链接中，应用程序可以分支，以便按照需要加载不同的模块。 在开发多语言版本时，这一点很重要。

#### DLL 入口点

在创建 DLL 时，可以有选择地指定入口点函数。 当进程或线程将它们自身附加到 DLL 或者将它们自身从 DLL 分离时，将调用入口点函数。 您可以使用入口点函数根据 DLL 的需要来初始化数据结构或者销毁数据结构。 此外，如果应用程序是多线程的，则可以在入口点函数中使用线程本地存储 (TLS) 来分配各个线程专用的内存。 下面的代码是一个 DLL 入口点函数的示例。

```
BOOL APIENTRY DllMain(
HANDLE hModule,// Handle to DLL module
DWORD ul_reason_for_call,// Reason for calling function
LPVOID lpReserved ) // Reserved
{
switch ( ul_reason_for_call )
{
case DLL_PROCESS_ATTACHED:
// A process is loading the DLL.
break;
case DLL_THREAD_ATTACHED:
// A process is creating a new thread.
break;
case DLL_THREAD_DETACH:
// A thread exits normally.
break;
case DLL_PROCESS_DETACH:
// A process unloads the DLL.
break;
}
return TRUE;
}
```

当入口点函数返回

 

FALSE

 

值时，如果你使用的是加载时动态链接，则应用程序不启动。 如果您使用的是运行时动态链接，则只有个别 DLL 不会加载。

入口点函数只应执行简单的初始化任务，不应调用任何其他 DLL 加载函数或终止函数。 例如，在入口点函数中，不应直接或间接调用

 

LoadLibrary

 

函数或

 

LoadLibraryEx

 

函数。 此外，不应在进程终止时调用

 

FreeLibrary

 

函数。

注意

 

在多线程应用程序中，请确保将对 DLL 全局数据的访问进行同步（线程安全），以避免可能的数据损坏。 为此，请使用 TLS 为各个线程提供唯一的数据。

#### 导出 DLL 函数

要导出 DLL 函数，您可以向导出的 DLL 函数中添加函数关键字，也可以创建模块定义文件 (.def) 以列出导出的 DLL 函数。

要使用函数关键字，您必须使用以下关键字来声明要导出的各个函数：

__declspec(dllexport)

要在应用程序中使用导出的 DLL 函数，你必须使用以下关键字来声明要导入的各个函数：

__declspec(dllimport)

通常情况下，你最好使用一个包含

 

define

 

语句和

 

ifdef

 

语句的头文件，以便分隔导出语句和导入语句。

你还可以使用模块定义文件来声明导出的 DLL 函数。 当您使用模块定义文件时，您不必向导出的 DLL 函数中添加函数关键字。 在模块定义文件中，你可以声明 DLL 的

 

LIBRARY

 

语句和

 

EXPORTS

 

语句。 下面的代码是一个定义文件的示例。

```
// SampleDLL.def
//
LIBRARY "sampleDLL"

EXPORTS
  HelloWorld
```

#### 示例 DLL 和应用程序

在 Microsoft Visual C++ 6.0 中，可以通过选择“Win32 动态链接库”



项目类型或

“MFC 应用程序向导 (dll)”



项目类型来创建 DLL。

下面的代码是一个在 Visual C++ 中通过使用

“Win32 动态链接库”



项目类型创建的 DLL 的示例。

```
// SampleDLL.cpp
//

#include "stdafx.h"
#define EXPORTING_DLL
#include "sampleDLL.h"

BOOL APIENTRY DllMain( HANDLE hModule, 
                       DWORD  ul_reason_for_call, 
                       LPVOID lpReserved
 )
{
    return TRUE;
}

void HelloWorld()
{
MessageBox( NULL, TEXT("Hello World"), TEXT("In a DLL"), MB_OK);
}
// File: SampleDLL.h
//
#ifndef INDLL_H
#define INDLL_H

#ifdef EXPORTING_DLL
extern __declspec(dllexport) void HelloWorld() ;
#else
extern __declspec(dllimport) void HelloWorld() ;
#endif

#endif
```

下面的代码是一个调用 SampleDLL DLL 中的导出 DLL 函数的“Win32 应用程序”



项目的示例。

```
// SampleApp.cpp 
//

#include "stdafx.h"
#include "sampleDLL.h"

int APIENTRY WinMain(HINSTANCE hInstance,
                     HINSTANCE hPrevInstance,
                     LPSTR     lpCmdLine,
                     int       nCmdShow)
{ 
HelloWorld();
return 0;
}
```

注意

 

在加载时动态链接中，你必须链接在生成 SampleDLL 项目时创建的 SampleDLL.lib 导入库。

在运行时动态链接中，您应使用与以下代码类似的代码来调用 SampleDLL.dll 导出 DLL 函数。

```
...
typedef VOID (*DLLPROC) (LPTSTR);
...
HINSTANCE hinstDLL;
DLLPROC HelloWorld;
BOOL fFreeDLL;

hinstDLL = LoadLibrary("sampleDLL.dll");
if (hinstDLL != NULL)
{
    HelloWorld = (DLLPROC) GetProcAddress(hinstDLL, "HelloWorld");
    if (HelloWorld != NULL)
        (HelloWorld);

    fFreeDLL = FreeLibrary(hinstDLL);
}
...
```

当您编译和链接 SampleDLL 应用程序时，Windows 操作系统将按照以下顺序在下列位置中搜索 SampleDLL DLL：

1. 应用程序文件夹

2. 当前文件夹

3. Windows 系统文件夹

   注意 GetSystemDirectory 函数返回 Windows 系统文件夹的路径。

4. Windows 文件夹

   注意 GetWindowsDirectory 函数返回 Windows 文件夹的路径。

### .NET Framework 程序集

在引入 Microsoft .NET 和 .NET Framework 以后，大多数与 DLL 相关联的问题已经通过使用程序集消除了。 程序集是在 .NET 公共语言运行库 (CLR) 控制之下运行的逻辑功能单元。 程序集实际上是作为 .dll 文件或 .exe 文件存在的。 但是，在内部，程序集与 Microsoft Win32 DLL 大不相同。

程序集文件包含程序集清单、类型元数据、Microsoft 中间语言 (MSIL) 代码和其他资源。 程序集清单包含程序集元数据，以提供使程序集成为自描述程序集所需的全部信息。 程序集清单中包含以下信息：

- 程序集名称
- 版本信息
- 区域性信息
- 强名称信息
- 程序集文件列表
- 类型引用信息
- 引用和依赖程序集信息

程序集中包含的 MSIL 代码是无法直接执行的， 需要通过 CLR 来执行。 默认情况下，当您创建一个程序集时，该程序集是应用程序专有的。 要创建共享程序集，需要为该程序集分配强名称，然后在全局程序集缓存中发布该程序集。

下表说明了程序集的一些功能，并将其与 Win32 DLL 的功能进行了比较：

- 自描述
  当您创建程序集时，CLR 运行该程序集所需的全部信息都包含在程序集清单中。 程序集清单包含一个依赖程序集列表。 因此，CLR 可以维护一组在应用程序中使用的一致的程序集。 在 Win32 DLL 中，当您使用共享 DLL 时，无法维护应用程序中使用的一组 DLL 之间的一致性。
- 版本控制
  在程序集清单中，版本信息由 CLR 记录和实施。 另外，可以通过版本策略来实施版本特定用法。 在 Win32 DLL 中，无法由操作系统实施版本控制。 相反，你必须确保 DLL 向后兼容。
- 并行部署
  程序集支持并行部署。 一个应用程序可以使用一个版本的程序集，而另一个应用程序可以使用另一不同版本的程序集。 从 Windows 2000 开始，通过将 DLL 放置到应用程序文件夹中支持并行部署。 另外，Windows 文件保护能够防止系统 DLL 被未经授权的代理改写或替换。
- 独立和隔离
  通过使用程序集开发的应用程序可以是独立的，并且与计算机中正在运行的其他应用程序隔离。 这一特性有助于创建零干扰安装。
- 执行
  程序集在程序集清单所提供的并且由 CLR 控制的安全权限下运行。
- 语言无关性
  可以通过使用任何一种受支持的 .NET 语言来开发程序集。 例如，可以在 Microsoft Visual C# 中开发程序集，然后在 Microsoft Visual Basic .NET 项目中使用该程序集。

## 参考

------

有关 DLL 和 .NET Framework 程序集的更多信息，请访问下面的 Microsoft 网站：

[DLL conflicts（DLL 冲突）](http://msdn2.microsoft.com/library/ms811694.aspx)

[Implementing side-by-side component sharing in applications（在应用程序中实现并行组件共享）](http://msdn2.microsoft.com/library/ms811700.aspx)

[How to build and service isolated applications and side-by-side assemblies for Windows XP（如何生成和维护用于 Windows XP 的独立应用程序和并行程序集）](http://msdn2.microsoft.com/library/ms997620.aspx)

[Simplifying deployment and solving DLL conflicts with the .NET Framework（使用 .NET Framework 简化部署和解决 DLL 冲突）](http://msdn2.microsoft.com/netframework/aa497268.aspx)

[.NET Framework 开发人员指南： 程序集](http://msdn2.microsoft.com/library/hk5f40ct(vs.71).aspx)

[Run-time dynamic linking（运行时动态链接）](http://msdn2.microsoft.com/library/ms685090.aspx)

[Thread local storage（线程本地存储）](http://msdn2.microsoft.com/library/ms686749.aspx)



------

上次更新时间：2020年1月23日