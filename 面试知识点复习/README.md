># css
>## 移动端适配1px的问题
- 伪元素+transform
```css
.border-1px {
    position: relative;
}
@media screen and (-webkit-min-device-pixel-ratio: 2) {
    .border-1px:before {
        content: " ";
        position:absolute;
        left: 0;
        top: 0;
        width: 100%;
        border-top: 1px solid #D9D9D9;
        color: #D9D9D9;
        -webkit-transform-origin: 0 0;
        transform-opigin: 0 0;
        -webkit-transform: scaleY(0.5);
        transform: scaleY(0.5);
    }
}
```
- 用js计算rem基准值和viewport所放置
```javascript
/* 设计稿是750,采用1：100的比例,font-size为100 * (docEl.clientWidth * dpr / 750) */
var dpr, rem, scale;
var docEl = document.documentElement;
var fontEl = document.createElement('style');
var metaEl = document.querySelector('meta[name="viewport"]');
dpr = window.devicePixelRatio || 1;
rem = 100 * (docEl.clientWidth * dpr / 750);
scale = 1 / dpr;
// 设置viewport，进行缩放，达到高清效果
metaEl.setAttribute('content', 'width=' + dpr * docEl.clientWidth + ',initial-scale=' + scale + ',maximum-scale=' + scale + ', minimum-scale=' + scale + ',user-scalable=no');
// 设置data-dpr属性，留作的css hack之用，解决图片模糊问题和1px细线问题
docEl.setAttribute('data-dpr', dpr);
// 动态写入样式
docEl.firstElementChild.appendChild(fontEl);
fontEl.innerHTML = 'html{font-size:' + rem + 'px!important;}';
```

>## 介绍flex布局
>flex是弹性布局，用来为盒状模型提供了最大的灵活性
flex-direction  决定项目的排列方向，值为row | row-reverse | column | column-reverse
flex-wrap   决定如何换行，值为nowrap | wrap | wrap-reverse
flex-flow   是flex-direction || flex-wrap 的缩写，默认值为row nowrap
justify-content 定义了项目在主轴上的对齐方式，值为flex-start | flex-end | center | space-between | space-around
align-items 定义项目在交叉轴上如何对齐，值为flex-start | flex-end | center | baseline | stretch;
align-content 定义了多根轴线的对齐方式。

>## 其他css方式设置垂直居中
1. 使用绝对定位和负外边距对块级元素进行垂直居中
2. 使用绝对定位和transform (transform: translate(0, -50%))
3. 父元素使用padding
4. 使用flex
5. 使用line-height
6. 图片使用vertical-align

>## 居中为什么要使用transform（为什么不使用marginLeft/Top）
transform可以在未知宽高的情况下使用，translate移动
```
.container {
    height: 200px;
    position: relative;
}
.center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(50% 50%);
}
```

>## 介绍css3中position:sticky
> 设置了sticky的元素，在屏幕范围（viewport）时该元素的位置并不受到定位影响（设置是top、left等属性无效），当该元素的位置将要移出偏移范围时，定位又会变成fixed，根据设置的left、top等属性成固定位置的效果。

特点:

- 该元素并不脱离文档流，仍然保留元素原本在文档流中的位置。
- 当元素在容器中被滚动超过指定的偏移值时，元素在容器内固定在指定位置。亦即如果你设置了top: 50px，那么在sticky元素到达距离相对定位的元素顶部50px的位置时固定，不再向上移动。
- 元素固定的相对偏移是相对于离它最近的具有滚动框的祖先元素，如果祖先元素都不可以滚动，那么是相对于viewport来计算元素的偏移量

>## 介绍position属性包括CSS3新增
position指定位类型
取值类型有：
static 默认值，没有定位，处于正常的文档流
absolute 绝对定位
relative 相对定位，不脱离文档流，原本位置会被保留
fixed 相对于body的绝对定位
inherit 继承父元素的position属性
sticky  如上

>## 清除浮动 
使用clear: both;

>## 定位问题（绝对定位、相对定位等） 
absolute 绝对定位
relative 相对定位，保留原来的空间

>## 动画的了解
- css3的transition
```
div {
    transition: width 2s linear;
    -moz-transition: width 2s linear;
    -webkit-transition: width 2s linear;
    -o-transition: width 2s linear;
}
```
- css3的animation
@keyframes 用于创建动画
```
@keyframes myfirst {
    from {background: red};
    to {background: yellow};
}

@-moz-keyframes myfirst {           //Firefox
    from {background: red};
    to {background: yellow};
}

@-webkit-keyframes myfirst {        //Safair 和 Chrome
    from {background: red};
    to {background: yellow};
}

@-o-keyframes myfirst {            //Opera
    from {background: red};
    to {background: yellow};
}

div {
    animation: myfirst 0.5s;
    -moz-animation: myfirst 0.5s;
    -webkit-animation: myfirst 0.5s;
    -o-animation: myfirst 0.5s;
}
```
- Jquery的animate函数
>`$(dom).animate(style, speed, easing, callback)`
`$(dom).animate(style, options);`
options = {
    speed: 速度
    easing: 动画的速度函数
    callback: 动画完成之后要执行的函数
    queue: 是否放置在效果队列中，是布尔值，为false则立即开始
    specialEasing: styles参数的一个或多个属性映射及对应的easing函数
}
```
$(.aa). animate({
    width: 300px
}, "slow");
```
- 原生js，使用setInterval进行循环更新

>## 一般怎么组织CSS（Webpack）

>## 两个元素块，一左一右，中间相距10像素
使用margin-left或margin-right

