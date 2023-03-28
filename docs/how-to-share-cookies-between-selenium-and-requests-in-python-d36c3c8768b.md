# 如何在 Python 中在 Selenium 和请求之间共享 cookies

> 原文：<https://medium.com/geekculture/how-to-share-cookies-between-selenium-and-requests-in-python-d36c3c8768b?source=collection_archive---------3----------------------->

想象你正试图在某种安全机制背后的安全位置检索或创建内容。大多数情况下，安全机制是某种登录门户，通过发送 POST 请求的凭证并创建随后续请求一起发送的会话，可以很容易地“绕过”它。如果您被重定向到中央登录服务器或最坏的情况 SSO(单点登录)身份验证机制，会发生什么？在某些情况下，取决于…