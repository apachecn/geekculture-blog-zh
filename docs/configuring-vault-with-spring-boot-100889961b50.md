# 使用 Spring Boot 配置保管库

> 原文：<https://medium.com/geekculture/configuring-vault-with-spring-boot-100889961b50?source=collection_archive---------1----------------------->

根据 Spring Cloud Vault 和 Spring Boot 的版本，保管库属性的配置会有所不同。

在 Spring Cloud Vault 3.0 和 Spring Boot 2.4 中，属性源的引导上下文初始化(`bootstrap.yml`、`bootstrap.properties`)不再使用**。
最新版本的新增强可以参考[https://docs . spring . io/spring-cloud-vault/docs/current/reference/html/# new-in-3 . 0 . 0](https://docs.spring.io/spring-cloud-vault/docs/current/reference/html/#new-in-3.0.0)。**

## 相关性设置

首先，我们将把 spring cloud vault 配置依赖项添加到我们的`pom.xml`中

```
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-vault-config</artifactId>
     <version>{project-version}</version>
</dependency>
```

现在，我们将了解如何为不同版本配置 vault 属性。

## 支持引导上下文的配置

在早期版本中，spring cloud vault 在引导上下文中运行，最初获取配置属性，因此它可以将这些属性提供给自动配置和我们的应用程序本身。

我们可以用`bootstrap.yml`或`bootstrap.properties`来配置我们的应用

```
spring:
  cloud:
    vault:
      enabled: true
      kv:
        backend: <secret>
        enabled: true
        application-name: <vault-application-name>
      authentication: APPROLE
      app-role:
        role-id: <role-id>
        secret-id: <secret-id>
        app-auth-path: approle
      scheme: https
      uri: <vault-server>
      connection-timeout: 5000
      read-timeout: 15000
```

*   `scheme`将方案设置为`http`将使用普通 HTTP。支持的方案有`http`和`https`。
*   `uri`用 URI 配置 Vault 端点。优先于主机/端口/方案配置
*   `connection-timeout`以毫秒为单位设置连接超时
*   `read-timeout`以毫秒为单位设置读取超时
*   `authentication`设置认证机制以授权客户端请求。Spring Cloud Vault 支持多种身份验证机制，通过 Vault 对应用程序进行身份验证。请参考[https://docs . spring . io/spring-cloud-vault/docs/current/reference/html/# authentic ation](https://docs.spring.io/spring-cloud-vault/docs/current/reference/html/#authentication)
*   `kv`设置键值配置

## 不支持引导上下文的配置

这可以通过两种方式实现:

**1。**使用 Spring Boot 2.4 配置数据 API(首选)

Spring Cloud Vault 的新版本支持 Spring Boot 的配置数据 API，它允许从 Vault 导入配置。

将所有属性从`bootstarp.yml`文件移动到`application.yml`文件。`aaplication.yml`文件会是什么样子

```
spring:
  cloud:
    vault:
      authentication: APPROLE
      app-role:
        role-id: <role-id>
        secret-id: <secret-id>
        app-auth-path: approle
      uri: <vault-server>
      connection-timeout: 5000
      read-timeout: 15000
  config:
    import: vault://<secret>/<vault-application-name>
```

`spring.config.import`设置 vault 键值后端的挂载路径。

这个属性文件也可以以下面的格式提供

```
spring:
  cloud:
    vault:
      enabled: true
      kv:
        backend: <secret>
        enabled: true
        application-name: <vault-application-name>
      authentication: APPROLE
      app-role:
        role-id: <role-id>
        secret-id: <secret-id>
        app-auth-path: approle
      scheme: https
      uri: <vault-server>
      connection-timeout: 5000
      read-timeout: 15000
  config:
    import: optional:vault://
```

`spring.cloud.vault.enabled`用于启用/禁用保险库。当 vault 被禁用时，在应用程序启动期间将跳过作为`optional`提供的配置位置。

**2。**如果我们仍然想使用引导上下文，我们可以通过
包括下面对`pom.xml`的依赖来启用它

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

并将该配置属性`spring.cloud.bootstrap.enabled=true`添加到`application.yml`文件中。

# 参考

[1][https://docs . spring . io/spring-cloud-vault/docs/current/reference/html/# client-side-usage](https://docs.spring.io/spring-cloud-vault/docs/current/reference/html/#client-side-usage)

[2][https://cloud . spring . io/spring-cloud-vault/reference/html/# _ client _ side _ usage](https://cloud.spring.io/spring-cloud-vault/reference/html/#_client_side_usage)