>## 上下固定，中间滚动布局如何实现
使用flex弹性布局
```
/*flex最外框*/
.page{
	display:-webkit-box;
	-webkit-box-orient:vertical;
	height:100%;
}
/*flex内容区*/
.page .content{
	position:relative;
	-webkit-box-flex:1;
	-webkit-flex:1;
	flex:1;
	overflow:auto;
	-webkit-overflow-scrolling:touch;
	background-color:#ccc;
}
/*flex头部*/
.header{
	display:-webkit-box;
}

/*flex底部，按钮平均分布*/
.footer{
	overflow:hidden;
	background:#fff;
}
```

>## CSS选择器有哪些
元素选择器
id选择器
类选择器
属性选择器
后代选择器
子元素选择器
相邻兄弟选择器
伪类
伪元素

>## 盒子模型，以及标准情况和IE下的区别
标准情况下，盒子模型的范围包括margin，border，padding，content，并且content部分不包含其他部分
IE情况下，盒子模型的范围包括margin，border，padding，content，但content部分包含了border和padding

>## 如何实现高度自适应
使用百分比
使用flex

>## rem、flex的区别（root em）
rem：相对于根节点字体大小的值计算的单位，通过只修改根元素的大小，能成比例调整所有字体的大小。
flex: 弹性布局适配

>## em和px的区别
px。绝对尺寸（像素），是相对于显示器屏幕分辨率而言
em。相对长度单位，是相对当前对象内文字的字体尺寸，值不是固定的，会继承父级元素的字体大小

#html
>## 浏览器事件流向
事件捕获 处于目标 事件冒泡

>## html语义化的理解
> 根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。

- 为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构:为了裸奔时好看；
- 用户体验：例如title、alt用于解释名词或解释图片信息、label标签的活用；
- 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
- 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页；
- 便于团队开发和维护，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵循这个标准，可以减少差异化。

>## b和strong的区别
`<strong>` 加重语气标签
`<b>` 粗体标签
WEB标准提倡样式与内容分离，所以纯粹为了达到加粗而使用B已经不建议这样做。
从XHTML文档有意义性及用户体验角度来说，strong更有益，更被建议使用。而SEO方面，则针对优化情况而定。

># http
>## 缓存
>###  cookie，sessionStorage，localeStorage的区别
> cookie是存储在浏览器端，并且随浏览器的请求一起发送到服务器端的，它有一定的过期时间，到了过期时间自动会消失。sessionStorage和localeStorage也是存储在客户端的，同属于web。
> Storage，比cookie的存储大小要大有8m，cookie只有4kb，localeStorage是持久化的存储在客户端，如果用户不手动清除的话，不会自动消失，会一直存在，sessionStorage也是存储在客户端，但是它的存活时间是在一个回话期间，只要浏览器的回话关闭了就会自动消失。

>###  cookie放哪里，cookie能做的事情和存在的价值
cookie存储在浏览器端，存储用户写相关信息，用于唯一验证客户来自于发送的哪个请求。

>###  cookie和token都存放在header里面，为什么只劫持前者
cookie：
用户登录成功以后，会在服务端存一个sesion，同时发送给浏览器端一个cookie，数据需要服务端和浏览器端同时存储，用户进行操作时，需要带上cookie，在服务端进行验证。cookie是有状态的。
token：
用户在进行任何操作时，都要带上token，token只存在客户端，服务器在接收到token时，会进行解析。token是无状态的。token更适合于跨平台，移动平台等。

>###  cookie和session有哪些方面的区别
session也是存储用户的相关信息，只是session存储在服务端，cookie存储在浏览器端。

>###  如何设计一个localStorage，保证数据的实效性

>###  介绍localstorage的API
localStorage.setItem(name, value);
localStorage.getItem(name);
localStorage.clear();
localStorage.key(index);  //获取index位置处的值得名字
localStorage.removeItem(name);


