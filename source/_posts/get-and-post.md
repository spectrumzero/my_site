---
title: GET 和 POST 方法的区别
date: 2025-11-14 21:14:26
tags:
  - [System Design]
---

GET 和 POST 是 HTTP 协议中两种常用的请求方法，它们在不同的场景和目的下有不同的特点和用法。一般来说，可以从以下几个方面来区分它们：

- 语义上的区别：GET 通常用于获取或查询资源，而 POST 通常用于创建或修改资源。GET 请求应该是幂等的，即多次重复执行不会改变资源的状态，而 POST 请求则可能有副作用，即每次执行可能会产生不同的结果或影响资源的状态。
- 格式上的区别：GET 请求的参数通常放在 URL 中，形成查询字符串（querystring），而 POST 请求的参数通常放在请求体（body）中，可以有多种编码格式，如 application/x-www-form-urlencoded、multipart/form-data、application/json 等。GET 请求的 URL 长度受到浏览器和服务器的限制，而 POST 请求的 body 大小则没有明确的限制。
- 缓存上的区别：由于 GET 请求是幂等的，它可以被浏览器或其他中间节点（如代理、网关）缓存起来，以提高性能和效率。而 POST 请求则不适合被缓存，因为它可能有副作用，每次执行可能需要实时的响应。
- 安全性上的区别：GET 请求和 POST 请求都不是绝对安全的，因为 HTTP 协议本身是明文传输的，无论是 URL、header 还是 body 都可能被窃取或篡改。为了保证安全性，必须使用 HTTPS 协议来加密传输数据。不过，在一些场景下，GET 请求相比 POST 请求更容易泄露敏感数据，因为 GET 请求的参数会出现在 URL 中，而 URL 可能会被记录在浏览器历史、服务器日志、代理日志等地方。因此，一般情况下，私密数据传输应该使用 POST + body。

上述内容参考了：[Java Guide](https://javaguide.cn/)。
