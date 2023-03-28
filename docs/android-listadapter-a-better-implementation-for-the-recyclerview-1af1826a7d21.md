# Android: ListAdapter，RecyclerView 的更好实现

> 原文：<https://medium.com/geekculture/android-listadapter-a-better-implementation-for-the-recyclerview-1af1826a7d21?source=collection_archive---------3----------------------->

![](img/9f05bb935a8bfac56b4a6948004f37c5.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 我假设您了解 RecyclerView 及其实现！

几乎我们使用的每个应用程序都有某种列表，无论是垂直列表还是水平列表。RecyclerView 是作为 ListView 和 GridView 的后继者引入的，它一直存在至今，以一种高效的方式呈现任何基于适配器的视图。但是总有改进的余地，**不是吗？**
图中出现了 **ListAdapter** 。根据谷歌文档，
ListAdapter 是 RecyclerView。用于在`[*RecyclerView*](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView)`中显示列表数据的适配器基类，包括在后台线程上计算列表之间的差异。

好，现在什么是计算差异？
大多数时候，只有我们列表的一部分被更新(添加、更新或删除)，而不是整个列表数据，当我们调用`*notifyDataSetChanged()*` *，*时，它刷新整个列表。结果是`*onBindViewHolder()*` 对列表的每一项都被调用；我们的营救行动开始了。

DiffUtils 是围绕`[*AsyncListDiffer*](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/AsyncListDiffer)`的一个方便的包装器，它实现了条目访问和计数的适配器通用默认行为。

AsyncListDiffer 是一个帮助器类，用于通过后台线程上的`[*DiffUtil*](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/DiffUtil)`计算两个列表之间的差异。

足够的理论，让我们开始写一些代码和理解实现。

ListAdapter 实现需要一个**模型**类(在我们的例子中是 User)，一个**recycle view。ViewHolder** 实现，和一个 **DiffUtil。item callback<T>作为构造函数的参数。**

# **RecylerView。视图保持器和 DiffUtil。ItemCallBack < T >片段**

```
// Model class
data class User(val name: String, val age: Int)

// ViewHolder 
class UserViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView){

    fun bind(user: User){
        // use the model to bind data to the views
    }
}
```

以上是一个模型类和一个视图持有者类的基本实现。

```
private class UserDiffCallBack : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem
}
```

在这里，我创建了一个 UserDiffCallBack 类，它实现了 **DiffUtil。ItemCallBack < T >，**其中 T 是我们的模型类，反过来我们需要覆盖两个方法，这两个方法比较项目。

> 这里我使用了`oldItem == newItem`表达式来检查内容是否相同，因为我们使用了默认情况下为`equals()`方法提供实现的数据类。

现在让我们转到 ListAdapter 的实现。

# ListAdapter 代码段

```
class UserRecyclerAdapter :
    ListAdapter<User, UserRecyclerAdapter.UserViewHolder>(UserDiffCallBack()) {

    class UserViewHolder(itemView: View) :
        RecyclerView.ViewHolder(itemView) {

        fun onBind(user: User) {
            // your implementation
        }
    }

    private class UserDiffCallBack : DiffUtil.ItemCallback<User>() {
        override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
            oldItem == newItem

        override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
            oldItem == newItem
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
       // your implementation
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        // your implementation
    }
}
```

除了上面解释的实现，我们需要覆盖旧的`*OnCreateViewHolder*`和`*onBindViewHolder*`方法，就像我们对 RecyclerView 实现所做的那样。

剩下要做的就是初始化适配器，并将列表提供给适配器。

# **初始化适配器**

```
UserRecyclerAdapter userAdapter = UsersAdapter()//rvUsers is the recyclerview rvUsers.*apply* **{** *layoutManager* = LinearLayoutManager(
        this
    )
    *adapter* = userAdapter
**}**

userViewModel.observe(this) **{** userList ->storiesRecyclerAdapter.submitList(userList)
**}**
```

ListAdapter 附带了一个方法`*submitList(List<T> list)*`，用于向适配器提供新的或修改过的数据，并处理所有的 diffs 计算。因此，我们不再需要 setter 方法来设置列表。将在后台线程上计算差异，并将结果通知主线程上的适配器。

> 就是这么实现的！它删除了所有的样板代码，使开发过程更加高效和无缝。