# 保护 WordPress 自定义表单免受 CSRF 攻击

> 原文：<https://medium.com/geekculture/protecting-wordpress-custom-forms-from-csrf-attack-a5528b91d0df?source=collection_archive---------5----------------------->

![](img/3acae56ddaa510f3847c4542ab8b5bdc.png)

你可能听说过 WordPress 是迄今为止被黑客攻击最多的 CMS 平台。尽管如此，世界上许多开发者或博客仍然在使用 WordPress 建立网站。这就是为什么我们应该尽可能在 WordPress 网站上应用安全措施。

CSRF 攻击已经存在了相当长的时间。如果您的站点没有受到保护，攻击者很容易劫持您的会话并在未经您同意的情况下对您的站点执行状态更改。我建议阅读[在 OWASP](https://owasp.org/www-community/attacks/csrf) 上的这篇文章，对 CSRF 袭击事件有一个全面的了解。

# 1.策划针对 WordPress 的 CSRF 攻击

有时当表单插件不能满足你的定制需求时，你将不得不创建你自己的定制表单，例如通过插入一个定制的短代码。这个表单可能是一个登录表单、一个联系表单，甚至是更复杂的表单，允许用户在您的站点上输入帐户详细信息或进行订购。在下面的示例中，我将创建一个没有任何 CSRF 保护的简单的更改电子邮件表单，攻击者很容易利用该表单在第三方恶意站点上触发 POST 请求，将您的电子邮件更改为攻击者的电子邮件。

假设我们将`test-form`实现为一个短代码，显示在您站点的某个地方，如下所示:

```
function test_form() {
    ob_start();
    ?>
        <h2>Change Email Form</h2>
        <form method="POST" id="test-form">
            <div class="input-group">
                <label for="email" class="form-label">Your new email <span style="color: red">*</span></label>
                <input type="email" name="new_email" id="email">
            </div>
            <button type="submit">Submit</button>
        </form>
    <?php
    return ob_get_clean();
}function test_form_shortcode() {
    add_shortcode( 'test-form', __NAMESPACE__ . '\\test_form' );
}
add_action('init', __NAMESPACE__ . '\\test_form_shortcode');
```

在此之下，您还添加了代码来处理所述`test-form`的提交:

```
function process_test_form() {
    if (isset($_POST['new_email']) && is_user_logged_in()) {
        // execute code to change user email to new email
        ... wp_redirect( get_home_url() . '/change-email-success/' );
        exit;
    }
}
add_action( 'wp_loaded', __NAMESPACE__ . '\\process_test_form' );
```

假设您站点的用户 Tom 已经登录到您的站点。他偶然发现一个被利用的页面，该页面看起来绝对正常，但实际上隐藏着一个指向攻击者恶意页面的 iframe，该页面在页面加载时向您的站点提交 POST 请求:

```
<body onload='document.CSRF.submit()'>
    ...
    <form action="[http://yoursite.com/change-email/](http://tt-tutort.local/contact-us/)" method="POST" name="CSRF">
        <input type="hidden" name="new_email" id="email" value="[hacker@example.com](mailto:hacker@gmail.com)">
    </form>
    ....
</body>
```

Tom 不会注意到任何事情，因为 iframe 是隐藏的，而且在那个时候，Tom 在你的 WordPress 站点上的电子邮件地址已经被改变了。现在，攻击者可能能够在您的网站上沿着忘记密码的路线，劫持 Tom 的帐户。稍后，Tom 会发现他再也无法登录自己的帐户。

# 2.修补 CSRF 漏洞

幸运的是，这实际上很容易解决。一个简单的方法是在表单中包含一个 WordPress 随机数，然后在每次处理 POST 请求时验证这个随机数。

在表单顶部添加这一行:

```
<form id="test-form" method="POST">
    <input name="form_nonce" type="hidden" value="<?=wp_create_nonce('test-nonce')?>" />
    ....
```

然后，当您处理表单时，验证 nonce 以及您的原始检查:

```
if (isset($_POST['form_nonce']) && wp_verify_nonce($_POST['form_nonce'],'test-nonce') && isset($_POST['new_email']) && is_user_logged_in()) {
```

在那之后，你已经安全了，不会受到 CSRF 的大部分攻击。从现在开始，要完成 POST 请求，攻击者必须知道 nonce 的正确值，这比完全没有保护要安全得多。然而，这并不能防止重放攻击，因为 WordPress 生成的随机数不会只使用一次。它们也不是只使用一次，而是有一个有限的“寿命”，在此之后它们就会过期。在此期间，将为给定上下文中的给定用户生成相同的随机数。”—[https://codex.wordpress.org/WordPress_Nonces](https://codex.wordpress.org/WordPress_Nonces)

如果你想让你的表单有一个额外的安全层，我建议在你的表单中添加 Google reCaptcha。它不仅保护您的表单免受 CSRF 攻击，还保护您免受垃圾邮件机器人的攻击。