# 代码工厂—关系运算符

> 原文：<https://medium.com/geekculture/code-factory-relational-operators-3a0f2f06e638?source=collection_archive---------22----------------------->

![](img/62ffc21bf9796df7953825eb80ab2848.png)

在编程中，经常需要对表达式求值。我们可能需要检查存储在两个变量中的值是否相等，或者一个变量中的值是否大于另一个变量中的值，或者反之亦然，等等。对于这样的操作，我们使用关系运算符。

# 布尔数据类型

有一种特殊的数据类型称为布尔型。此数据类型只能存储两个值之一:true 或 false。要在 C 中使用布尔数据类型，我们必须导入一个名为“stdbool.h”的头文件。我们知道如何添加头文件。

```
//Adding stdbool.h file
#include<stdbool.h>//This is how to declare a boolean in C
bool c;
//We can only assign true or false to c
c = true;
```

我们只能给 c 赋值 true 或 false。

# ==

这是“等于”运算符。该运算符检查运算符两边的操作数值是否相等。

```
int a, b;
bool c;a = 3;
b = 5;c = a == b;
```

这里 c 将包含 false，因为 a 不等于 b。如果 a 和 b 相等，c 将包含 true。

# !=

这是“不等于”运算符。它检查运算符两边的操作数的值是否不相等。如果值相等，则返回 false，否则返回 true。

```
int a, b;
bool c;a = 3;
b = 5;c = a != b;
```

这里 c 将包含 true，因为 a 和 b 不相等。如果 a 和 b 相等，c 将包含 false

# >

这是“大于”运算符。它检查左边操作数的值是否大于右边操作数的值。

```
int a, b;
bool c;a = 3;
b = 5;c = a > b;
```

这里 c 将包含 false，因为 a 小于 b。如果 a 大于 b，c 将包含 true

# >=

这是“大于或等于”运算符。该运算符检查左边操作数的值是否大于或等于右边操作数的值。

```
int a, b;
bool c;a = 3;
b = 5;c = a >= b;
```

这里 c 将包含 false，因为 a 小于 b。如果 a 和 b 相等或者 a 大于 b，c 将包含 true。

# <

这是“小于”运算符。它检查左边操作数的值是否小于右边操作数的值。

```
int a, b;
bool c;a = 3;
b = 5;c = a < b;
```

这里 c 将包含 true，因为 a 小于 b。如果 a 大于 b，c 将包含 false。

# <=

这是“小于或等于”运算符。它检查左边操作数的值是否小于或等于右边操作数的值。

```
int a, b;
bool c;a = 3;
b = 5;c = a <= b;
```

这里 c 将包含 true，因为 a 小于 b。如果和 b 相等，c 也将包含 true。只有当 a 大于 b 时，c 才会包含 false。

[**先前= >代码工厂—算术运算符**](https://medium.datadriveninvestor.com/code-factory-arithmetic-operators-697082818925)