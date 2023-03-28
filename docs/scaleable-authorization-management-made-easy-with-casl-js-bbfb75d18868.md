# CASL 使可扩展的授权管理变得简单。射流研究…😀

> 原文：<https://medium.com/geekculture/scaleable-authorization-management-made-easy-with-casl-js-bbfb75d18868?source=collection_archive---------19----------------------->

![](img/6e5f3d86305446b4b082481d49877571.png)

# 介绍

[CASL。JS](https://casl.js.org/v5/en/) 的核心是一个同构授权 JavaScript 库”。🤔

> *“同构”这个有趣的词意味着你可以以完全相同的方式在前端和后端使用这个库。*——*塞尔吉·斯托茨基*
> 
> CASL(读作/ˈkæsəl/，像一座城堡)是一个同构的授权 JavaScript 库，它限制了给定客户端可以访问的资源。它被设计成可增量采用的，并且可以在简单的基于声明的和全功能的基于主题和属性的授权之间轻松扩展。它使得跨 UI 组件、API 服务和数据库查询管理和共享权限变得容易。

CASL 是多功能的，您可以从简单的基于角色的访问控制开始，并扩展您的应用程序，以包括全功能的基于属性的访问控制。

CASL 是声明性的，它允许您使用几乎逐字匹配您的业务需求的领域特定语言来定义服务器端内存中的权限。

CASL 是类型安全的，它是用类型脚本编写的，这使得应用程序更安全，开发者体验更愉快。

CASL 很小，只有 4.5KB 左右，还可以更小，多亏了摇树！最小大小约为 1.5KB。

# 背景

因此，在工作中使用 Nodejs 应用程序时；该团队花了数周时间研究和开发一个混合访问控制框架，该框架既可扩展，又可根据业务需求动态变化。

最近的发展表明，基于属性的访问控制(ABAC)可以在动态分布式系统和企业应用程序中提供灵活和细粒度的访问控制。由于只考虑主体、客体和环境的属性，典型的基于角色的访问控制(ABAC)方案的大多数当前解决方案不能为具有不同属性的一系列用户扩展对资源的许可。

同样在(ABAC)框架中，目标属性由资源本身在特定条件下获得或定义，例如时间、位置、IP 地址等

因此，在混合访问控制框架中，对资源的许可跨平台上用户的角色、声明、属性进行建模；例如由简档所有者指定的用户定义的属性，例如性别、姓名、工作、家乡、爱好等。

构建一个定制的混合访问控制框架，就像上面讨论的那样多才多艺，可能会耗费大量资源，尤其是对于小型团队🙄。

我们发现 [CASL JS](https://github.com/stalniy/casl) 授权库非常棒，易于采用，维护量大，并且能够有效地随业务扩展，以动态管理对资源的许可😃。

**假设**。我假设你已经对 [Nodejs](https://nodejs.org/en/) 和通过声明或角色管理应用程序中的权限有所了解😌。

# 开始行动——空谈是廉价的😆

1).将[@ casl](https://hashnode.com/@casl)/能力作为一个依赖项安装在你的 Nodejs 应用程序中:

`npm i @casl/ability`

2).定义能力

有三种方法可以定义能力:

```
- using defineAbility function
- using AbilityBuilder class
- using JSON objects
```

在这个例子中，我们将使用`defineAbility function`

这个函数允许使用 can 和 cannot 方法创建一个能力实例。它允许在不写太多代码的情况下定义和使用能力实例。

```
//ability/defineAbility.jsconst { AbilityBuilder, Ability } = require("[@casl/ability](http://twitter.com/casl/ability)");/**
 * [@param](http://twitter.com/param) user contains details about logged in user: its id, name, email, etc
 */
// Define abilities for subjects here// permissions on Organization
exports.defineAbilitiesOnOrganizationFor = (user) => {
  const { can, cannot, rules } = new AbilityBuilder(Ability);// condition == True
  if (user.isAdmin) {
    // can manage (i.e., do anything) own Oganization
    can(
      "manage", // can do everything
      "Oganization", //  Organization collection
      ["email",
        "phone",
        "password",
        "firstName", // feilds that can be managed
        "lastName"
      ],
      { _id: user._id } // condition , if OrgId=user.Id (belongs to an Org)
    );// But cannot delete Oganization
    cannot(
      "delete", // cannot delete Organization any Organization
      "Organization" // collection Organization
    );
  }if (user.isSuperAdmin) {
    // define the abilities for superAdmin on organization(subject)
    can(
      "manage",
      "Organization"
    );
  }return new Ability(rules);
};
```

在上面的实现中，我们为“管理员”和“超级管理员”定义了权限，我们进一步为组织设置了条件，使其成为可由两个用户管理的资源。

CASL 在能力级别上操作，这是用户在应用程序中实际可以做的事情。能力本身取决于 4 个参数(最后 3 个是可选的):

**用户动作**描述了用户在 app 中实际可以做的事情。用户动作是一个依赖于业务逻辑的词(通常是动词)(例如，延长、读取)。它通常是来自 CRUD 的单词列表——创建、读取、更新和删除。

**主题**要检查用户操作的主题或主题类型。通常，这是一个业务(或域)实体(例如，订阅、文章、用户、组织)。主题和主题类型之间的关系与对象实例和它的类之间的关系相同。

**字段**可用于将用户操作仅限于匹配主题的字段(例如，允许管理员更新资源的字段，不允许更新某些字段)

**条件**将用户操作仅限于匹配主题的标准。当您需要授予特定主题的权限时(例如，允许用户管理他们自己的简档帐户)，这很有用

有；业务需求可以很容易地转化为 CASL 规则🤩。

3).数据库集成 CASL 有一个补充包[[@ casl](https://hashnode.com/@casl)/mongose]，它提供了与 MongoDB 和[mongose]的轻松集成。

安装依赖关系:`npm install @casl/mongoose`

```
// Organization.jsimport { AbilityBuilder } from '[@casl/ability](http://twitter.com/casl/ability)';
import { accessibleRecordsPlugin } from '[@casl/mongoose](http://twitter.com/casl/mongoose)';
import mongoose from 'mongoose';
import {defineAbilitiesOnOrganizationFor} from 'ability/defineAbility.js'mongoose.plugin(accessibleRecordsPlugin);const user = getUserLoggedInUser(); // app specific functionconst ability = defineAbilitiesOnOrganizationFor(user);const Organization = mongoose.model('Organization', mongoose.Schema({
  email: String,
  isAdmin: Boolean, 
  phone: Number,
  password: String,
  content: String,
  createdAt: Date,
  firstName: String,
  lastName: String}))Organization.plugin(accessibleRecordsPlugin)module.exports = mongoose.model('Organization', Organization)
```

4).检查能力

```
// ../controllers/organizationController.jsconst { NotFound, Unauthorized, InternalServerError } = require("http-errors");
const Organizations = require("../models/organization");async function permissionChecker(model, defineAbilitiesOnSubjectFor, userId) {
  try {// get user and all attributes
    const user = await model.findOne({ _id: userId });
    if (!user) {
      throw new NotFound("user not found");
    }
    // get ability
    const ability = defineAbilitiesOnSubjectFor(user);
     return ability;} catch (error) {
    throw new Unauthorized(error.message);
  }
}let action = "update" // specify the type of action on the resource, to check for
let subject = "Organization"  // resource that needs permission and access control// check for permission using the permission checker
    const ability = await permissionChecker(
      Organization,
      defineAbilitiesOnOrganizationFor,
      user
    );if(ability) {
   try {
        const organization = new Organization();
        organization.set(data); // app specific data
        ForbiddenError.from(ability).throwUnlessCan(action, organization);
        await organization.save();
   }catch(error){
      throw new Unauthorized(error.message);
   }
} else{
   throw new InternalServerError(error.message);
}
```

是的，在你的 [Nodejs](https://nodejs.org/en/) 应用中用 CASL 实现可伸缩的内存权限管理和资源授权就是这么简单😊。

CASL 的适应性越来越强，这意味着您可以从简单的基于角色的授权开始您的项目，并在以后随着您的应用程序功能的发展而发展。

感谢观众，希望这篇文章对你有所帮助🤗。你可以随时在 Github、T2、推特和 T4 的 LinkedIn 上联系我。一定要点赞、评论和分享😌。

# 了解更多信息

*   [CASL JS 文档](https://casl.js.org/)

*最初发布于*[*https://blog . next Webb . tech*](https://blog.nextwebb.tech/scaleable-authorization-management-made-easy-with-casljs/)*。*