# 如何在上传一个新的大集合之前检查一些数据是否已经存在于数据库中。

> 原文：<https://medium.com/geekculture/how-to-check-whether-some-of-the-data-already-exists-in-the-database-before-uploading-a-new-large-fa9c580afa99?source=collection_archive---------14----------------------->

## 如果您使用基于 HTTP 的 API 来推送数据，建议您进行这项检查。

![](img/78f4bc029030ee0fdda213038b5d30f6.png)

Photo by [Campaign Creators](https://unsplash.com/@campaign_creators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/database-administrators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

进行这种检查可能有多种原因，包括不尝试插入重复数据，从而节省带宽、时间和资源。

# 问题是

您需要将一大组数据插入到数据库表中，并且需要验证成员是否已经存在于表中。

# 解决方案

> 下面的查询是用 MySQL 数据库中的一百万行和 50 万新数据测试的。

1.  选择应该对其进行检查的列。它应该是索引良好的唯一键列或多列唯一键。
2.  创建一个临时表，将类似的列签名作为实际表中的键。

```
CREATE TEMPORARY TABLE my_keys ( new_unique_key INT(11) NOT NULL);
```

3.解析新数据集中的键列，并创建插入 SQL，如下所示

```
INSERT INTO my_keys (new_unique_key) VALUES (key1), (key2), ...;
```

举个例子，

```
INSERT INTO my_keys (new_unique_key) VALUES (201), (258);
```

*检查数据库配置中的查询大小限制。如果键的数量超过了限制，您需要编写单独的 SQL。*

4.在 unique_key 上对临时表和实际表进行左连接，并检查右表(实际表)中的条目是否不为 null。如果不为 null，则意味着实际表中存在 unique_key。结果中将只列出这样的键。

```
SELECT my_keys.new_unique_key FROM my_keys LEFT JOIN actual_table ON actual_table.unique_key = my_keys.new_unique_key WHERE actual_table.unique_key IS NOT NULL;
```

# 自动生成 SQL。

建议使用脚本来生成 SQL。下面是一个 Ruby 示例代码，它从文件中读取密钥并生成。sql 文件。

***注意，下面的代码只是指示性的。***

```
file = File.open("./query.sql", "w")
file.prints "CREATE TEMPORARY TABLE my_keys ( new_unique_key INT(11) NOT NULL);\nINSERT INTO my_keys (new_unique_key) VALUES "count = 0
number_of_rows = 500000 #do `wc -l` in unix/linux/mac File.readlines("./new_data_set").each do |line|
  File.prints "(\""+line.gsub("_pattern_")+"\")" if count < number_of_rows
        file.prints ", "
  end
  #no comma for last one. count += 1
end
#_pattern_ -> regex pattern to extract unique_key partfile.prints ";\nSELECT my_keys.new_unique_key FROM my_keys LEFT JOIN actual_table ON actual_table.unique_key = my_keys.new_unique_key WHERE actual_table.unique_key IS NOT NULL;"

file.close
```

这将创建文件“query.sql”。

您可以使用 MySQL CLI 来运行 SQL 文件。确保将输出发送到一个文件中。

```
mysql>tee output
mysql>source query.sql
mysql>exit
```

***即使表中有大量的行，SQL 也能在一秒钟内运行。***

就是这样。所有重复的密钥将在文件“输出”中列出。现在，您可以使用这些键来过滤批量更新过程。

希望这有所帮助。