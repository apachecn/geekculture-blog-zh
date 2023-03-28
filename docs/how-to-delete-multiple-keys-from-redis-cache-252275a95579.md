# 从 Redis 缓存中删除多个键的方法。

> 原文：<https://medium.com/geekculture/how-to-delete-multiple-keys-from-redis-cache-252275a95579?source=collection_archive---------7----------------------->

![](img/1902b77bffc1d8ef78ccd04e0e33afca.png)

Photo by [Ujesh Krishnan](https://unsplash.com/@ujesh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/delete?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Redis 缓存键可以用两种方法删除。

## **用按键命令**

Keys 命令将扫描 Redis 数据库中存在的所有键，匹配的模式作为单次输入提供。如果您有数百万个密钥，扫描生产 Redis 中的所有密钥将需要时间。众所周知，Redis 是单线程的，扫描 prod Redis 缓存中的所有键会阻止其他 Redis 命令运行。因此，我们需要避免在 prod Redis 实例上使用 keys 命令。

## 带有 Lua 脚本的扫描命令。

因为`**Scan**`命令允许增量迭代，每次调用只返回少量元素。默认情况下，扫描以 10 个为一组返回密钥。如果您想要多于或少于 10，您可以使用 COUNT 来指定。请确保不要给计数值太大。如果你给的计数值太大，那么它将只作为按键命令工作。我们可以将扫描命令与 Lua 脚本结合起来，以利用 Lua 脚本的优势。下面是使用 Lua 脚本进行 Redis 操作的一些优点。

[Lua 脚本的好处](https://developpaper.com/benefits-and-sample-code-of-redis-executing-lua-scripts/) **:**

1.减少网络开销:原来五个网络请求的操作可以用一个请求完成，原来五个请求的逻辑可以在 Redis 服务器上完成。脚本的使用减少了网络往返延迟。

2.原子操作:Redis 会将整个脚本作为一个整体执行，不会被其他命令插入。

3.重用:客户端发送的脚本将永久存储在 Redis 中，这意味着其他客户端可以重用该脚本，而无需使用代码来完成相同的逻辑。

现在让我们看看如何用 both 方法删除这个键。

*   **通过按键命令** :-

我们可以使用如下的 keys 命令从 Redis 中删除所有匹配给定模式"`user*"` 的键。

```
**redis-cli -h {host}-p {port} — raw keys “**users***” |** **xargs redis-cli DEL**
```

**注意:-** 不建议在生产 Redis 实例上使用。

*   **用 Lua 脚本扫描命令删除缓存键:-
    这个**
    我写过 Lua 脚本按模式删除多个键。脚本使用 scan 命令在增量迭代中删除 Redis 缓存键。请按照以下步骤，通过带有扫描命令的 Lua 脚本删除带有模式的 Redis 键。
*   在你的机器上复制下面的 Lua 脚本，并用。lua 扩展。

```
local cursor="0";
local count = 0;
repeat
 #replace "REPLACE_ME_IAM_KEY_PATTERN" with your key pattern that you want to delete
 local scanResult = redis.call("SCAN", cursor, "MATCH", "REPLACE_ME_IAM_KEY_PATTERN", "COUNT", 100);
	local keys = scanResult[2];
	for i = 1, #keys do
		local key = keys[i];
		redis.replicate_commands()
		redis.call("DEL", key);
		count = count +1;
	end;
	cursor = scanResult[1];
until cursor == "0";
return "Total "..count.." keys Deleted" ;
```

*   在本地创建文件后。请确保您对要删除的密钥模式进行了更改。(这一步对于验证您是否要从 Redis 中删除预期的密钥非常重要)
*   请 CD 到你创建 Lua 脚本的文件夹
*   通过替换适当的主机，用下面的命令运行 Lua 脚本。
*   **Comamnd:-redis-CLI-h**[**{**](http://dev-acminterface-new.5gksuj.ng.0001.use1.cache.amazonaws.com)**host }-eval Delete-keys . Lua**
*   命令将返回被 Lua 脚本删除的键的总数。
    示例:- **总计“删除 100 个键”**

**注意:请注意，EVAL 命令还会阻塞 Redis 实例，直到完成 lua 脚本。**

感谢您的阅读…！！！☺️

[Lua 脚本的优势](https://developpaper.com/benefits-and-sample-code-of-redis-executing-lua-scripts/)