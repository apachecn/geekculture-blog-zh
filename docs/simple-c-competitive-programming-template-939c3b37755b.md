# 简单的 C++竞争性编程模板

> 原文：<https://medium.com/geekculture/simple-c-competitive-programming-template-939c3b37755b?source=collection_archive---------0----------------------->

我一直在寻找一段标准的冗余代码，我必须在我的每个 C++程序中编写这段代码。在搜索的时候，我发现了很多，其中一些真的很好，但是它们有很多不必要的模板类和太多我认为没有多大用处的函数。

所以在看完所有这些之后，我为自己创建了一个模板。我分享这些，希望对其他人有益:

```
#include <bits/stdc++.h>using namespace std;#define read(*type*) readInt<*type*>() // Fast read#define ll long long#define nL "\n"#define pb push_back#define mk make_pair#define pii pair<int, int>#define a first#define b second#define vi vector<int>#define all(*x*) (x).begin(), (x).end()#define umap unordered_map#define uset unordered_set#define MOD 1000000007#define imax INT_MAX#define imin INT_MIN#define exp 1e9#define sz(*x*) (int((x).size()))int32_t main(){ ios_base::sync_with_stdio(false); cin.tie(NULL); int ttt; cin >> ttt; while(ttt--) { } return 0;}
```

代码将针对给定的样本测试用例运行

我使用了“int32_t main()”而不是“int main()”，就好像你编写了使用“int”变量的程序，但后来你意识到你必须用“long long”来增加范围。

```
So intead of,
#define ll long long
use : 
#define int long long
```

这将节省您更改每个 int 变量的时间，但是没有“long long”主函数，所以最好将它改为

```
int32_t main()
instead of : int main()
```

我使用了“IOs _ base::sync _ with _ stdio(false)；”这个代码片段提高了 cin 和 cout 的执行速度。我已经在这个博客中解释过了

[](/@rajataggarwal2603/dont-use-cin-and-cout-in-c-89e9c70701f5) [## 不要在 C++中使用 cin 和 cout

### 竞争性编程就是这种情况。在竞争中，cin 和 cout 命令往往需要更多的执行…

medium.com](/@rajataggarwal2603/dont-use-cin-and-cout-in-c-89e9c70701f5) 

所有其余的#define 只是任何程序中最常用函数的缩写。