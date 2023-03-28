# 使用 JavaScript 显示 ActiveRecord 验证错误

> 原文：<https://medium.com/geekculture/using-javascript-to-display-activerecord-validation-errors-ecce915212c0?source=collection_archive---------11----------------------->

这篇博客探讨了 ActiveRecord 提供的几种服务器端验证方法，以及如何在 JavaScript 的帮助下以用户友好的方式显示它们。

# 什么是验证？

在编程环境中，验证是指检查和/或证明数据输入的有效性、相关性和准确性的过程。它的范围从检查是否填写了必需的年龄字段到检查信用卡号是否有效。

# 为什么验证很重要？

验证是非常重要的，因为它是程序员或软件开发人员确保输入的数据准确且有用的一种方式。它避免了程序员错误地授权未经验证的用户，同时还能够以所需的方式组织用户输入，而不必遍历数据库中的所有数据。

此外，验证也有助于避免边缘情况，并确保程序在其设计的情况下运行。通过数据验证，程序员可以不必为所有可能的用户输入编写条件。相反，简单的数据验证将确保用户只能输入满足某些预定义要求的数据。

这就是说，尽管数据验证看起来像是一个额外的高级功能，可能会减慢您的开发过程，但它非常重要，因为它可以帮助您创建健壮的结果。

# 验证的类型

根据它们发生的阶段，有两种主要的验证类型——客户端验证和服务器端验证。

*   **客户端验证**

客户端验证是指确保表单输入的格式与填写时的格式一致的过程。它确保用户输入的数据在被发送到服务器进行处理之前符合程序员设定的要求。

这种类型的验证对于良好的用户体验非常重要，因为它通知并允许用户在发送输入并被服务器拒绝之前修复输入是否无效。因此，它避免了数据从客户端到服务器来回传输所造成的延迟。不仅如此，它还减少了计算量，从而节省了更多的资源，这对开发人员和客户都有好处。

客户端验证的一个示例如下:

```
<form>
<input type="text" name="first_name" required>
</form<!-- The "required" keyword in the HTML above makes sure that the user does not submit the form without providing a first name. If the user forgets to do so, the form will not go ahead and send the data to the server. Rather, it does not perform any action until the user fills out the required input field -->
```

*   **服务器端验证**

在服务器端验证中，用户的输入由存储在服务器上的代码进行验证。服务器中的后端代码向客户端发送回一个关于输入验证的响应，不管它是否有效。由于客户端无法直接访问服务器端的后端源代码(与前端 HTML 不同)，因此在服务器端进行验证可以更好地防范任何恶意尝试。在服务器端修改代码比用浏览器的开发工具修改页面的 HTML 要困难得多。

# **为什么最好同时使用服务器端验证和客户端验证**

尽管用户友好，但是客户端验证是脆弱和不可靠的。可以通过更改给定网页的前端代码来绕过它们。但是，如果将它们与服务器端验证结合起来，它们的漏洞将不会成为大问题，因为服务器端验证将能够处理任何可能绕过客户端验证的恶意尝试。

因此，结合使用这两种验证技术有助于开发人员建立健壮的、用户友好的、响应迅速的和详尽的验证。

# **ActiveRecord**

按照其官网上的定义，ActiveRecord 是“系统中负责表现业务数据和逻辑的层”。它“方便了数据需要持久存储到数据库的业务对象的创建和使用。”更简单地说，它指的是用于管理数据库资源的对象关系映射系统(Ruby 编程语言及其 Rails 开发框架)。

除了其他几个函数之外，ActiveRecord 还为我们提供了几个内置的验证函数，其中一些如下所示:

```
acceptance
validates_associated
confirmation
exclusion
format
inclusion
length
numericality
presence
absence
uniqueness
validates_with
validates_each
```

# 使用活动记录验证

ActiveRecord 验证通常应用于模型中，我们希望在将数据存储到数据库之前验证这些模型的数据。它们通常遵循以下通用格式:

