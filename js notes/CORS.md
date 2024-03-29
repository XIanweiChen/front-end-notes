跨域资源共享([CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS))

### 功能概述

跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源.对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 以外的 HTTP 请求)浏览器必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS) 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。

### 简单请求

某些请求不会触发 [CORS 预检请求](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS#Preflighted_requests)。本文称这样的请求为“简单请求”，请注意，该术语并不属于 [Fetch](https://fetch.spec.whatwg.org/) （其中定义了 CORS）规范。若请求满足所有下述条件，则该请求可视为“简单请求”：

- 使用下列方法之一：

  - [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET)
  - [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD)
  - [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST)

- Fetch 规范定义了

  对 CORS 安全的首部字段集合，不得人为设置该集合之外的其他首部字段。该集合为：

  - [`Accept`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept)
- [`Accept-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language)
  - [`Content-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)
  - [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) （需要注意额外的限制）
  - `DPR`
  - `Downlink`
  - `Save-Data`
  - `Viewport-Width`
  - `Width`
  
- `Content-Type`

   

  的值仅限于下列三者之一：

  - `text/plain`
  - `multipart/form-data`
  - `application/x-www-form-urlencoded`

- 请求中的任意[`XMLHttpRequestUpload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestUpload) 对象均没有注册任何事件监听器；[`XMLHttpRequestUpload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestUpload) 对象可以使用 [`XMLHttpRequest.upload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/upload) 属性访问。

- 请求中没有使用 [`ReadableStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/ReadableStream) 对象。



### 预检请求

与前述简单请求不同，“需预检的请求”要求必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)   方法发起一个预检请求到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

当请求满足下述任一条件时，即应首先发送预检请求：

- 使用了下面任一 HTTP 方法：

  - [`PUT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/PUT)
  - [`DELETE`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/DELETE)
  - [`CONNECT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/CONNECT)
  - [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)
  - [`TRACE`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/TRACE)
  - [`PATCH`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/PATCH)

- 人为设置了

  对 CORS 安全的首部字段集合

  之外的其他首部字段。该集合为：

  - [`Accept`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept)
  - [`Accept-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language)
  - [`Content-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)
  - [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) (需要注意额外的限制)
  - `DPR`
  - `Downlink`
  - `Save-Data`
  - `Viewport-Width`
  - `Width`

- `Content-Type`的值不属于下列之一:

  - `application/x-www-form-urlencoded`
- `multipart/form-data`
  - `text/plain`

- 请求中的[`XMLHttpRequestUpload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestUpload) 对象注册了任意多个事件监听器。

- 请求中使用了[`ReadableStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/ReadableStream)对象。

