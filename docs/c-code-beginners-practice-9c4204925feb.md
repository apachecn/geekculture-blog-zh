# c 初学者，嵌套 For 循环

> 原文：<https://medium.com/geekculture/c-code-beginners-practice-9c4204925feb?source=collection_archive---------9----------------------->

更多 C 代码初学者练习用解答。这次我们关注嵌套循环。关于基本 for 循环的第一部分是这里的。

![](img/c52894ce236c83b032897a9c209cfcaa.png)

# 嵌套 For 循环

循环可以嵌套。也就是说，你可以在 for 循环中使用 for 循环，深度不限。通常超过 2-3 个巢，很难跟踪。下面是一个简单的例子，你可以用它来理解嵌套:

```
for (i=1; i <=n ; i++) {

    printf("\n"); 

    for (j=1; j <=i ; j++) {

       printf(" %d", j); 
    }
}
```

n = 5 时的输出:

```
Enter a positive number n: 5 1
 1 2
 1 2 3
 1 2 3 4
 1 2 3 4 5
```

另一个简单的例子:

```
for (i=1; i <=n ; i++) {

    printf("\n ** starting outer loop i = %d \n ", i); 

    for (j=1; j <=n ; j++) {

       printf(" (i, j) = (%d, %d) ", i, j); 
    }
}
```

输入 n=4 的输出:

```
Enter a positive number n: 4** starting outer loop i = 1 
  (i, j) = (1, 1)  (i, j) = (1, 2)  (i, j) = (1, 3)  (i, j) = (1, 4) 
 ** starting outer loop i = 2 
  (i, j) = (2, 1)  (i, j) = (2, 2)  (i, j) = (2, 3)  (i, j) = (2, 4) 
 ** starting outer loop i = 3 
  (i, j) = (3, 1)  (i, j) = (3, 2)  (i, j) = (3, 3)  (i, j) = (3, 4) 
 ** starting outer loop i = 4 
  (i, j) = (4, 1)  (i, j) = (4, 2)  (i, j) = (4, 3)  (i, j) = (4, 4)
```

# 练习

对于每个练习，都给出了预期的输出。你的任务是产生相同的输出。

1.  打印一串数字。每个数字打印#次。

```
Enter a positive number n: 6
 1
 2 2
 3 3 3
 4 4 4 4
 5 5 5 5 5
 6 6 6 6 6 6
```

2.打印给定尺寸的起点方块。

```
Enter a positive number n: 5
*****
*   *
*   *
*   *
*****
```

3.按以下格式打印倒置的三角形数字:

```
Enter a positive number n: 6
 1 2 3 4 5 6
 1 2 3 4 5
 1 2 3 4
 1 2 3
 1 2
 1
```

4.对于给定的数字，打印下面的数字三角形。

```
Enter a positive number n: 6
 6
 6 5
 6 5 4
 6 5 4 3
 6 5 4 3 2
 6 5 4 3 2 1
```

5.检查一个给定的数是否是原始的，如果不是，打印它的所有分母。对于另一种变化，添加分母数量的打印输出。

```
Enter a positive number n: 8
4 is denominator of 8
 4 * 2 = 8
2 is denominator of 8
 2 * 4 = 8
```

或者

```
Enter a positive number n: 7
7 is primal, has no denominators
```

6.以给定的大小打印 X 的 X 形状

```
Enter a positive number n: 9
x      x 
 x    x  
  x  x   
   xx    
   xx    
  x  x   
 x    x  
x      x
```

7.练习 6 的变化，在印刷品上添加一个框架。

```
Enter a positive number n: 11
xxxxxxxxxxx
xx      x x
x x    x  x
x  x  x   x
x   xx    x
x   xx    x
x  x  x   x
x x    x  x
xx      x x
xxxxxxxxxxx
```

要编写和运行 C 代码，您可以使用在线 C 编译器网站。我用 https://www.onlinegdb.com/online_c_compiler 的，它很棒。

# 解决方法

解决方案 1

```
for (i=1; i <=n ; i++) {

    for (j=1; j <=i ; j++) {

       printf(" %d", i); 
    }
    printf("\n");
}
```

解决方案 2

```
for (i=1; i <=n; i++) {

    printf("*");
}
printf("\n");
for (i=2; i <=n-1; i++) {
    printf("*");
    for (j=2; j <=n-1 ; j++) {

       printf(" "); 
    }
    printf("*\n");
}
for (i=1; i <=n; i++) {

    printf("*");
}
```

解决方案 3

```
for (i=n; i >= 1; i--) {

    for (j=1; j <=i ; j++) {

       printf(" %d", j); 
    }
    printf("\n");
}
```

解决方案 4

```
for (i=1; i <=n; i++) {

    for (j=1; j <=i ; j++) {

       printf(" %d", n - j +1); 
    }
    printf("\n");
}
```

解决方案 5

```
 int i,j,n, is_primal = 1;printf("Enter a positive number n: "); 
scanf("%d",&n);for (i=2; i <n ; i++) {
    for (j=1; j <=n ; j++) {

        if (j*i == n){

            printf("%d is denominator of %d\n ", j, n); 
            printf("%d * %d = %d\n", j,i, n); 
            is_primal = 0;
            break;

         }
     }
}
if (is_primal) {
    printf("%d is primal, has no denominators \n ", n); 
}
```

解决方案 6

```
for (i=1; i <=n-1; i++) {

    for (j=1; j <=n ; j++) {

       if (j==i | j==n-i){
         printf("x");   
       } else{
         printf(" "); 
       }
    }
    printf("\n");
}
```

解决方案 7

```
for (i=1; i <=n-1; i++) {

    for (j=1; j <=n ; j++) {

       if (j==i | j==n-i | i==1 | j==1 | j==n | i==n-1){
         printf("x");   
       } else{
         printf(" "); 
       }
    }
    printf("\n");
}
```

[c-初学者-循环练习](https://naomi-fridman.medium.com/c-beginners-practice-for-loop-4933a368d966)<-上一章简单的循环