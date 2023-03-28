# 控制反转和依赖注入

> 原文：<https://medium.com/geekculture/inversion-of-control-and-dependency-injection-58240337ef1a?source=collection_archive---------15----------------------->

在本文中，我们将学习 spring 中的控制反转和依赖注入

![](img/89e016ce08477bde9553b076eaf4f2a7.png)

# 弹簧框架

Spring 是一个有很多特性的小框架。它有时被称为框架的框架，因为它支持各种框架，包括 Struts、Hibernate、Tapestry、EJB、JSF 等等。IOC、AOP、DAO、Context、ORM、WEB MVC 和其他模块都是 Spring 框架的一部分。

**弹簧框架的优势**

*   **易于测试**:依赖注入使得测试程序变得更加容易。尽管 EJB 或 Struts 应用程序需要服务器来运行，但是 Spring 框架不需要。
*   **松耦合:**使用依赖注入更容易测试应用程序。Spring framework 不需要像 EJB 或 Struts 这样的服务器。
*   预定义模板:Spring 框架包含了 JDBC、Hibernate 和 JPA 等技术的模板。因此，没有必要编写大量代码。它隐藏了这些技术中的基本步骤。
*   **快速开发:**Spring 框架的依赖注入特性及其对各种框架的支持，使得 JavaEE 应用的开发变得容易。

## **控制反转**

这是一个将事物的控制权交给容器或框架的概念。它最常用于面向对象编程。在传统编程中，我们的自定义代码调用一个库；然而，IoC 允许一个框架控制一个程序的流程并调用我们的代码。负责对象生成的人在内存中跟踪对象。在运行时，IOC 构建类的对象，并使它在任何需要的地方可用。它主要负责确保维护对象的生命周期。

**Bean:**Bean 是 Spring 中的对象，它们构成了应用程序的主干，由 Spring IoC 容器维护。bean 是 Spring IoC 容器实例化、组装和管理的对象。另一方面，bean 只是你的程序中许多对象中的一个。Beans 及其相互依赖关系反映在容器的配置元数据中。

春豆的范围分为五大类，其中两大类如下:

**Singleton:** 每个容器只有一个 bean 实例。Spring beans 将此作为它们的默认作用域。如果使用这个范围，请确保 bean 没有任何共享的实例变量，因为这可能会导致数据不一致。

**原型:**每次请求 bean 时，都会创建一个新的实例。

## **依赖注入**

依赖注入是实现 IoC 的一种模式。它允许在类之外创建依赖对象，并以各种方式将这些对象提供给类。我们使用 DI 将依赖对象的创建和绑定移到它们所依赖的类之外。

**依赖注入的类型**

1.  构造函数注入
2.  定型剂注射
3.  现场注射

**构造函数注入:**

```
package com.app;import org.springframework.stereotype.Component;[@Component](http://twitter.com/Component)
public class FortuneService{ public String getFortune() {
  return "Today is your lucky day!";
 }
}
```

这是一个有 getFortune()方法的类，我们想在另一个类中使用这个方法。

```
package com.app;import org.springframework.stereotype.Component;
import org.springframework.beans.factory.annotation.Autowired;[@Component](http://twitter.com/Component)
public class Coach {private FortuneService fortuneService;

 [@Autowired](http://twitter.com/Autowired)
 public Coach(FortuneService fortuneService) {
  this.fortuneService = fortuneService;
 } public String getDailyWorkout() {
  return "Do cardio daily!!";
 } public String getDailyFortune() {
  return fortuneService.getFortune();
 }
}
```

配置文件是

```
<beans  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"><context:component-scan base-package="com.app"/></beans>
```

**二传手注射:**

```
package com.app;import org.springframework.stereotype.Component;[@Component](http://twitter.com/Component)
public class FortuneService{public String getFortune() {
  return "Today is your lucky day!";
 }
}
```

这是一个有 getFortune()方法的类，我们想在另一个类中使用这个方法。

```
package com.app;import org.springframework.stereotype.Component;
import org.springframework.beans.factory.annotation.Autowired;[@Component](http://twitter.com/Component)
public class Coach {private FortuneService fortuneService;

[@Autowired](http://twitter.com/Autowired)
 public void setFortuneService(FortuneService fortuneService) {
  this.fortuneService = fortuneService;
 }public String getDailyWorkout() {
  return "Do cardio daily!!";
 }public String getDailyFortune() {
  return fortuneService.getFortune();
 }
}
```

配置文件是

```
<beans  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"><context:component-scan base-package="com.app"/></beans>
```

**现场注射:**

```
package com.app;import org.springframework.stereotype.Component;[@Component](http://twitter.com/Component)
public class FortuneService{public String getFortune() {
  return "Today is your lucky day!";
 }
}
```

这是一个有 getFortune()方法的类，我们想在另一个类中使用这个方法。

```
package com.app;import org.springframework.stereotype.Component;
import org.springframework.beans.factory.annotation.Autowired;[@Component](http://twitter.com/Component)
public class Coach {[@Autowired](http://twitter.com/Autowired)
private FortuneService fortuneService;public String getDailyWorkout() {
  return "Do cardio daily!!";
 }public String getDailyFortune() {
  return fortuneService.getFortune();
 }
}
```

配置文件是

```
<beans  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"><context:component-scan base-package="com.app"/></beans>
```