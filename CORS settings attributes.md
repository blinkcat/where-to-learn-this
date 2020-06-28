# CORS settings attributes

> 在 HTML5 中，一些 HTML 元素提供了对 CORS 的支持， 例如 \<audio\>、\<img\>、\<link\>、\<script\> 和 \<video\> 均有一个跨域属性 (crossOrigin property)，它允许你配置元素获取数据的 CORS 请求。

> 这些属性是枚举的，并具有以下可能的值：

| 关键字          | 描述                                                                            |
| --------------- | ------------------------------------------------------------------------------- |
| anonymous       | 对此元素的 CORS 请求将不设置凭据标志。                                          |
| use-credentials | 对此元素的 CORS 请求将设置凭证标志；这意味着请求将提供凭据。                    |
| ""              | 设置一个空的值，如 crossorigin 或 crossorigin=""，和设置 anonymous 的效果一样。 |

> 默认情况下（即未指定 crossOrigin 属性时），CORS 根本不会使用。如 Terminology section of the CORS specification 中的描述，在非同源情况下，设置 "anonymous" 关键字将不会通过 cookies，客户端 SSL 证书或 HTTP 认证交换用户凭据。

> 即使是无效的关键字和空字符串也会被当作 anonymous 关键字使用。

### 示例：使用 crossorigin 的 script 元素

> 你可以使用下面的 <script\> 元素告诉浏览器执行来自 https://example.com/example-framework.js 的脚本且不发送用户凭据。

```html
<script
  src="https://example.com/example-framework.js"
  crossorigin="anonymous"
></script>
```

### 示例：Webmanifest with credentials

> 在获取需要用户凭据的 manifest 时，属性值必须设置为 use-credentials。即使是同源的情况。

```html
<link rel="manifest" href="/app.webmanifest" crossorigin="use-credentials" />
```

## 启用了 CORS 的图片

> HTML 规范中图片有一个 crossorigin 属性，结合合适的 CORS 响应头，就可以实现在画布中使用跨域 <img\> 元素的图像，就像在原生 <canvas\> 中使用一样。

### 安全性和“被污染”(tainted)的 canvas

> 尽管不通过 CORS 就可以在 <canvas\> 中使用其他来源的图片，但是这会污染画布，并且不再认为是安全的画布，这将可能在 <canvas\> 检索数据过程中引发异常。

> 如果从外部引入的 HTML <img\> 或 SVG <svg\> ，并且图像源不符合规则，将会被阻止从 <canvas\> 中读取数据。

> 在"被污染"的画布中调用以下方法将会抛出安全错误：

> - 在 <canvas\> 的上下文上调用 getImageData()
> - 在 <canvas\> 上调用 toBlob()
> - 在 <canvas\> 上调用 toDataURL()

> 这种机制可以避免未经许可拉取远程网站信息而导致的用户隐私泄露。

这时候需要在两端同时设置

> 首先，你必须有一个可以对图片响应正确 Access-Control-Allow-Origin 响应头的服务器。

> 在 HTMLImageElement 上设置 crossOrigin 的 crossorigin 属性，这将允许浏览器在下载图像数据时允许跨域访问请求。

## 参考

1. [CORS settings attributes](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_settings_attributes)
2. [启用了 CORS 的图片](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image)
3. [Why can't I force download of tainted canvas and why is it a security issue?](https://stackoverflow.com/questions/36888453/why-cant-i-force-download-of-tainted-canvas-and-why-is-it-a-security-issue#:~:text=The%20browser%20can't%20read,That's%20an%20indirect%20security%20violation.)
4. [Why is a “tainted canvas” a risk?](https://security.stackexchange.com/questions/179432/why-is-a-tainted-canvas-a-risk)
