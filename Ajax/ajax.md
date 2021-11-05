# **跨域关于Access-Control-Allow-Origin问题**

### **什么是Access-Control-Allow-Origin？**

[Access-Control-Allow-Origin - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) 说明了 The **Access-Control-Allow-Origin** response header indicates whether **the response** can be shared with requesting code from the given **origin**. ( Access-Control-Allow-Origin 就是响应报头指示直接控制响应是否可以有自给定的origin请求 )

### **什么是orgin？**

[Access-Control-Allow-Origin - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) 说明了 **Web** **request** content's **origin** is defined by the scheme (protocol), hostname (domain), and port of the URL used to access it.<u> Two objects have the same origin only when the scheme, hostname, and port all match.</u>

Some operations are restricted to same-origin content, and this restriction **can be lifted using CORS**. (origin**是web请求**带上的一个属性，用一些操作是可以**解除CORS的限制**)

### **解决跨域问题**

1. 前端request的时候可以查看一下自己的origin是什么，把origin用于给后端写入值。

2. 既然Access-Control-Allow-Origin是在response的时候响应的，那在后端返回http请求时加上header中关于Access-Control-Allow-Origin的值。当然Access-Control-Allow-Origin直接写*也是可以的

### **解决跨域的原理**

前端浏览器会在请求分析当前页面域名与请求的域名是否相同，如果不相同必是跨域请求。此时此刻浏览器把请求分为2次，第一次是叫做OPTIONS请求(具体叫做**预检请求** **preflight request**)，然后端返回Access-Control-Allow-Origin判断是否与origin一致，如果一致可以进行下一步的GET或者POST的请求，如果不一致就会跨域的错误。

### **写了Access-Control-Allow-Origin与orgin相同的值还是出现跨域问题？**

如果Access-Control-Allow-Origin与orgin相同的值还是出现跨域问题，正确来说不是跨域，是在预检请求的时候出现了错误，此时此刻可以看看**Access-Control-Allow-Headers**中是不是有一些**自定义的请求头**（此处自定义的请求头是指在Accept、Accept-Language、Content-Language、Content-Tye这4个大小写和符号都要一样除外的请求头，具体参考[CORS-safelisted request header - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/CORS-safelisted_request_header#additional_restrictions)），当有了自定义的Access-Control-Allow-Headers时Access-Control-Allow-Origin是不允许直接使用"星号"，需要具体指明一个origin
