# 理解 C 语言中动态内存分配的简易指南

> 原文：<https://medium.com/geekculture/an-easy-guide-to-understand-dynamic-memory-allocation-in-c-programming-language-bb34d29f7a06?source=collection_archive---------9----------------------->

![](img/46d719dbdb960a62f8a7075552a95d15.png)

C 语言的一个奇妙之处在于它与内存如此紧密地交织在一起——我的意思是程序员对“*什么去了*哪里”有很好的理解。通过[](https://en.wikipedia.org/wiki/C_standard_library)**中的一组函数，即`malloc`、`realloc`、`calloc` 和`free` ，可以在 [**C 编程语言**](https://en.wikipedia.org/wiki/C_(programming_language)) 中对 [**进行手动内存管理、动态内存分配**](https://en.wikipedia.org/wiki/Dynamic_memory_allocation) 。**

# **c 有三个主要的内存池:**

1.  ****静态:** *全局变量存储，永久用于程序的整个运行。***

```
static int static_No;
```

**这是存储已声明为全局或静态的变量以及那些字符串常量(例如`"My string"`)的区域。在这个内存区域中，你可以找到从程序开始到执行结束的所有数据。**

**2.**堆栈** : *局部变量存储(自动、连续记忆)。***

```
**int** main(**void**){**int** i; /* i is only visible/usable inside main()*/**return** 0;}
```

**内存堆栈是一个区域，在程序执行过程中，变量会在某个点出现和消失。我们主要用它来存储函数的局部变量。这些变量有一个缩小的范围，它们只在函数(我们已经定义了它们)被执行时可用。所有这些变量都存储在堆栈中，因此，该区域不断接收插入和删除变量的操作。**

****3。堆** : *动态存储(大内存池，不按连续顺序分配)*。**

**该区域包含在程序执行过程中的任何时候可用于*保留*和*释放*的内存。它不像在堆栈中那样为局部变量函数保留，而是为“*动态内存*”保留，用于在程序执行之前不知道需要的数据结构。这就是`malloc()`和`free()`的位置。**

> **注意，在这三个内存区域中，只有全局内存有一个固定的大小，这个大小在程序开始执行时是已知的。堆栈和堆都存储数据，其大小只有在程序执行时才能知道。**

# **先说 malloc()和 free()**

**假设您想在应用程序的执行过程中分配一定的内存。你可以随时调用`malloc()`函数，它会从*堆*中请求一块内存。操作系统将*为你的程序保留*一块内存，你可以用任何你喜欢的方式使用它。当你使用完这个块后，你通过调用`free()`函数将*返回给操作系统进行回收，然后其他应用程序可以保留它供自己使用。***

**C 语言中的`**malloc()**`或**【内存分配】**方法用于动态分配指定大小的单个大内存块。它返回一个 void 类型的指针，可以转换成任何形式的指针。**

```
/* Let's see the abracadabra of malloc and free in this code*/int main()
{
	int *p; 

	p = (int *)malloc(sizeof(int)); /*requested for 4 bytes */
	if (p == 0)
	{
		printf("ERROR: Out of memory\n"); 
		return 1; 
	}
	*p = 5;
	printf("%d\n", *p);
	free(p); /*free the requested memory for int *p */
	return 0;
}
```

## **上述代码中发生的关键过程是:**

1.  **malloc 语句首先查看*堆*上的可用内存量，并询问*“是否有足够的可用内存来分配所请求大小的内存块？”*从传递给 malloc 的参数中可以知道该块所需的内存量——在本例中，`**sizeof(int)**`是 4 个字节。如果没有足够的可用内存，malloc 函数返回地址零来显示错误(零的另一个名字是`NULL`，你会看到它在整个 C 代码中使用)。否则 malloc 继续。**
2.  **如果*堆*上有可用内存，系统会从指定大小的堆中“分配”或“保留”一块内存，这样就不会被一个以上的`malloc` 语句意外使用。**
3.  **然后，系统将保留块的地址放入指针变量中(本例中为`p`)。`*p`本身包含一个地址。分配的块可以保存指定类型的值，指针指向它。**

**程序接下来检查指针 p 以确保分配请求通过行`**if (p == 0)**`(也可以写成 `**if (p == NULL)**`或者甚至`**if (!p)**`)成功。如果分配失败(如果`p`为零)，程序结束。如果分配成功，则程序将该块初始化为值 5，打印出该值，并在程序结束前调用 free 函数将内存返回到*堆*。**

**C 中的`**free()**` 用于动态地**去分配**内存。使用功能`malloc()`和`calloc()`分配的内存不会自行取消分配。因此，每当动态内存分配发生时，就使用 `free()`方法。它有助于通过释放内存来减少内存浪费。要释放的变量在括号中传递。参见上面的代码块。**

**`malloc()`有一对双胞胎兄弟`calloc()` 和双胞胎姐妹`realloc()`，让我们看看这对兄妹给我们带来了什么**

# **卡洛克( )**

**C 中的`calloc` 或**连续分配**方法用于动态分配指定类型内存的指定块数。它与`malloc()`非常相似，但有两点不同，它们是:
i)它用默认值‘0’初始化每个程序块。
ii)与`malloc()`相比，它有两个参数或自变量**