```
class **className** **<** ApplicationRecord
  validates :attribute_to_be_validated, validation_method: **parameter**
end
```

以下是使用内置助手 ActiveRecord 验证函数的不同验证示例:

```
class **Person** **<** ApplicationRecord
  validates :name, presence: **true**
end#=> Return this if the name attribute is blank: ActiveRecord::RecordInvalid: Validation failed: Name can't be blankclass **Coffee** **<** ApplicationRecord
  validates :size, inclusion: { in: %w(small medium large),
    message: "%{value} is not a valid size" }
end#=> We can customize the error message returned by failed validations as we wantclass **Person** **<** ApplicationRecord
  validates :name, length: { minimum: 2 }
  validates :bio, length: { maximum: 500 }
  validates :password, length: { in: 6**..**20 }
  validates :registration_number, length: { is: 6 }
end#=> Validating the character lengths of different attributesclass **Player** **<** ApplicationRecord
  validates :points, numericality: **true**
  validates :games_played, numericality: { only_integer: **true** }
end#=> Validating the data types of different attributes
```

> [https://guides . ruby on rails . org/active _ record _ validations . html](https://guides.rubyonrails.org/active_record_validations.html)

# **使用 JavaScript 显示验证错误**

当显示服务器端验证返回的错误消息时，JavaScript 就进入了验证的范畴。最简单、最方便的方法是检查某个类函数在控制器中被调用时是否返回错误，如果是，则将错误呈现为 JSON。然后，我们可以从前端监听这个响应，并将错误显示为警报或不同的文本形式。

下面是如何实现这一点的简单演示:

```
In the model...
class **Person** **<** ApplicationRecord
  validates :name, presence: **true**
endIn the controller...
def create
    person = Person.create(person_params)
    if person
        render json: person
    else 
        render person.errors.full_messages
    end
endIn the front-end...
fetch(CORRESPONDING_LINK)
.then(r => r.json())
.then()
.catch((error) => {
        alert(error);
    });
```

总而言之，数据验证是网站开发中的一个重要步骤，它可以从客户端和服务器端完成。HTML、JavaScript 和 ActiveRecord 可以协同使用，为不同的用户输入创建一个健壮的验证系统。

# 参考作品

[](https://guides.rubyonrails.org/active_record_validations.html) [## 活动记录验证——Ruby on Rails 指南

### 活动记录验证本指南教你如何在对象进入数据库之前验证它们的状态…

guides.rubyonrails.org](https://guides.rubyonrails.org/active_record_validations.html) [](https://www.tjvantoll.com/2015/09/13/fetch-and-errors/) [## 用 fetch()处理失败的 HTTP 响应

### 测验:这个对 web 的新 fetch() API 的调用是做什么的？fetch("http://httpstat.us/500 ")。然后(function() {…

www.tjvantoll.com](https://www.tjvantoll.com/2015/09/13/fetch-and-errors/) [](http://net-informations.com/faq/asp/validation.htm#:~:text=In%20the%20Server%20Side%20Validation,new%20dynamically%20generated%20web%20page) [## 区分客户端验证和服务器端验证

### 客户端验证与服务器端验证有一个常见的问题，即哪种类型的验证更好或…

net-informations.com](http://net-informations.com/faq/asp/validation.htm#:~:text=In%20the%20Server%20Side%20Validation,new%20dynamically%20generated%20web%20page)  [## 什么是数据验证？它的工作原理及其重要性

### 数据验证是任何数据处理任务的重要组成部分，无论您是在现场收集信息…

www.safe.com](https://www.safe.com/what-is/data-validation/) [](https://guides.rubyonrails.org/active_record_basics.html) [## 活动记录基础- Ruby on Rails 指南

### 活动记录基础本指南介绍了活动记录。看完这本指南，你会知道:什么对象…

guides.rubyonrails.org](https://guides.rubyonrails.org/active_record_basics.html)