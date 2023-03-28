# Python 3 面试准备练习

> 原文：<https://medium.com/geekculture/python-drills-for-interview-preparation-62196676a306?source=collection_archive---------6----------------------->

## 必知的 Python 3 数据结构和重要操作之旅

![](img/85b6f701426702e2196c8e10d0b41928.png)

Photo by [Steve Johnson](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/abstract-painting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

每个部分都由一个“演练”组成，探究一些常见 Python 数据结构的重要操作。

变量和方法的名称有意保持简短，以尽量减少完成代码练习所需的输入量。

# 这篇文章不是什么

这篇文章是关于通过做来学习*的，并没有对所使用的操作提供全面的解释。您可以在自己的时间里更详细地探索这些操作。*

# 设置

在这里安装最新版本的 Python。

打开终端，键入“python3”并按回车键:

现在你可以开始练习了，希望这对你有用！

# 基本控制流程

```
a = 1
b = 2**if** a < b:
    print('a < b')
**elif** a == b:
    print('a = b')
**else**:
    print('a > b')            // 'a < b'**while** a < 3:
   print(a)
   a += 1
**else**:
   print('a = 3')             // '1'
                              // '2'
                              // 'a = 3'**for** x **in** [1,2]:
    print(x)                  // 1
                              // 2
```

# 数字

```
5/2             // 2.5
5//2            // 2
5%2             // 1
5 ** 2          // 25
5e3             // 5000.0**type**(1)         // <type 'int'>
**type**(1.1)       // <type 'float'>**int**(1.1)        // 1
**float**(1)        // 1.0
**str**(1.1)        // '1.1' **round**(1.4)      // 1.0
**round**(1.5)      // 2.0
**round**(1.15,1)   // 1.1
**round**(1.16,1)   // 1.2**abs**(-1)         // 1
**min**(1,2)        // 1
**max**(1,2)        // 2import **math**math.**floor**(1.1)    // 1
math.**ceil**(1.1)     // 2
math.**factorial**(3)  // 6(1.0).**is_integer**() // True
(1.1).**is_integer**() // False
```

# 用线串

```
'a b'           // 'a b'
'a\'b'          // "a'b"
2 * 'a'         // 'aa'
'a' 'b'         // 'ab'
'a' + 'b'       // 'ab'x = 'abc'
**len**(x)          // 3x.**upper**()       // 'ABC'
x.**lower**()       // 'abc'
x.**swapcase**()    // 'ABC'
x.**capitalize**()  // 'Abc'
x.**islower**()     // True
x.**isupper**()     // False
x.**isnumeric**()   // False
x.**isalpha**()     // True
x.**isalnum**()     // Truex.**count**('a')        // 1
x.**count**('A')        // 0
x.**startswith**('a')   // True
x.**endswith**('c')     // True
x.**find**('b')         // 1
x.**find**('e')         // -1
x.**index**('b')        // 1
x.**index**('e')        // 🔥
x.**find**('b', 0, 1)   // -1
x.**index**('b',0, 2)   // 1x.**replace**('a','z')  // 'zbc'
x.**partition**('b')    // ('a', 'b', 'c')
x.**partition**('a')    // ('', 'a', 'bc')x[0]            // 'a'
x[1]            // 'b'
x[2]            // 'c'
x[-1]           // 'c'
x[-2]           // 'b'
x[-3]           // 'a'
x[4]            // 🔥
x[-4]           // 🔥x[0:1]          // 'a'       (from index 0 -> 1, not including 1)
x[0:2]          // 'ab'
x[0:3]          // 'abc'
x[0:4]          // 'abc'
x[2:3]          // 'c'
x[3:4]          // ''
x[**2**:**1**]          // ''        (2 > 1 -> '')
x[**2**:**2**]          // ''        (2 = 2 -> '')x[0:]           // 'abc'
x[:]            // 'abc'
x[1:]           // 'bc'      (from index position 1 onwards)
x[2:]           // 'c'
x[3:]           // ''
x[:1]           // 'a'
x[:2]           // 'ab'
x[:3]           // 'abc'x[-3:]          // 'abc'     (max(len(x)**-3**,0) = **0**; x[-3:] == x[**0**:])
x[-2:]          // 'bc'      (max(len(x)**-2**,0) = **1**; x[-2:] == x[**1**:])
x[-1:]          // 'c'       (max(len(x)**-1**,0) = **2**; x[-1:] == x[**2**:])
x[-4:]          // 'abc'     (max(len(x)**-4**,0) = **0**; x[-4:] == x[**0**:])'a' + x[1:]     // 'abc'
'a' + 3 * x[1:] // 'abcbcbc'[s **for** s **in** x]                 // ['a', 'b', 'c']
[s **for** s **in** x **if** s != 'b']     // ['a', 'c']s = '1=2'
a = s.**split**('=')
a[0]                           // '1'
a[1]                           // '2'p = ' a '       
p.**strip**()       // 'a'
p.**lstrip**()      // 'a '
p.**rstrip**()      // ' a's = '${p:.2f}'   
s.**format**(p=100)     // '$100.00'
```

# 列表

```
x = ['a','b','a','c']x.**count**('a')        // 2
x.**count**('e')        // 0
x.**index**('a')        // 0
x.**index**('a',1)      // 2      (at index 1 or greater)
x.**index**('a',2)      // 2      (at index 2 or greater)
x.**index**('a',3)      // 🔥'a' **in** x            // True
'e' **in** x            // Falsex.**reverse**()     
x                   // ['c', 'a', 'b', 'a']
x.**sort**()        
x                   // ['a', 'a', 'b', 'c']
x.**pop**()             // 'c'  
x                   // ['a', 'a', 'b']
**len**(x)              // 3
x.**remove**('a')   
x                   // ['a', 'b']x.**remove**('e')       // 🔥
x.**append**('c')
x                   // ['a', 'b', 'c']
x.**insert**(**2**,'a')     
x                   // ['a', 'b', 'a', 'c'] (insert at index 2)

y = x[:]       
y.**pop**()             // 'c'
y.**pop**()             // 'a'
x                   // ['a', 'b', 'a', 'c']
y                   // ['a', 'b']y[0]                // 'a'
y[1]                // 'b'
y[2]                // 🔥y[-1]               // 'b'
y[-2]               // 'a'
y[-3]               // 🔥y[:]                // ['a', 'b']y[0:]               // ['a', 'b']
y[1:]               // ['b']
y[2:]               // []
y[3:]               // []y[-1:]              // ['b']
y[-2:]              // ['a', 'b']
y[-3:]              // ['a', 'b']x[**2**:] = []           (x[**2**:] -> remove all after **2**nd comma)
x                    // ['a', 'b']     
x = []          
x                    // []**del** y[1]        
y                    // ['a']
**del** y[:]        
y                    // []x + ['d','e']        // ['d', 'e']
x                    // []
z = x + ['d','e']
z                    // ['d', 'e']
z[2:] = ['f','g']    
z                    // ['d', 'e', 'f', 'g']
z[0:1] = ['a','b']
z                    // ['a', 'b', 'e', 'f', 'g']
z[-2:] = []      
z                    // ['a', 'b', 'e']
z[2] = ['e1','e2']     
z                    // ['a', 'b', ['e1', 'e2']]
z[0]                 // 'a'
z[1]                 // 'b'
z[2][0]              // 'e1'
z[2][1]              // 'e2'p = [5,1,2]
**max**(p)               // 5
**min**(p)               // 1
**sum**(p)               // 8
**sum**(p,1)             // 9       (sum(p,**1**) -> sum(p) **+ 1**)q = ['c','a','b']
**max**(q)               // 'c'
**min**(q)               // 'a'**all**([True,True])     // True
**all**([True,False])    // False
**any**([True,False])    // True
**any**([False,False])   // Falsef = ['a','b']
g = [1,2]
**for** f,g **in** **zip**(f,g):    (think of zip fasten LHS+RHS at same levels)
    print('{0}:{1}'.**format**(f,g))     // a:1
                                     // b:2
```

## 关于列表理解和迭代的更多信息

```
[0] * 3                                 // [0, 0, 0]
[None] * 2                              // [None, None][a ** 2 **for** a **in** range(3)]              // [0, 1, 4]
[a ** 2 **for** a **in** range(1,4)]            // [1, 4, 9]
[(a, a ** 2) **for** a **in** range(1,4)]       // [(1, 1), (2, 4), (3, 9)]
[int(x) for x in ['1','2','3']]         // [1, 2, 3]list(**map**(**int**, ['1','2','3']))           // [1, 2, 3]
list(**map**(**str.capitalize**, ['aa','bb']))  // ['Aa','Bb']
list(**map**(**pow**,[1,2,3],[1,2,3]))          // [1, 4, 27] (range(1,4) -> **[**1,2,3,**4)** = **[**1,2,3**]**)[a for a in range(1,4)]                 // [1, 2, 3]
[a for a in range(1,4**,1**)]               // [1, 2, 3]
[a for a in range(1,4**,2**)]               // [1, 3]
[a for a in range(3,**0,-1**)]              // [3, 2, 1]
[a for a in range(3,**0,-2**)]              // [3, 1]
[a for a in range(3,**1,-1**)]              // [3, 2]
[a for a in range(3,**2,-1**)]              // [3]
[a for a in range(**2**,0,-1)]              // [2, 1] (**start 2**, back 1s)
[a for a in range(**1**,0,-1)]              // [1]    (start 1, back 1s)
[a for a in range(**2**,0,**-2**)]              // [2]    (start 2, back 2s)[(a, b) **for** a **in** [1,2] **for** b **in** [2,3]]  
// [(1, 2), (1, 3), (2, 2),(2, 3)][(a**2, b**3) **for** a **in** [1,2,3] **for** b **in** [2,3,4] **if** a == b]
// [(4, 8), (9, 27)]x = [5,1,2]
[a **for a in** x **if** a > 1]           // [5, 2]
[a **for** a **in** x **if** a % 2 == 0]      // [2]it = **iter**(x)
**next**(it)                                // 5
**next**(it)                                // 1
**next**(it)                                // 2
**next**(it)                                // 🔥it = **iter**(x)
**while**(1):
    val = **next**(it, **-1**)
    if val == -1:
        break
    else:
        print(val)                      // 5
                                        // 1
                                        // 2
```

## 堆

如何像 LIFO 栈一样使用链表？

```
s = ['a','b']
s                    // ['a','b']
s.**append**('c')
s                    // ['a', 'b', 'c']
s.**pop**()              // 'c'
s                    // ['a','b']
```

## 长队

如何像 FIFO 队列一样使用列表？

```
from **collections** import **deque**q = **deque**(['a','b'])
q                    // deque(['a', 'b'])
q.**append**('c')
q                    // deque(['a', 'b', 'c'])
q.**popleft**()          // 'a'
q                    // deque(['b', 'c'])
**list**(q)              // ['b', 'c']
```

# 元组

```
x = 'a','b',5
x                    // ('a','b', 5)
x[0]                 // 'a'
x[1]                 // 'b'
x[2]                 // 5x[1:]                // ('b', 5)
x[1:2]               // ('b',)
x[-1:]               // (5,)
x[-2:]               // ('b', 5)
x[-3:]               // ('a', 'b', 5)
x[:]                 // ('a', 'b', 5)t[1] = 'c'           // 🔥
**del** x[2]             // 🔥x + (6,)             // ('a', 'b', 5, 6)
x                    // ('a', 'b', 5)
z = x + (6,)         
z                    // ('a', 'b', 5, 6)p = ([1,2],['d','e'])
p[0] = [3, 4]        // 🔥
p[0][0] = 3
p[0][1] = 4          
p                    // ([3, 4], ['d', 'e'])
p[1][:] = ['f','g']
p                    // ([3, 4], ['f', 'g'])q = (5,1,2)
**len**(q)               // 3
**min**(q)               // 1
**max**(q)               // 5r = **tuple**([1,2])
r                    // (1, 2)f,g = r    
f                    // 1
g                    // 2[a **for** a **in** q **if** a > 1]    // [5, 2]([0] * 3**,**) * 2       // ([0, 0, 0], [0, 0, 0])t = ('a','b')
u = ','.join(t)
u                    // 'a,b'
```

# 设置

```
x = {'a','b'}
'a' in x             // True
'c' in x             // Falsey = **set**('abccc')
y                    // {'a', 'c', 'b'}   (no duplicates)y - x                // {'c'}
x - y                // set()
x | y                // {'b', 'a', 'c'}
x & y                // {'b', 'a'}        (common elements)
x ^ y                // {'c'} x[0]                 // 🔥x.**add**('c')           
x                    // {'c', 'b', 'a'}x.**discard**('a')
x                    // {'c', 'b'}x.**add**('b')
x                    // {'c', 'b'}        (no duplicates)
```

# 字典

```
x = {'a': 5, 'b': 10}x['a']               // 5
x['c']               // 🔥'a' **in** x             // True
'c' **in** x             // Falsex.**keys**()             // dict_keys(['a', 'b'])
x.**values**()           // dict_values([5, 10])
x.**items**()            // dict_items[('a', 5), ('b', 10)]list(x.**keys**())       // ['a', 'b']
list(x.**values**())     // [5, 10]
list(x.**items**())      // [('a', 5), ('b', 10)]for a,b in x.**items**():
    print(a,b)           // 'a 5'
                         // 'b 10'for a,b in **enumerate**(x):
    print(a,b)           // '0, a'
                         // '1, b'**len**(x)               // 2x.**get**('a')           // 5
x.**get**('c')           //
x.**get**('c',-1)        // -1y = x.**copy**()
y['b'] = 11
x                    // {'a': 5, 'b': 10}
y                    // {'a': 5, 'b': 11}y.**update**({'b':15})
y                    // {'a': 5, 'b': 15}y.**update**({'c':20})
y                    // {'a': 5, 'b': 15, 'c': 20}y['d'] = 25
y                    // {'a': 5, 'b': 11, 'c': 20, 'd': 25}**del** y['d']
y                    // {'a': 5, 'b': 11, 'c': 20}y.**clear**()
y                    // {}s = ('a','b')
p = **dict**.**fromkeys**(s)
p                    // {'a': None, 'b': None}
p = **dict**.**fromkeys**(s,-1)
p                    // {'a': -1, 'b': -1}z = **dict**(a=5, b=10)
z                    // {'a': 5, 'b': 10}x == p               // False
x == z               // True
```

# 一些基本算法

## 检查一个数是否是质数

```
import mathdef is_prime(n):
  for i in range(2, **int(math.sqrt(n)) + 1**):
    if (n%i) == 0:
      return False
  return Trueprint(is_prime(11))    // True
print(is_prime(12))    // False
```

## 有效地列出一个范围内的质数

```
import mathdef primes(lower, upper):
    primes = []
    for num in range(lower, upper + 1):
        for i in range(2, **int(math.sqrt(num)) + 1**):
           if (num % i) == 0:
               break
        else:
           primes.append(num)
    return primesprint(primes(10,20))    // [11, 13, 17, 19]
```

## 斐波那契数列中的第 n 个数(递归)

```
def F(n):
    if n == 0: return 0
    elif n == 1: return 1
    else: **return F(n-1) + F(n-2)**F(3)     // 2
```

## 获取前 n 个斐波那契数列

```
def f(n):
    nums = []
    a, b = 0, 1
    for i in range(n):
        a, b = b, a + b
        nums.append(a)
    return numsprint(f(5))   // 5
```

## 打印最后一位数字

```
n = 123
print(str(n)[-1])           // 3
```

## 将数字提取到列表中

```
a = '123'
b = [**int**(i) for i in **str**(a)]          // [1, 2, 3]
```

## 计算一个数字的位数

```
a = 123
count = 0**while** a != 0:
    a //= 10
    count += 1print(count)         // 3
```

或者

```
**len**(**str**(a))          // 3
```

或使用对数(正/负):

```
import mathdef digits(n):
    if n > 0:
        return int(math.**log10**(n))+1
    elif n == 0:
        return 1
    else:
        return int(math.**log10**(**-n**))+1print(digits(-10))          // 2
print(digits(0))            // 0
print(digits(123))          // 3
```

## 递归打印前 N 个自然数

```
def p(n):
   if n > 0:
      p(n - 1)
      print(n, end = ' ')p(3)                        // 1 2 3
```

## 递归计算数字的和

```
def s(n):
    if n == 0:
        return 0
    return n % 10 + s(n // 10)

print(s(123))               // 6
```

## 反向字符串速记

```
s = 'abc'
print(s[::-1])              // 'cba'
```

## 按升序排列的一个数的唯一除数

```
import mathn = 36unique_divisors = **set()**
i = 1
while i < math.sqrt(n):
    if n % i == 0:
        unique_divisors.add(i) if n / i != i:
            unique_divisors.add(int(n/i)) i += 1print(sorted(unique_divisors))       // [1, 2, 3, 4, 9, 12, 18, 36]
```

## 最大公约数

```
def gcd(x, y):
   while(y):
       x, y = y, x % y
   return xprint(gcd(4, 6))          // 2
```

## 最低公倍数

```
def lcm(x, y):
   lcm = (x * y) // **gcd**(x,y)
   return lcmprint(lcm(4, 6))          // 12
```

## 数字的阶乘(迭代)

```
def fac(n):
    result = 1
    while n > 1:
        result *= n
        n -= 1
    return resultprint(fac(3))             // 6
```

## 数字的阶乘(递归)

```
def fac(n):
    if n == 1:
       return n
    else:
       return n * fac(n-1)print(fac(3))             // 6
```

## 递归地将两个数相乘

```
def m(x,y):
    if  y == 0 :
        return 0
    elif y < 0:
        return -(x - m(x,y+1))
    else:
        return x + m(x,y-1)print("3 * 2 = " , m(3,2))         // 3 * 2 =  6
print("3 * (-2) = ", m(3,-2))      // 3 * (-2) =  -6
print("(-3) * 2 = ", m(-3,2))      // (-3) * 2 =  -6
print("(-3) * (-2)= ", m(-3,-2))   // (-3) * (-2)=  6
```

感谢阅读！请在下面的评论区告诉我你的想法，别忘了订阅哦👍