```
/* n = number of elements
 * element_size = the size of each element
 */ptr = (cast-type*)calloc(n, element_size);
```

# **realloc()**

**该函数试图通过调用`malloc` 或`calloc`来调整*由指针指向的*存储块的大小**

```
void *realloc(void *ptr, size_t size)
```

*   ****ptr**——这是指向先前分配有`malloc`、`calloc`或`realloc` 的内存块的指针，该内存块将被重新分配。如果为空，则分配一个新的块，并由函数返回一个指向它的指针。**
*   ****大小**—这是存储块的新大小，以字节为单位。如果它是 0 并且指针指向一个现有的内存块，那么指针所指向的内存块将被释放并返回一个空指针。**

# **注意**

****每次分配后检查指针是否为零非常重要。**因为堆的大小不断变化，这取决于哪些程序正在运行，它们分配了多少内存等等。，永远不能保证对`malloc` 的调用会成功。您应该在任何对`malloc` 的调用之后检查指针，以确保指针有效。**

****在程序结束前删除** `**malloc**` **ed 内存块。**当一个程序结束时，操作系统“在它之后清理”，释放它的可执行代码空间、堆栈、全局内存空间和任何堆分配，以便回收。因此，在项目终止时将分配留在未决状态不会产生长期后果。但是，我认为它不是最好的编码技术，程序执行过程中的“内存泄漏”是有害的。**

**我希望这个指南能让你更容易理解这个概念。**

## **更新:**

*   ***我所有即将发表的技术文章都将放在* [*我的 Hashnode 博客*](https://betascribbles.hashnode.dev/)**
*   ***该中型账户仅包含非技术岗位***

# **你喜欢我的文章吗？多吃点**

**[](/geekculture/how-i-tackle-my-projects-at-alx-holberton-school-610f3f5a6448) [## 我如何处理我在 alx-Holberton 学校的项目

### 按时完成项目是一回事，掌握项目的概念是另一回事。但是您可以同时管理这两者…

medium.com](/geekculture/how-i-tackle-my-projects-at-alx-holberton-school-610f3f5a6448) [](https://betascribbles.medium.com/why-programmers-choose-linux-over-windows-8f556c303b14) [## 为什么程序员选择 Linux 而不是 Windows

### 大多数程序员选择在 Linux 环境中工作，因为它有很多优点。然而，这并不能概括…

betascribbles.medium.com](https://betascribbles.medium.com/why-programmers-choose-linux-over-windows-8f556c303b14) [](https://betascribbles.medium.com/my-thoughts-on-the-alchemist-by-paulo-coelho-baf6e70dfb73) [## 我对保罗·柯艾略《炼金术士》的看法

### “过好你的生活”通常与“实现你的人生梦想”相混淆。这本书让我意识到了不同…

betascribbles.medium.com](https://betascribbles.medium.com/my-thoughts-on-the-alchemist-by-paulo-coelho-baf6e70dfb73) [](https://betascribbles.medium.com/best-app-for-the-best-reading-experience-bc30747603a6) [## 最佳阅读体验的最佳应用

### 很长一段时间以来，成为一名书迷一直是我最喜欢的事情。我每年阅读数百本书。就像我一样…

betascribbles.medium.com](https://betascribbles.medium.com/best-app-for-the-best-reading-experience-bc30747603a6)**