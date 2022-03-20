# 跨域及其解决方法



## 跨域

跨域指的是**浏览器**不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。



## 同源策略

同源策略是指协议、域名、端口都要相同，其中有一个不同都会产生跨域

| URL                                                          | 说明                   | 是否允许通信                           |
| ------------------------------------------------------------ | ---------------------- | -------------------------------------- |
| http://www.a.com/a.js<br />http://www.a.com/b.js             | 同一域名下             | 允许                                   |
| http://www.a.com/static/a.js<br />http://www.a.com/script/b.js | 同一域名下，不同文件夹 | 允许                                   |
| http://www.a.com/a.js<br />http://www.a.com:8080/b.js        | 同一域名下，不同端口   | 不允许                                 |
| http://www.a.com/a.js<br />https://www.a.com/b.js            | 同一域名下，不同协议   | 不允许                                 |
| http://www.a.com/a.js<br />http://192.168.0.1198/b.js        | 域名和域名对应IP       | 不允许                                 |
| http://www.a.com/a.js<br />http://script.a.com/b.js          | 主域相同，子域不同     | 不允许                                 |
| http://www.a.com/a.js<br />http://a.com/b.js                 | 同一域名，不同二级域名 | 不允许（cookie这种情况下也不允许访问） |
| http://www.a.com/a.js<br />http://www.b.com/b.js             | 不同域名               | 不允许                                 |



## 跨域流程

> https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS

![跨域流程](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202203201554411.png)



## 解决跨域

### 方法一：使用Nginx部署为同一域

![使用nginx部署为同一域](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202203201702212.png)

### 方法二：允许请求跨域

#### 添加响应头

- `Access-Control-Allow-Origin`：支持那些来源的请求跨域
- `Access-Control-Allow-Methods`：支持那些请求方法跨域
- `Access-Control-Allow-Credentials`：跨域请求默认不包含cookie，设置为true可以包含cookie
- `Access-Control-Expose-Headers`：跨域请求暴露的字段

> CORS请求时，`XMLHttpRequest`对象的`getResponseHeader()`方法只能拿到6个基本字段：
>
> - Cache-Control
> - Content-Language
> - Content-Type
> - Expires
> - Last-Modified
> - Pragma
>
> 如果想要拿到其他字段，就必须在`Access-Control-Expose-Headers`里面指定

- `Access-Control-Max-Age`：表明该响应的有效时间为多少秒。在有效时间内，浏览器无需为同一请求再次发起预检请求。请注意，浏览器自身维护了一个最大有效时间，如果该字段的值超过了最大有效时间，将不会生效