# 拉勒维尔教程~拉勒维尔雄辩的关系

> 原文：<https://medium.com/geekculture/laravel-tutorial-laravel-eloquent-relationships-ec62367e8940?source=collection_archive---------5----------------------->

![](img/39ff19c86b6ed8ccec917e82d5a68500.png)

你好，朋友们你们好吗，希望你们永远健康成功。

这次我们将讨论雄辩表之间的关系。这个功能会让你更容易让关系变得更简洁简单。如果我们使用这个 SQL 命令，它将使表之间的关系更长。让我们在雄辩上讨论关系。

在我继续之前，可能会有一些朋友感兴趣的教程。

[使用 Laravel 中的范围](https://temanngoding.com/menggunakan-scope-di-laravel/)

[使用 Laravel 中的软删除完成教程](https://temanngoding.com/tutorial-lengkap-menggunakan-soft-delete-di-laravel/)

[如何在拉韦尔使用播种机](https://temanngoding.com/cara-menggunakan-seeder-di-laravel/)

Laravel 数据库关系

1.  一对一关系，其中一个表中的数据只与另一个表中的数据有关系。例如，数据表 tb_User 在 tb_Contact 表中有一个关系 1 电话号码。
2.  一对多关系，其中一个表中的一个数据与另一个表中的一些数据有关系。例如，数据表 tb_Category 与 tb_Inventory 中的许多数据项有关系。或者换句话说，1 品类有很多库存数据。
3.  多对一关系(一对多逆关系)是一对多关系的反义词。例如，我们想知道 tb_Inventory 中的项目数据属于哪个类别，那么将使用这个关系。
4.  多对多关系，其中一个表中的许多数据与其他表中的许多数据也有关系。这种关系是通过辅助表形成的。例如，tb_Student 表中的许多记录与 tb_Buku 表中的许多数据有借用关系。这种关系由一个名为 tb_Transaksi 的帮助表构成。

# 一对一的雄辩关系

这是最基本的关系。例如:每个用户有 1 部手机。传递给 hasOne 方法的第一个参数是相关的模型名称。

```
public function profile():
{
    return $this->hasOne(Profile::class);
}
```

hasOne 函数现在希望在概要文件模型中有一个用户 id 字段。如果您的列名不同，添加第二个参数到与其他列名有一个关系的**:**

```
return $this->hasOne(Profile::class, 'foreign_key');
```

*   外键
    口才根据型号名称确定外键。在这种情况下，假设概要文件模型有一个 **foreign_key** 默认 author_id。
    指定一个外键，其中默认的用户和作者关系是 user_id。要手动设置外键，可以将其写为第二个参数。

```
return $this->hasOne(Profile::class, 'foreign_key','local_key');
```

*   反向关系外键。在前面的例子中，雄辩试图将手机型号的 user_id 与用户型号的 id 相匹配。默认的外键名是关系的方法名加上 _id 后缀，因此在本例中是 user_id。
    如果不匹配，可以写在 belongsTo 的第二个自变量中:

```
public function profile():
{
    return $this->belongsTo(Profile::class);
}
```

*   反关系主键
    Apabila dii ginka relais 不支持默认主键，参数 ketiga 中的参数 dapat dituliskan 属于:

```
return $this->belongsTo(Profile::class, 'foreign_key','local_key');
```

# 雄辩的一对多关系

另一件非常重要，甚至可能是最重要的事情是一对多关系。也被称为 **hasMany-relationship** ，这种关系定义了“一个项目有许多其他项目”的关系。这个关系和上面的很像。

继续我们的博客例子，假设一个档案有多篇文章。转到您的概要文件模型，并添加以下方法:

```
public function posts(): HasMany
{
    return $this->hasMany(Post::class);
    //Or: return $this->hasMany(Post::class, 'foreign_key');
}
```

*   外键 dan 本地键
    一对多**中外键和主(本地)键的使用也类似于**一对一**。**

```
return $this->hasMany(Post::class, 'foreign_key');
return $this->hasMany(Post::class, 'foreign_key','local_key');
```

# 多对多

这种关系也被广泛应用。它的用法比前两种关系稍微复杂一些。
下面的例子是有用户、角色和 role_user 的地方。一个用户可以有多个角色，一个角色可以有多个用户。
role_user 是默认的中间表或透视表。表名来自角色和用户表，按字母顺序排列
关系使用 **belongsToMany** 方法定义。

```
public function profiles(): BelongsToMany
{
    return $this->belongsToMany(Profile::class);
}
```

*   数据透视表
    如前所述，数据透视表名称由两个表名生成，并按字母顺序排序。如果需要，可以在任何方法下面的**的第二个参数中定义。**

```
return $this->belongsToMany(Profile::class,'role_user');
```

除了定义数据透视表名称之外，与其他类型的关系一样，每个表的键也可以定义如下:

```
return $this->belongsToMany(Profile::class,'role_user','user_id','role_id');
```

如果数据透视表中有其他字段/列，并通过写入 **withPivot** 方法来定义。

```
return $this->belongsToMany(Profile::class)->withPivot('column1','column2');
```

这就是我这次可以传达的教程，希望有用。

谢谢你……