>###  http缓存控制
![](leanote://file/getImage?fileId=5c7a2c71b1e99504bc000000)
浏览器缓存分为强缓存和协商缓存
1、浏览器先根据这个资源的http头信息来判断是否命中强缓存，如果命中则直接加载缓存中的资源信息，并不会将请求发给服务端。此时http状态码为200。
2、协商缓存：如果未命中强缓存，则浏览器会将请求发送至服务器。服务器根据http头信息中的Last-Modify/If-Modify-Since或Etag/If-None-Match来判断是否命中协商缓存。如果命中，则http状态码则返回304，浏览器从缓存中加载资源。

>## 状态码
>###  介绍下HTTP状态码
当浏览者访问一个网页时，浏览者的浏览器会向网页所在的服务器发出请求。当浏览器接收并显示网页时，此网页所在的服务器会返回一个包含http状态码的信息头用以响应浏览器的请求。
1** 信息，服务器收到请求，需要请求者继续执行操作 
2** 成功，操作被成功接收并处理 
3** 重定向，需要进一步的操作以完成请求 
4** 客户端错误，请求包含语法错误或无法完成请求 
5** 服务器错误，服务器在处理请求的过程中发生了错误

>###  403、301、302、304是什么是什么
301 永久移动。请求的资源已被永久的移动到新URI，返回的信息会返回新的URI，浏览器会自动定向到新的URI。今后任何新的请求都应使用新URI代替。
302 临时移动。同301类似。但资源只是临时被移动。客户端应继续使用原有URI。
304 未修改。所请求的资源未被修改，服务端返回此状态时，不会返回任何资源。客户端从缓存中获取资源。
403 服务器理解客户端请求，但是拒绝执行此请求。

>## 跨域
>### 介绍下浏览器跨域
浏览器从一个域名的网页去请求另一个域名的资源时，域名、端口、协议任一不同，都是跨域。
解决方法：
1、CORS
2、jsonp

>### 怎么去解决跨域问题
1、CORS
2、jsonp
3、iframe
4、Nginx
5、图像ping
6、comet

>### jsonp方案需要服务端怎么配合
1、接受验证参数
根据与前端Ajax约定的jsonp参数名来验证参数
2、构造参数并返回
将接收的的验证参数callback与实际要返回的json参数按“callback(json)”的方式来构造。

>### Ajax发生跨域要设置什么（前端）


>### 加上CORS之后从发起到请求正式成功的过程
1、对于简单请求，浏览器直接发出CORS请求，在HTTP HEADER中增加了Origin字段。
2、服务器根据这个值，决定是否同意这次请求。
如果Origin制定的源不在许可范围之内，服务器会返回一个正常的HTTP回应。浏览器发现Http Response头信息中没有包含Access-Control-Allow-Origin字段，就判断出错并抛出一个错误，被XMLHttpRequest的onerror回调函数捕获。注意，这种错误无法通过状态码识别，因为HTTP回应的状态码有可能是200.
  如果Origin制定的域名在许可范围内，服务器返回的响应中多出几个头信息字段。
  
>### xsrf跨域攻击的安全性问题怎么防范
>xsrf: cross-site request forgery（跨站点请求伪造），也称为CSRF，是一种常见的web攻击方式。

通过加入一个供校验的action token。

>### CORS如何设置
通过在header中设置Access-Control-Allow-Origin

>### jsonp为什么不支持post方法
JSONP的最基本的原理是：动态添加一个`<script>`标签，而script标签的src属性是没有跨域的限制的。这样说来，这种跨域方式其实与ajax XmlHttpRequest协议无关了。
可以说jsonp的方式原理上和`<script src="http://跨域/...xx.js"></script>`是一致的，因为他的原理实际上就是 使用js的script标签 进行传参，那么必然是get方式的了，和浏览器中敲入一个url一样

##其他
>### 常见Http请求头
请求头|说明|示例
--|--|--
Accept|可接受的响应内容类型|Accept:text/plain
Accept-Charset|可接受的字符集|Accept-Charset:utf-8
Accept-Encoding|可接受的响应内容的编码方式|Accept-Encoding:gzip,deflate
Accept-Language|可接受的响应内容语言列表|Accept-Language:en-US
Cache-Control|用来指定当前的请求/回复中，是否使用缓存机制|Cache-Control:no-cache
Connection|客户端（浏览器）想要优先使用的连接类型|Connection:keep-alive
Cookie|由之前服务器通过set-Cookie设置的一个HTTP协议Cookie|Cookie:$Version=1;skin=new
Host|表示服务器的域名以及服务器所监听的端口号。如果所请求的端口是对应的服务的标准端口（80），则端口号可以省略|Host:www.baidu.com:80
Referer|发出请求的页面的URI|Referer:http://www.baidu.com/nodejs
User-Agent|浏览器的身份标识字符串|User-Agent:Mozila/...

>### Http报文的请求会有几个部分
http协议报文
> 请求报文
>> 请求行：请求方法字段、URL字段、HTTP协议版本
>> 请求头
>> 请求数据：post方法中，会把数据以key、value形式发送请求

> 响应报文
>> 状态行
>> 消息报文
>> 响应正文

>### 从输入URL到页面加载全过程
1、浏览器构建HTTP Request请求
2、网络传输
3、服务器构建HTTP Response响应
4、网络传输
5、浏览器渲染页面

>### tcp3次握手
三次握手
![](leanote://file/getImage?fileId=5c7b75407aa4883b3a000001)
1、在创建连接时，客户端首先要SYN=1，表示要创建连接
2、服务端收到后，要告诉客户端，表示接受到了，所以要加一个ACK=1，就变成ACK=1,SYN=1
3、为防止意外，客户端要再发一个消息给服务端确认一下，这时只需返回ACK=1。

四次分手
![](leanote://file/getImage?fileId=5c7b76287aa4883b3a000002)
1.首先客户端请求关闭客户端到服务端方向的连接，这时客户端就要发送一个FIN=1，表示要关闭一个方向的连接（见上面四次分手的图）
2.服务端接收到后是需要确认一下的，所以返回了一个ACK=1
3.这时只关闭了一个方向，另一个方向也需要关闭，所以服务端也向客户端发了一个FIN=1 ACK=1
4.客户端接收到后发送ACK=1，表示接受成功

>### tcp属于哪一层
1 物理层 -> 2 数据链路层 -> 3 网络层(ip)-> 4 传输层(tcp) -> 5 应用层(http)
![](leanote://file/getImage?fileId=5c7b74077aa4883b3a000000)

>### 缓存相关的HTTP请求头
Expire 缓存过期时间
Cache-Control  用来指定当前的请求/回复中的，是否使用缓存机制。
Pragma 与具体的实现相关，这些字段可能在请求/回应链中的任何时候产生。
Last-Modified 所请求的对象的最后修改日期

>### 介绍HTTPS
https: 是以安全为目标的HTTP通道，简单讲是HTTP的安全版， 即HTTP下加入SSL层，HTTPS的安全基础是SSL， 因此加密的详细内容就需要SSL。 https协议的主要作用是： 建立一个信息安全通道，来确保数组的传输，确保网站的真实性。

>### HTTP和HTTPS的区别
1、Https协议需要ca证书，费用较高。
2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
3、使用不同的链接方式，端口也不同，一般而言，http协议的端口为80，https的端口为443。
4、http的连接很简单，是无状态的。
5、HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

>### HTTPS怎么建立安全通道
通过加入SSL层进行加密

>### HTTPS的加密过程
![](leanote://file/getImage?fileId=5c7b7b827aa4883b3a000003)
1. 客户端发起HTTPS请求
2. 服务端配置
3. 传送证书
4. 客户端解析证书
5. 传送加密信息
6. 服务端解密信息
6. 传输加密后的信息
7. 客户端解密信息

>### 介绍下数字签名的原理
数字签名技术就和对“非对称密钥加解密”和“数字摘要”两项技术的应用，它将摘要信息用发送者的私钥加密，与原文一起传送给接收者。接收者只有用发送者的公钥才能解密被加密的摘要信息，然后用HASH函数对收到的原文产生一个摘要信息，与解密的摘要信息对比。如果相同，则说明收到的信息是完整的，在传输过程中没有被修改，否则说明信息被修改过，因此数字签名能够验证信息的完整性。
数字签名的过程如下：
明文 ——> hash算法 ——> 摘要 ——> 私钥加密 ——> 数字签名
数字签名有两种功效：
一、能确定消息确实是由发送方签名并发出来的，因为别人假冒不了发送方的签名。
二、数字签名能确定消息的完整性。

>### 前后端通信使用什么方案
靠的是HTTP，前端发起HTTP请求的方式：
1、Ajax
2、WebScoket

>### RESTful常用的Method

>### Access-Control-Allow-Origin在服务端哪里配置
把response的header头中设置Access-Control-Allow-Origin为制定可请求当前域名下数据的域名即可

>### 前端和后端怎么联调

>### 介绍SSL和TLS
1. SSL（Secure Socket Layer，安全套接字层）
SSL为Netscape所研发，用以保障在Internet上数据传输之安全，利用数据加密技术，可确保数据在网络上传输过程中不会被截取，当前为3.0版本。
SSL协议可分为两层：SSL记录协议（SSL Recore Protocol）：它建立在可靠地传输协议（如TCP）之上，为高层协议提供数据封装、压缩、加密等基本功能的支持。SSL握手协议（SSL Handshake Protocol）：它建立在SSL记录协议之上，用于在实际的数据传输开始前，通讯双方进行身份证、协商加密算法、交换加密密钥等。
2. TLS（Transport Layer Security，传输层安全协议）
用于两个应用程序之间提供保密性和数据完整性。
TLS 1.0是IETF（Internet Engineering Task Force，Internet工程任务组）制定的一种新的协议，它建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本，可以理解为SSL 3.1，它是写入了RFC的。该协议由两层组成：TLS记录协议（TLS Recore）和TLS握手协议（TLS Handshake）。较低的层为TLS记录协议，位于某个可靠的传输协议（例如TCP）上面。

>### 介绍DNS解析
1. 浏览器会检查缓存中有没有这个域名对应的解析过的IP地址，如果缓存中有，这个解析过程就将结束。浏览器对域名的缓存是有时间限制，一般情况下是几十秒，不同的浏览器TTL不同。谷歌浏览器可以通过这个地址来查看域名过期情况。chrome://net-internals/#dns
2. 如果用户的浏览器缓存中没有，浏览器会查找操作系统缓存中是否有这个域名对应的DNS解析结果。本机操作系统缓存的DNS域名结果存在的时间长短不一致。可以通过ipconfig/displaydns来查看本机操作对各个域名的缓存结果。
3. 如果在本机操作系统缓存中还是无法获取到对应的DNS，那么就会读取操作系统中的静态DNS即hosts文件。文件的存放地址如下： 
C:\Windows\System32\drivers\etc\hosts
4. 如果在hosts文件中还是无法找到对应的DNS，那么就会到ISP中的DNS服务器中取查找。这个ISP就是你连接网络的供应商，如电信、联通。至此，绝大多数的DNS都会在这里被解析到。ISP中的DNS也有缓存，我们可以通过nslookup -d www.zhihu.com来查看。
5. 如果在ISP的DNS服务器中还是无法找到，就会到国际顶级域名服务器gTLD，如.com、.cn、.org等。
6. 找到对应DNS的对应关系之后会返回一个TTL值和IP，ISP的DNS服务器就会根据这个TTL值来进行缓存，并继续返回给客户端，本地操作系统得到这个值之后也会进行缓存。

># js
>## 闭包
>### 请简单描述闭包与垃圾回收
闭包是指有权访问另一个函数作用域中的变量的函数。
垃圾回收机制是为了以防内存泄漏，内存泄漏的含义就是当已经不需要某块内存时这块内存还存在着，垃圾回收机制就是间歇的不定期的寻找到不再使用的变量，并释放掉它们所指向的内存。
 
>### JS里垃圾回收机制是什么，常用的是哪种，怎么处理的
常用的垃圾回收机制策略：
1、标记清除
垃圾收集器给内存中的所有变量都加上标记，然后去掉环境中的变量以及被环境中的变量引用的变量的标记。在此之后再被加上的标记的变量即为需要回收的变量，因为环境中的变量已经无法访问到这些变量。
2、引用计数
机制就是跟踪一个值的引用次数，当声明一个变量并将一个引用类型赋值给该变量时该值引用次数加1，当这个变量指向其他一个时该值的引用次数便减一。当该值引用次数为0时就会被回收。

>### 对闭包的看法，为什么要用闭包
只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。所以本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
闭包可以读取函数内部的变量，可以让变量的值始终保持在内存中。

>### 介绍闭包以及闭包为什么没清除
因为匿名函数的作用域链仍然在引用这个活动对象。

>### 闭包的使用场景
场景一：采用函数引用方式的setTimeout调用
场景二：将函数关联到对象的实例方法
场景三：封装相关的功能集

>## 原型
>### 请用es5实现原型链继承
```javascript
function SuperType() {
    this.property = true;
}

function SubType() {
    this.subproperty = false;
}

SubType.prototype = new SuperType();

var instance = new SubType();
console.log(instance.property);     //输出true
```

>### 介绍下原型链（解决的是继承问题吗）
原型链是实现继承的主要方法，基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法

>### JS的继承方法
使用原型链

>### 使用原型最大的好处
通过该构造函数生成的实例所拥有的方法都是指向一个函数的索引，这样可以节省内存。

>## 异步
>### promise、async有什么区别
promise简单来说一个容器，保存着一个异步操作结果。 对象的状态不受外界影响，且有三个状态pending，fulfilled，rejected。
async寄生于promise是generator生成器的语法糖，async用于声明一个function是异步的，await可以认为是async wait的简写，等待一个异步方法执行完成。

>### 对async、await的理解，内部原理


>### 介绍下Promise，内部实现
每个promise都会经历一个短暂的生命周期：先是处于进行中的状态，此时操作尚未完成，所以他也是未处理的，一旦异步操作执行结束，promise则变为已处理的状态。
内部属性[[PromiseState]]被用来表示Promise的3种状态：pending，fulfilled以及rejected。当promise的状态改变时，通过then方法采取特定的行动。

>### 如何设计Promise.all()

>### 使用Async会注意哪些东西
1、async表示这是一个async函数，await只能用在这个函数里面
2、await表示在这里等待promise返回结果后，再继续执行
3、await后面跟着的应该是一个promise对象

>### Async里面有多个await请求，可以怎么优化（请求是否有依赖）
可以使用promise.all让多个await操作并行

>### Promise和Async处理失败的时候有什么区别
async：
如果是promise的reject状态，可以通过try catch进行捕获
如果是当内部出现一些错误时，需要通过catch回调捕获。

>### Promise和Callback有什么区别
callback嵌套多，可读性较差，深度开发
promise宽度开发，promise链，解决嵌套多的问题
深度与宽度的区别

>### Promise有几个状态
pending,fulfilled,rejecte

>### JS怎么实现异步
可通过promise，async

>### 异步整个执行周期
![](leanote://file/getImage?fileId=5c8767f566c73379aa000000)

>### Promise和setTimeout执行先后的区别
promise是微任务
settimeout是宏任务

>### Promise构造函数是同步还是异步执行，then呢
promise构造函数式同步执行
then是异步执行

>### Promise有没有解决异步的问题
promise链是真正强大的地方

>### Promise和setTimeout的区别
Event Loop
进入的事件队列不同

>### JS异步解决方案的发展历程以及优缺点
嵌套回调
promise
generator
async、await

>### promise的精髓，以及优缺点

>## ES6
>### 介绍class和ES5的类以及区别
class只是es5中类的语法糖，并没有改变类的本质。
区别：
1. 函数声明可以被提升，而类声明与let声明类似，不能被提升：真正执行声明语句之前，它们会一直存在于临时死区中。
2. 类声明中的所有代码将自动运行在严格模式下，而且无法强行让代码脱离严格模式执行
3. 在自定义类型中，需要通过Object.defineProperty()方法手工指定某个方法为不可枚举；而在类中，所有方法都不可枚举
4. 每个类都有一个名为[[Construct]]的内部方法，通过关键字new调用那些不含[[Construct]]的方法会导致程序抛出错误
5. 使用关键字new以外的方法调用类的构造函数会导致程序抛出错误
6. 在类中修改类名会导致程序报错

>### ES6中let块作用域是怎么实现的
js预编译时，不把let声明的变量提升到顶部，而是把它放到临时死区，只有执行过变量声明语句后，变量才会从临时死区中移出，然后方可正常访问。

>### 介绍箭头函数和普通函数的区别
箭头函数与普通函数的不同之处在于：
1. 没有this、super、arguments 和 new.target绑定
2. 不能通过new关键字调用
3. 没有原型
4. 不可以改变this的绑定
5. 不支持arguments对象
6. 不支持重复的命名参数

>### let、const以及var的区别
var 全局声明；提升机制：声明后，会被当成在当前作用域顶部声明的变量；会覆盖已存在的全局属性
let、const 块级声明；不会被提升，会出现临时死区，且不能重声明；不能覆盖全局属性，只能遮蔽它。
const 只能声明常量，声明后不能修改绑定，但允许修改值（针对const声明对象）

>### 介绍箭头函数的this
箭头函数中没有this的绑定，必须通过查找作用域链来决定其值。
如果箭头函数被非箭头函数包含，则this绑定的是最近一层非箭头函数的this；否则，this的值会被设置为全局对象。

>### ES6使用的语法

>### ES6中的map和原生的对象有什么区别
object和map存储的都是键值对组合
区别：
object的键的类型是字符串
map的键的类型可以是任意类型

>### ES5和ES6有什么区别
es6新增功能：
1. 块级作用域 let和const
2. 箭头函数、默认参数、不定参数
3. 对象字面量语法的变化，新的反射方法
4. 对象和数组的解构
5. Symbol 和 Symbol属性
6. Set集合与Map集合
7. Iterator和Generator
8. 类class
9. 数组增加了新方法
10. 异步编程 promise
11. 代理proxy和反射reflection 的api
12. 用模块封装代码

>## 防抖和节流
>### 什么是防抖和节流？有什么区别？如何实现
防抖函数：指触发事件的n秒内函数只执行一遍，如果在n秒内又触发了事件，则会重新计算函数执行时间
```
function debounce(fun, wait) {
    let timeout;
    return function() {
        let context = this;
        let args = arguments;
        if(timeout) {
            clearTimeout(timeout);
        }
        timeout = setTimeout(() => {
            func.apply(context, args);
        }, wait)
    }
}
```
节流函数：指连续触发事件，但在n秒内只执行一次函数。
```
function throttle(fun, wait) {
    let timeout;
    return function() {
        let context = this;
        let args = arguments;
        if(!timeout) {
            timeount = setTimeout(function() {
                timeout = null;
                fun.apply(context, args);
            }, wait)
        }
    }
}
```

>### 搜索请求如何处理
使用防抖

>## 其他
>### 什么是工厂模式和策略模式
工厂模式属于创建型模式，策略模式属于行为式模式
工厂模式： 
1）根据你给出的目的来生产不同用途的斧子，例如要砍人，那么工厂生产砍人斧子，要伐木就生产伐木斧子。 
2）即根据你给出一些属性来生产不同行为的一类对象返回给你。 
3）关注对象创建
策略模式： 
1）用工厂生产的斧子来做对应的事情，例如用砍人的斧子来砍人，用伐木的斧子来伐木。 
2）即根据你给出对应的对象来执行对应的方法。 
3）关注行为的选择

>### 请手写实现浅拷贝与深拷贝
```
var a = {name: 'aa', value: 11};

//浅拷贝
var b = a;
//深拷贝
//方法一
var b = JSON.parse(JSON.stringfy(a));
//方法二
function deepCopy(obj) {
    var result = Array.isArray(obj) ? [] : {};
        for (var key in obj) {
            if (obj.hasOwnProperty(key)) {
                if (typeof obj[key] === 'object') {
                    result[key] = deepCopy(obj[key]);   //递归复制
                } else {
                    result[key] = obj[key];
                }
            }
        }
    return result;
}
```

>### 通过什么做到并发请求

>### 改变函数内部this指针的指向函数，bind、call、apply的区别
> call和apply，假设要改变fn函数内部的this的指向，指向obj，那么可以fn.call(obj);或者fn.apply(obj);那么问题来了，call和apply的区别是什么，其是call和apply的区别在于参数，他们两个的第一个参数都是一样的，表示调用该函数的对象，apply的第二个参数是数组，是[arg1,arg2,arg3]这种形式，而call是arg1,arg2,arg3这样的形式。还有一个bind函数，var bar=fn.bind(obj);那么fn中的this就指向obj对象了，bind函数返回新的函数，这个函数内的this指针指向obj对象。

>### js题目
>已知如下数组：
var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10];
编写1个程序将数组扁平化去并除其中重复部分数据，最终得到1个升序且不重复的数组。
```
// 扁平化
function getArray(arr) {
	arr.forEach(function(item, index) {
		if(item instanceof Array) {
			getArray(item);
		}else {
			newArr.push(item);
		}
	})
}

//去重
function unique1(arr) {
	return [...new Set(arr)];
}

function unique2(arr) {
	return arr.reduce(function(prev, cur) {
		if(!prev.includes(cur)) {
			prev.push(cur);
		}
	}, [])
}

function unique3(arr) {
	let map = new Map();
	return arr.filter(function(item) {
		if(map.has(item)) {
			return false;
		}else {
			map.set(item);
		}
	})
}

// 排序
function sort1(arr) {
	return arr.sort(function(item1, item2) {
		return item1 - item2;
	});
}

//冒泡
function sort2(arr) {
	for(let i = 0; i < arr.length - 1; i ++) {
		for(let j = 0; j < arr.lenth - 1 - i; j++) {
			if(arr[j] < arr[j + 1]) {
				let temp = arr[j + 1];
				arr[j + 1] = arr[j];
				arr[j] = temp;
			}
		}
	}
	return arr;
}

//选择排序
function sort4(arr) {
	for(let i = 0; i < arr.length; i ++) {
		let minIndex = i;
		for(let j = i + 1; j < arr.length; j ++) {
			if(arr[j] < arr[minIndex]) {
				minIndex = j;
			}
		}
		if(minIndex != i) {
			let temp = arr[minIndex];
			arr[minIndex] = arr[i];
			arr[i] = temp;
		}
	}
	return arr;
}

//快速排序
function sort3(arr, left, right) {
	let partitionIndex;
	left = typeof left != 'number' ? 0 : left,
	right = typeof right != 'number' ? arr.length - 1 : right;
	if(left < right) {
		partitionIndex = partition(arr, left, right);
		sort3(arr, left, partitionIndex - 1);
		sort3(arr, partitionIndex + 1, right);
	}
}

function partition(arr, left, right) {
	let pivot = left,
		index = pivot + 1;
	for(i = 0; i <= right; i++) {
		if(arr[i] < arr[pivot]) {
			swap(arr, i, j)
			index ++;
		}
	}
	swap(arr, pivot, index - 1);
	return index - 1;
}

function swap(arr, i, j) {
	let temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```

>### WebView和原生是如何通信
![](leanote://file/getImage?fileId=5c7f6faf85c4513b01000000)
这个过程是Angular控制器使用Cordova JavaScript API调用Cordova，Cordova使用原生SDK和设备通信。

>### == 和 ===的区别，什么情况下用相等==
== 在表达式两边数据类型不一致时，会隐式转换为相同数据类型，然后对值进行比较
=== 恒等于，不会进行类型转换，在比较时除了对值进行比较以外，还比较两边的数据类型

>### 观察者和发布-订阅的区别

>### 介绍纯函数
- 函数的返回结果只依赖于它的参数。
- 函数执行过程里面没有副作用。

>### sum(2, 3)实现sum(2)(3)的效果
函数柯里化, 保证一次传入更少的参数, 做更加明确的事情
```
function sum() {
    if(arguments.length == 2) {
        return arguments[0] + arguments[1];
    }else {
        return function(sec) {
            return sec + arguments[0];
        }
    }
}
```
>### 两个对象如何比较
方法一：通过JSON.stringfy
方法二：通过深度对比，遍历每个属性

>### 变量作用域链
这个作用域链是一个对象列表或者链表，这组对象定义了这段代码作用域中的变量。
当查找变量x的值时，他会从链中的第一个对象开始查找，如果这个对象有一个名为x的属性，则会直接使用这个属性的值，如果不存在，则会继续查找链上的下一个对象。如果第二个对象依然没有名为x的属性，则会继续查找下一个对象，以此类推。如果作用域上没有任何一个对象含有属性x，则认为这段代码的作用域链上不存在x，并最终抛出一个引用错误异常。

>### 介绍DOM树
dom是一个由多层节点构成的结构。可以通过js操作这个节点树，进而改变底层文档的外观和结构。

>### JS变量类型分为几种，区别是什么
>基本数据类型
>引用数据类型

区别:
1.声明变量时不同的内存分配

- 基础数据：存储在栈中的简单数据段。它们的值直接存储在变量访问的位置。
- 引用数据：存储在堆中的对象。存储在变量处的值是一个指针，指向存储对象的内存地址。

2.不同的内存分配机制也带来了不同的访问机制

- 基础数据：直接访问
- 引用数据：按引用访问

3.复制变量时的不同
4.参数传递的不同

>### [1, 2, 3, 4, 5]变成[1, 2, 3, a, b, 5]
```
var arr = [1,2,3,4,5];
var newarr = arr.splice(3, 1, 'a', 'b');
```

>### 取数组的最大值（ES5、ES6）
```
var arr = [22,13,6,55,30];
//假设法
var max = arr[0];
for(var i = 1; i < arr.length; i++) {
   var cur = arr[i];
   cur > max ? max = cur : null
}

//使用Math的max/min方法
var max = Math.max.apply(null, arr);
var min = Math.min.apply(null, arr);

//使用es6的扩展运算符
console.log(arr.Math(...arr)); // 55
```

>### some、every、find、filter、map、forEach有什么区别
some() 对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true
every() 对数组中的每一项运行给定函数，如果该函数每一项都返回true，则返回true
find() 返回数组中满足提供的测试函数的第一个元素的值，否则返回undefined
filter() 对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组
map() 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组
forEach() 对数组中的每一项运行给定函数。这个函数没有给定值

>### 上述数组随机取数，每次返回的值都不一样

>### 如何找0-5的随机数，95-99呢
```
Math.floor(Math.random() * 5);

Math.floor(Math.random() * 4) + 95;
```

>### 页面上有1万个button如何绑定事件
js原生操作DOM，通过document.getElementsByTagName('button')；获取所有节点，进行事件绑定

>### 如何判断是button
可通过获取节点，node.nodeName进行判断

>### 循环绑定时的index是多少，为什么，怎么解决

>### 页面上有一个input，还有一个p标签，改变input后p标签就跟着变化，如何处理
```js
var input = document.getElementByTagName("input");
input.onchange = functin() {
    var p = document.getElementByTagName("p");
    p.style.color = "red";
}
```

>### 监听input的哪个事件，在什么时候触发
onchange 在用户输入新的文本或编辑已存在的文本时触发，它表明用户完成了编辑并将焦点移出了文本域
onclick 当type=button时，鼠标点击触发
onfocur 当input获得焦点时触发
onblur 当input失去焦点时触发
onkeypress 
onkeydown 当input中有键按下时触发
onkeyup 当input中有键抬起时触发
onselect 当input里的内容文本被选中后执行一段，只要选择了就会触发，不是非得全部选中
oninput  当input的value值发生变化时就会触发，不用等到失去焦点（与onchange的区别）

>### prototype和——proto——区别
![](leanote://file/getImage?fileId=5c80c1563976a43eb7000000)
1、JavaScript中的函数是对象，而且除了使用字面量定义外，都需要通过函数来创建对象；
2、prototype是构造函数访问原型对象，`__proto__`是对象实例访问原型对象。

>### _construct是什么

>### new是怎么实现的
1. 创建一个新对象
2. 将构造函数的作用域赋给新对象
3. 执行构造函数中的代码
4. 返回新对象

>### Array是Object类型吗
Array不是Object类型，但两者都是引用类型
Array是有序数据的集合
Object是无序数组的集合

>### var a = {name: "前端开发"}; var b = a; a = null那么b输出什么
输出 {name: "前端开发"}

>### var a = {b: 1}存放在哪里

>### var a = {b: {c: 1}}存放在哪里

>### 栈和堆的区别
1、空间分配

- 栈 由操作系统自动分配释放
- 堆 一般由程序员分配释放

2、缓存方式

- 栈 使用的是一级缓存，它们通常都是被调用时处于存储空间中，调用完毕后立即释放
- 堆 存放在二级缓存中，生命周期由虚拟机的垃圾回收算法来决定

3、数据结构

- 栈 一种先进后出的数据结构
- 堆 可以看成是树

>### 垃圾回收时栈和堆的区别

>### 数组里面有10万个数据，取第一个元素和第10万个元素的时间相差多少
复杂度为O(1);

>### JS为什么要区分微任务和宏任务
宏任务：包括整段代码的script，setTimeout，setInterval
微任务：Promise，process.nextTick
![](leanote://file/getImage?fileId=5c80db9a3976a43eb7000001)
![](leanote://file/getImage?fileId=5c80dba63976a43eb7000002)
```
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
}).then(function() {
    console.log('then');
})

console.log('console');
```
- 这段代码作为宏任务，进入主线程。
- 先遇到setTimeout，那么将其回调函数注册后分发到宏任务Event Queue。(注册过程与上同，下文不再描述)
- 接下来遇到了Promise，new Promise立即执行，then函数分发到微任务Event Queue。
- 遇到console.log()，立即执行。
- 好啦，整体代码script作为第一个宏任务执行结束，看看有哪些微任务？我们发现了then在微任务Event Queue里面，执行。
- ok，第一轮事件循环结束了，我们开始第二轮循环，当然要从宏任务Event Queue开始。我们发现了宏任务Event Queue中setTimeout对应的回调函数，立即执行。
- 结束。

>### JS执行过程中分为哪些阶段
1、预编译：把所有的函数定义提前，所有的变量声明提前，变量的赋值不提前
2、执行：从上到下执行

>### 词法作用域和this的区别

>### 平常是怎么做继承
通过原型链实现

>### 使用canvas绘图时如何组织成通用组件

>### formData和原生的ajax有什么区别

>### 介绍下表单提交，和formData有什么关系

>### 如何判断一个变量是不是数组
Array.isArray();
instanceof Array

>### 变量a和b，如何交换
```
function swap(item1, item2) {
    let temp = item1;
    item1 = item2;
    item2 = temp;
}

//es6：使用解构方法
var a = 1;
var b = 2;

[b, a] = [a, b]; v

```

>### 事件委托

>### 多个<li>标签生成的Dom结构是一个类数组

>### 类数组和数组的区别
类数组：

- 拥有length属性，其它属性（索引）为非负整数(对象中的索引会被当做字符串来处理，这里你可以当做是个非负整数串来理解)
- 不具有数组所具有的方法

javascript常见的类数组有`arguments`对象和 DOM方法的返回结果。比如 `document.getElementsByTagName()`。

>### dom的类数组如何转成数组
Array.prototype.slice.call(arrayLike)

>### 介绍单页面应用和多页面应用
![](leanote://file/getImage?fileId=5c80dfff3976a43eb7000003)

>### 介绍this和原型

>### setInterval需要注意的点

>### 定时器为什么是不精确的
在js中如果打算使用setInterval进行倒数,计时等功能,往往是不准确的,因为setInterval的回调函数并不是到时后立即执行,而是等系统计算资源空闲下来后才会执行.而下一次触发时间则是在setInterval回调函数执行完毕之后才开始计时,所以如果setInterval内执行的计算过于耗时,或者有其他耗时任务在执行,setInterval的计时会越来越不准,延迟很厉害.

>### setTimeout(1)和setTimeout(2)之间的区别

>### 介绍defineProperty方法，什么时候需要用到
`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

vue.js通过这个方法实现数据的双向绑定
可通过这个方法实现对对象属性值的监听。

>### for..in 和 object.keys的区别
for..in
使用for..in循环时，返回的是所有能够通过对象访问的、可枚举的属性，既包括存在于实例中的属性，也包括存在于原型中的实例。这里需要注意的是使用for-in返回的属性因各个浏览器厂商遵循的标准不一致导致对象属性遍历的顺序有可能不是当初构建时的顺序。

object.keys
用于获取对象自身所有的可枚举的属性值，但不包括原型中的属性，然后返回一个由属性名组成的数组。注意它同for..in一样不能保证属性按对象原来的顺序输出。

Object.getOwnPropertyNames()
方法返回对象的所有自身属性的属性名（包括不可枚举的属性）组成的数组，但不会获取原型链上的属性。

>### get和post有什么区别
- 本质
get是向服务器索取数据的一种请求，post是向服务器提交数据的一种请求
- 安全性
get安全性较低，但执行效率较高，post较安全


>### JS是什么范式语言(面向对象还是函数式编程)
JavaScript是多范式语言
- 命令式
- 面向对象
- 函数式


># 算法
>## 如何判断链表是否有环

>## 介绍二叉搜索树的特点

>## 介绍暂时性死区

>## 介绍冒泡排序，选择排序，冒泡排序如何优化

>## 介绍快速排序

>## 算法：前K个最大的元素

>## 介绍下DFS深度优先

>## 介绍下观察者模式

>## 观察者模式里面使用的数据结构(不具备顺序 ，是一个list)

># 优化
>## 整个前端性能提升大致分几类

>## 前端性能优化

>## 前端性能优化（JS原生和React）

>## 用户体验做过什么优化

>## 如何实现异步加载

>## 如何实现分模块打包（多入口）

>## 前端性能优化（1js css；2 图片；3 缓存预加载； 4 SSR； 5 多域名加载；6 负载均衡）

>## base64为什么能提升性能，缺点

># 小程序
>## 小程序里面开页面最多多少





