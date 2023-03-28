# Laravel 6+的 EnvClient

> 原文：<https://medium.com/geekculture/envclient-for-laravel-6-b6a5cd99f339?source=collection_archive---------7----------------------->

## 使用 artisan 控制台命令、环境规则和外观管理和验证环境变量

![](img/004e749ab6b957c7ca6af4d65eba1f14.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**Github**:[https://github.com/lionix-team/envclient](https://github.com/lionix-team/envclient)

**包装商:**https://packagist.org/packages/lionix/envclient

# 装置

```
composer require lionix/envclient
```

# Artisan 命令

签名描述`env:get {key}`打印。环境变量值`env:set {key} {value}`设置。环境变量如果验证规则通过`env:check`检查所有环境变量的有效性`env:empty`打印空白。env 变量`make:envrule {name}`创建一个新的。环境验证规则

# 基本用法

使用`env:set` artisan 命令设置环境变量。

```
php artisan env:set EXAMPLE_ENV_VARIABLE "example value"
```

该命令将通过替换或添加给定的密钥来修改环境文件。

# 验证环境变量

如果你想在`env:set`命令修改文件之前对环境变量应用验证规则，你必须发布命令包配置文件。

```
php artisan vendor:publish --provider="Lionix\EnvClient\Providers\EnvClientServiceProvider" --tag="config"
```

该命令将创建`config/env.php`

```
<?phpreturn [/**
     * Validation classes which contain environment rules
     * applied by env artisan commands.
     * 
     * Add your validation classes created by 
     * `php artisan make:envrule` command to apply their rules
     * 
     * @var array
     */
    "rules" => [
        \App\Env\BaseEnvValidationRules::class
    ]
];
```

和`app/Env/BaseEnvValidationRules.php`

```
<?phpnamespace App\Env;use Lionix\EnvValidator;class BaseEnvValidationRules extends EnvValidator
{
    /**
     * Validation rules that apply to the .env variables.
     *
     * @return array
     */
    public function rules() : array
    {
        return [
            //
        ];
    }
}
```

通过将验证规则添加到`rules`方法返回值中，您将把它们应用到`env:set`命令中。

```
...
public function rules() : array
{
    return [
        "DB_CONNECTION" => "required|in:mysql,sqlite"
    ];
}
...
```

这样，如果你试图用`env:set`命令给`DB_CONNECTION`变量设置一个无效值，控制台会打印出一个错误

```
$ php artisan env:set DB_CONNECTION SomeInvalidValue
The selected DB_CONNECTION is invalid.
```

如果您的环境文件被修改，您可以运行`env:check`命令，该命令将检查所有变量的有效性并打印出结果。

```
$ php artisan env:check
The selected DB_CONNECTION is invalid.
```

# 创建新的环境验证规则

# 运行`make:envrule`命令

默认情况下，脚本将在`App/Env`名称空间中生成一个类。

## 示例:

```
php artisan make:envrule DatabaseEnvRules
```

`app/Env/DatabaseEnvRules.php`

```
<?phpnamespace App\Env;use Lionix\EnvValidator;class DatabaseEnvRules extends EnvValidator
{
    /**
     * Validation rules that apply to the .env variables.
     *
     * @return array
     */
    public function rules() : array
    {
        return [
            //
        ];
    }
}
```

# 指定验证规则:

```
...
public function rules() : array
{
    return [
        "DB_CONNECTION" => "requried|in:mysql,sqlite,pgsql,sqlsrv"
        "DB_HOST" => "requried",
        "DB_PORT" => "requried|numeric",
        "DB_DATABASE" => "requried",
        "DB_USERNAME" => "requried",
        "DB_PASSWORD" => "requried"
    ];
}
...
```

# 应用规则:

您可以通过`rules`键将`DatabaseEnvRules`类添加到`env.php`配置文件中。这样，该类中指定的所有规则都将影响 package artisan 命令。

`config/env.php`

```
<?phpreturn [/**
     * Validation classes which contain environment rules
     * applied by env artisan commands.
     * 
     * Add your validation classes created by 
     * `php artisan make:envrule` command to apply their rules
     * 
     * @var array
     */
    "rules" => [
        \App\Env\BaseEnvValidationRules::class
        \App\Env\DatabaseEnvRules::class // <- our database rules
    ]
];
```

或者您可以使用`Lionix\EnvClient` Facade 按照给定的验证规则来验证输入:

```
...
$client = new \Lionix\Envclient();
$client
    ->useValidator(new \App\Env\DatabaseEnvRules())
    ->update($databaseCredintnails);if ($client->errors()->isNotEmpty()) {
    // handle errors
} else {
    // success, the variables are updated
}
...
```

# 外观

# Lionix\EnvClient

## 属性:

*   受保护的$ getter:*Lionix \ env client \ Interfaces \ env getter interface*
*   受保护的$ setter:*Lionix \ env client \ Interfaces \ env setter interface*
*   受保护的$ validator:*Lionix \ env client \ Interfaces \ EnvValidatorInterface*

## 方法:

*   `void` : __construct()
    使用默认依赖项创建 EnvClient 的新实例
*   `self`:use Getter(*Lionix \ env client \ Interfaces \ env getter interface*$ getter)
    设置客户端 getter 依赖关系
*   `self`:use Setter(*Lionix \ env client \ Interfaces \ env setter interface*$ setter)
    设置 setter 依赖关系
*   `self`:use validator(*Lionix \ env client \ Interfaces \ EnvValidatorInterface*$ validator)
    设置验证器依赖关系，将当前错误与验证器错误合并
*   `array` : all()
    从环境文件中获取所有 env 变量
*   `bool`:has(*string $ key*)
    检查环境文件中是否包含该密钥
*   `mixed`:Get(*string $ key*)
    使用键获取 env 变量(返回`Illuminate\Support\Env` get 方法的输出)
*   `self`:Set(*array $ values*)
    如果验证规则通过，则在运行时设置环境变量
*   `self` : save()
    将之前设置的变量保存到环境文件中
*   `self` : update()
    如果验证规则通过，则设置变量并保存到环境文件中
*   `bool` : validate( *数组* $values)
    检查值的有效性并检索通过状态
*   `Illuminate\Support\MessageBag` : errors()
    获取类生存期内发生的所有验证错误

# Lionix\EnvGetter

## 方法:

*   `void` : __construct()
    创建一个新的 EnvGetter 实例
*   `mixed`:Get(*string*$ key)
    使用键获取 env 变量(返回`Illuminate\Support\Env` get 方法的输出)
*   从环境文件中获取所有的环境变量
*   `bool`:has(*string*$ key)
    检查环境文件中是否包含该密钥

# Lionix\EnvSetter

## 属性:

*   受保护的$variablesToSet : *数组*

## 方法:

*   `void` : __construct()
    创建一个新的 EnvSetter 实例
*   `void`:set(*array*$ values)
    用 variablesToSet 属性合并给定值
*   `void` : save()
    将之前通过 set 方法设置的所有变量保存到环境文件中
*   受保护的`string` : sanitize( *字符串* $value)
    Sanitize 输入值

# Lionix\EnvValidator

## 属性:

*   受保护的$错误:*照亮\支持\信息包*

## 方法:

*   `void` : __construct()
    创建 EnvValidator 的新实例
*   `array` : rules()
    返回类验证规则
*   `bool`:验证(*数组*$值)
    验证给定值
*   `Illuminate\Support\MessageBag` : errors()
    获取验证器错误
*   `void`:Merge errors(*Illuminate \ Support \ MessageBag*$ errors)
    将给定的 MessageBag 与当前错误合并

# 学分:

*   [斯塔斯·瓦尔坦扬](https://github.com/vaawebdev)
*   [Lionix 团队](https://lionix-team.com/)