<https://docs.microsoft.com/zh-cn/cpp/preprocessor/auto-inline?view=vs-2019>

# Pragma

**#pragma** *标记-字符串*

Microsoft C 和 C++ 编译器识别以下杂注：

[`alloc_text`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/alloc-text?view=vs-2019)
[`auto_inline`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/auto-inline?view=vs-2019)
[`bss_seg`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/bss-seg?view=vs-2019)
[`check_stack`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/check-stack?view=vs-2019)
[`code_seg`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/code-seg?view=vs-2019)
[`comment`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/comment-c-cpp?view=vs-2019)
[`component`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/component?view=vs-2019)
[`conform`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/conform?view=vs-2019)1
[`const_seg`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/const-seg?view=vs-2019)
[`data_seg`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/data-seg?view=vs-2019)
[`deprecated`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/deprecated-c-cpp?view=vs-2019)

[`detect_mismatch`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/detect-mismatch?view=vs-2019)
[`fenv_access`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/fenv-access?view=vs-2019)
[`float_control`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/float-control?view=vs-2019)
[`fp_contract`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/fp-contract?view=vs-2019)
[`function`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/function-c-cpp?view=vs-2019)
[`hdrstop`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/hdrstop?view=vs-2019)
[`include_alias`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/include-alias?view=vs-2019)
[`init_seg`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/init-seg?view=vs-2019)1
[`inline_depth`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/inline-depth?view=vs-2019)
[`inline_recursion`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/inline-recursion?view=vs-2019)

[`intrinsic`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/intrinsic?view=vs-2019)
[`loop`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/loop?view=vs-2019)1
[`make_public`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/make-public?view=vs-2019)
[`managed`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/managed-unmanaged?view=vs-2019)
[`message`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/message?view=vs-2019)
[`omp`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/omp?view=vs-2019)
[`once`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/once?view=vs-2019)
[`optimize`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/optimize?view=vs-2019)
[`pack`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/pack?view=vs-2019)
[`pointers_to_members`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/pointers-to-members?view=vs-2019)1

[`pop_macro`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/pop-macro?view=vs-2019)
[`push_macro`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/push-macro?view=vs-2019)
[`region`，endregion](https://docs.microsoft.com/zh-cn/cpp/preprocessor/region-endregion?view=vs-2019)
[`runtime_checks`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/runtime-checks?view=vs-2019)
[`section`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/section?view=vs-2019)
[`setlocale`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/setlocale?view=vs-2019)
[`strict_gs_check`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/strict-gs-check?view=vs-2019)
[`unmanaged`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/managed-unmanaged?view=vs-2019)
[`vtordisp`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/vtordisp?view=vs-2019)1
[`warning`](https://docs.microsoft.com/zh-cn/cpp/preprocessor/warning?view=vs-2019)

1 仅由 c + + 编译器支持。

## intrinsic

**#pragma intrinsic(** *function1* [**,** *function2* ... ] **)**

指定对杂注参数列表中指定的函数的调用是内部的。

# pop_macro 

**#pragma pop_macro (** "*宏名*" **)**

## pop_macro 杂注

2019/08/29

将*宏名称*宏的值设置为此宏的堆栈顶部的值。

### 语法

> **#pragma pop_macro (** "*宏名*" **)**

### 备注

必须先发出[push_macro](https://docs.microsoft.com/zh-cn/cpp/preprocessor/push-macro?view=vs-2019) for*宏名称*, 然后才能执行**pop_macro**。

### 示例

C++

```cpp
// pragma_directives_pop_macro.cpp
// compile with: /W1
#include <stdio.h>
#define X 1
#define Y 2

int main() {
   printf("%d",X);
   printf("\n%d",Y);
   #define Y 3   // C4005
   #pragma push_macro("Y")
   #pragma push_macro("X")
   printf("\n%d",X);
   #define X 2   // C4005
   printf("\n%d",X);
   #pragma pop_macro("X")
   printf("\n%d",X);
   #pragma pop_macro("Y")
   printf("\n%d",Y);
}
```

Output

```Output
1
2
1
2
1
3
```

## Tick

if \__LINE__ < 12

define AType /##/

