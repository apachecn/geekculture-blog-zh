# 常见功能属性

> 原文：<https://medium.com/geekculture/some-common-function-attributes-that-i-found-useful-279690ec043b?source=collection_archive---------16----------------------->

## **我发现有用的一些常用函数属性！**

![](img/87d8906d7ecd1712f38626a199a6b082.png)

[https://commons.wikimedia.org/wiki/File:GNU_Love.png](https://commons.wikimedia.org/wiki/File:GNU_Love.png)

在我的[第一篇帖子](/@pinloon/hello-world-70a51f449d9f)之后，我觉得在进入机器人嵌入式 C/C++编程的更高层次之前，继续从低层次的角度来看可能是好的。

gcc 的[文档中有很多通用的函数属性。在这篇文章中，我将展示几个函数属性的用法示例，它们是](https://gcc.gnu.org/onlinedocs/gcc/Common-Function-Attributes.html#Common-Function-Attributes)

*   `__attribute__((warn_unused_result))`
*   `__attribute__((deprecated))`
*   `__attribute__((__weak__))`

也许你遇到过类似下面的情况

```
#ifndef __must_check
#define __must_check __attribute__((warn_unused_result))
#endif

#ifndef __deprecated
#define __deprecated __attribute__((deprecated))
#endif

#ifndef __weak
#define __weak __attribute__((__weak__))
#endif
```

## `__attribute__((warn_unused_result))`

在某些情况下，强迫用户捕捉函数的返回值是非常重要的。如下所示，有一个状态代码表示初始化过程的结果。

```
int Init() {
  // some status code after certain intiialization process
  return status_code;
}
```

在这种情况下，我们希望避免调用如下函数

```
int main(int argc, char **argv) {
  Init();
  return 0;
}
```

因此，我们可以添加`__attribute__((warn_unused_result))`,使函数看起来像

```
__attribute__((warn_unused_result)) 
int Init() {
  // some status code after certain intiialization process
  return status_code;
}
```

在这种情况下，如果函数的返回值被忽略(像上面的例子那样调用函数)，用户将得到如下警告

```
warning: ignoring return value of ‘int Init()’, declared with attribute warn_unused_result [-Wunused-result]
```

## `__attribute__((deprecated))`

大多数情况下，当我们维护一个由大量用户使用的代码库时，弃用代码是很正常的，但是可能会有一个承诺期来维护相同的接口，同时向后兼容也是一个常见的需求。下面的示例显示了当调用的函数被否决时，函数属性如何帮助向用户发出警告。

```
__attribute__((deprecated)) int Init();
```

拥有以上属性，用户在调用`Init()`的函数时会得到如下警告。

```
warning: ‘int Init()’ is deprecated [-Wdeprecated-declarations]
```

## _ _ 已弃用 _ 宏

作为标记为弃用的延续，我们也可以将一些旧的宏标记为`deprecated`。

```
#define __WARN(msg) __WARN_GCC(GCC warning msg)
#define __WARN_GCC(s) _Pragma(#s)

#ifndef __DEPRECATED_MACRO
#define __DEPRECATED_MACRO __WARN("Macro is deprecated")
#endif
```

如果之前我们有一个宏

```
#define MAX(a,b) (a > b? a: b)
```

我们可以将其标记为

```
#define MAX(a,b) __DEPRECATED_MACRO std::max(a,b)
```

调用宏现在会输出

```
warning: Macro is deprecated
```

## `__attribute__((__weak__))`

从`gcc`文档中，

*“弱属性导致声明作为弱符号而不是全局符号发出。这主要用于定义可以在用户代码中覆盖的库函数，尽管它也可以用于非函数声明。ELF 目标支持弱符号，使用 GNU 汇编程序和链接程序时，a.out 目标也支持弱符号。*

例如，如果我们有一个

```
__attribute__((__weak__)) int Init() { 
  std::cout << "Init is not supported" << std::endl;
  return 0; 
}
```

调用该函数将导致输出

```
Init is not supported
```

**如果**没有其他翻译单位提供相同功能的定义。

如果有其他文件也提供了定义，

```
int Init() { 
    std::cout << "Init done" << std::endl;
    return 2; 
}
```

调用`Init()`的输出现在应该是

```
Init done
```

该理论是，每当链接器发现弱符号和强符号时，它将首先选择强符号。如果只有弱符号，它将被选择。这仅对静态库有效。o .a .)，它在动态环境中不起作用。所以)图书馆。

基于`One Definition Rule`，

*“对象和非内联函数在整个程序中不能有多个定义”*

所以如果你删除了`__attribute__((__weak__))`，你会得到一个报错多重定义的错误。

帖子到此结束，希望你喜欢阅读！