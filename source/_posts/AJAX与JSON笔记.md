---
title: Ajax与Json笔记
date: 2022-02-20 02:25:00
author: 丁兆龙
img: 
top: false
mathjax: false
summary: Ajax与Json的笔记小结
categories: 

tags:

  - 前端
  - Ajax
  - Json
---



## 前端开发的演变

- **静态网页阶段**

  没有数据交互,所有数据都由后端生成,前端只负责展示.如果需要更新页面内容,必须重新加载整个网页.

  当只需要更新页面中部分内容时,也必须重载页面中的所有资源.用户体验差,还增加了服务器的负担.

- **Ajax 阶段**

  `Ajax` 全称是 Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）,是一种用于创建动态网页的技术.

  为了解决上述问题,04 年诞生了 Ajax 技术,让页面可以进行局部刷新,使前端不仅仅是展示页面,还能管理数据并与用户互动.
  
  Ajax 技术指的是脚本独立向服务器请求数据,拿到数据后进行处理并更新网页.后端只是负责提供数据,其他事情都由前端处理,实现了 "获取数据 → 处理数据 → 展示数据" 的完整业务逻辑.
  
  Ajax 可以提高系统性能,优化用户界面.很多框架以及代码库已将 Ajax 作为其必不可少的一个重要模块.



### 协议、域名、端口

> 协议、域名、端口指的是一个 URL 地址中的不同部分。

**一个 URL 地址可以有以下几个组成部分：**`scheme`**: //**`host`**:**`post`**/**`path`**?**`query`**#**`fragment`

- **scheme**:  通信协议，一般为 http 、https；
- **host**:  域名；
- **post**:  端口号，此项为可选项，http 协议默认的端口号为 80，https 协议默认的端口号为 443；
- **path**:  路径，由 "/ "隔开的字符串；
- **query**:  查询参数，此项为可选项；
- **fragment**:  信息片段，用于指定网络资源中的某片断，此项为可选项；



## Ajax 工作原理

  在 `客户端浏览器` 和 `服务器` 之间加了一个中间层：Ajax 引擎.由 Ajax 引擎独立向服务器请求数据，前端获取到 Ajax 返回的数据后，可以使用新数据来更新页面、或进行其它操作，使用户请求和服务器响应异步化，从而保证了在不刷新页面的前提下可以局部更新网页内容。

![](http://r7lh60uut.hb-bkt.clouddn.com/AJAX%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.jpg)

## Ajax 的优缺点

**优点：**

1. 页面无刷新更新，增强用户体验；
2. 使用异步方式与服务器通信，响应能力更迅速；
3. 可以将一些服务器工作转移到客户端，利用客户端资源来处理，减轻服务器和带宽的压力，节约空间和带宽租用成本；
4. 技术标准化，并被浏览器广泛支持，不需要下载插件或者小程序；
5. Ajax 可使因特网应用程序更小、更快、更友好。

**缺点：**

1. Ajax 不支持浏览器 back 返回按钮；
2. 有安全问题，Ajax 暴露了与服务器交互的细节；
3. 对搜索引擎不友好；
4. 破坏了程序的异常机制；
5. 不容易调试。



## 同源策略

同源策略是一种安全协议，是客户端脚本（尤其是 JavaScript）中重要的安全度量标准，

指**一段脚本只能读取同一来源的窗口和文档的属性**,

URL 地址中的 **协议**、**域名**、**端口** 三者 **都** 相同才能被称为 **同源**.

**例**: https://www.baidu.cn

- https://www.w3cschool.cn/tutorial 同源

- http://www.w3cschool.cn 不同源,协议不同

- https://pm.baidu.cn 不同源,域名不同

- https://www.pm.w3cschool.cn:8822 不同源,端口不同

### 同源策略的必要性

为了数据的安全性,浏览器需要有"同源策略".

若没有同源策略,黑客就可以在页面上任意请求后端数据,造成数据库内容被盗取,隐私数据泄漏等.

**在使用 Ajax 请求后端数据时,只能跟同源的后端接口进行数据交互**,

即：后端接口的 URL 与发起 Ajax 请求的页面 URL 之间,需要满足同源策略.

不满足 "同源策略" 的请求会使浏览器报错.

> 实现跨域请求的方式:

在一些场景中，需要 Ajax 访问不同源的数据(称为跨域访问),这时需要后端服务器进行相应设置.如果服务器端支持 CORS，可以通过设置`Access-Control-Allow-Origin`来实现跨域。如果浏览器检测到相应的设置，就会允许 Ajax 进行跨域访问。



## XMLHttpRequest 类

> Ajax 技术的核心是`XMLHttpRequest`类，简称 XHR，它允许脚本异步调用HTTP API。

浏览器在`XMLHttpRequest`类上定义了 HTTP API，这个类的每个实例都表示一个 **独立** 的 **请求/响应 对象**，并且这个实例对象上的属性和方法允许 **指定细节** 和 **提取响应数据**。

```javascript
var xhr = new XMLHttpRequest();//实例化
```



### open 指定请求

> 创建 XMLHttpRequest 对象之后，发起 HTTP 请求的下一步是调用 XMLHttpRequest 对象的`open`方法，指定 HTTP 请求的两个必需部分：**请求方法** 和 **URL**

```javascript
xhr.open("GET", "/statics/demosource/demo_get_json.php");
```

**解析：xhr.open(method, url, async)**

- **method**:  用于指定 HTTP 请求的方法，不区分大小写；
- 可取的值包括："GET"、"POST"、"HEAD"、"PUT"、"OPTIONS"、"DELETE".

- **url**:  用于指定 HTTP 请求的 URL 地址，可以是 **绝对URL** 或 **相对URL**.
- **async**:  可选，指定脚本是否以异步方式调用此次 Ajax 请求;默认为 true,不阻塞后续脚本的执行.

>允许脚本以异步的方式发起 Ajax 请求，是`XMLHttpRequest`技术的一个重要特性，可避免发生因网速慢等原因导致脚本代码阻塞、页面卡死现象；
`open()`方法还可以有第四、第五个参数，分别用于 HTTP 请求访问认证的用户名和密码，使用它们需要在服务器做相应的配置，较为少用。



### setRequestHeader 设置请求头

>  如果你的 HTTP 请求需要设置请求头，那么调用 open 方法之后的下个步骤就是设置它，使用的方法是：`setRequestHeader`

**示例**:  POST 请求设置 "Content-Type" 头来指定请求主体的编码格式

```javascript
//在open方法之后设置请求头:
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
```

**解析：xhr.setRequestHeader(name, value)**

- **name**:  请求头名称；
- **value**:  请求头的值。

>如果对相同的头调用`setRequestHeader`多次，新值不会取代之前指定的值，相反，HTTP 请求将包含这个头的多个副本或将这个头指定多个值；
>不能自己指定 "Content-Length"、"Date"、"Referer" 或 "User-Agent" 头，`XMLHttpRequest`将自动添加这些头而防止伪造它们。类似地，`XMLHttpRequest`对象还会自动处理 cookie、链接时间、字符集和编码判断，所以无法使用`setRequestHeader`方法设置它们。



### send 发送请求主体

> 使用 XMLHttpRequest 发起 HTTP 请求的最后一步是指定可选的请求主体、并向服务器发送它，使用的方法是`send`

- **示例1:GET请求：**

```javascript
xhr.send(null);//使用GET请求时,由于它没有请求主体,所以send可以传递null或省略这个参数.
```

- **示例2:POST 请求**

```javascript
//使用POST请求时,可在send方法中指定请求主体,且应该匹配setRequestHeader方法所指定的"Content-Type" 头.
xhr.send(msg);
```



## 获取服务器响应

> 一个完整的 HTTP 响应由 状态码、响应头和 响应主体 组成，这三者都可以通过`XMLHttpRequest`对象提供的属性和方法获取。

HTTP 请求时间的长短依赖于网速等原因,为了能够在 HTTP 响应准备就绪时得到通知，必须监听`XMLHttpRequest`对象上的`readystatechange`事件。

为了理解这个事件类型，需要先了解`readyState`属性，因为该事件监听的是`readyState`属性值的改变。

`readyState`属性在 HTTP 请求过程中，会从 0 变到 4

```javascript
var xhr = new XMLHttpRequest();
alert(xhr.readyState);// readyState 初始为 0
xhr.onreadystatechange = function () {    // 监听 readyState 属性的改变
    alert(xhr.readyState);
}
xhr.open("GET", "/statics/demosource/demo_get_json.php");xhr.send();
```



### readyState 属性

**`readyState`属性是一个整数，它的值代表了不同的 HTTP 请求状态。**

- 0：初始值，表示请求未初始化，`open`方法尚未调用；
- 1：启动请求，open 方法已经调用，但尚未调用 send 方法；
- 2：请求发送，已经调用 send 方法，但尚未接收到响应；
- 3：接收响应，已经接受到部分响应数据，主要是响应头；
- 4：HTTP 响应完成，已经接收到全部响应数据，而且可以在客户端使用。 

每次`readyState`属性值的改变都会触发`readystatechange`事件，属性值为 4 时表示 HTTP 的响应准备就绪.

因此，我们 Ajax 请求的代码通常都是这样的：

```javascript
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) {
        // HTTP响应完成
    }
};
xhr.open("GET", "/statics/demosource/demo_get_json.php");xhr.send();
```

**说明：**

- `readyState`的属性值只代表此时的 HTTP 请求处于哪个阶段：是发送了请求还是未发送请求，是只接收到了响应头还是响应完成；
- "响应完成" 只代表 HTTP 请求结束，至于服务器的响应状态：是请求成功还是请求错误，又或者是服务器错误，需要通过 HTTP 状态码判断，它存储在`XMLhttpRequest`的`status`属性上；



### status 属性

> `status`属性会以数字的形式保存服务器响应的 HTTP 状态码，诸如使用最频繁的 "200" 表示请求成功，"404" 表示 URL 不能匹配服务器上的任何资源。

HTTP 状态码是用来表示网页服务器响应状态的 3 位数字代码，所有状态码的第一个数字代表了响应的五种状态之一：

- 1xx：临时响应
- 2xx：成功
- 3xx：重定向
- 4xx：请求错误
- 5xx：服务器错误

**2开头的状态码** 与 **304**表示我们可以获取到 HTTP 响应数据。

2开头的状态码都表示请求成功，而 304 是对客户端可读取缓存的一种响应，同样能获取到 HTTP 的响应数据。



**示例**:  Ajax 请求成功

```javascript
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;    // 如果http响应不成功直接返回
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        // 获取到响应数据，可执行一些请求成功的回调函数
    }
};

xhr.open("GET", "/statics/demosource/demo_get_json.php");xhr.send();
```



### responseText 属性

> `responseText`属性以字符串的形式存储了响应主体，即服务器的响应数据。



**示例**:  响应HTML文本或JSONN数据

```javascript
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        // 当响应成功，获取响应数据,将数据赋值给本地
        oView.innerHTML = xhr.responseText;
        // 如果相应的数据为JSON,就使用JSON.parse转换响应数据        
        //var res = JSON.parse(xhr.responseText);
    }
};
xhr.open("GET", "/statics/demosource/demo_get.php");xhr.send();
```



实际上，响应主体有时还可以从`XMLHttpRequest`对象的`response`、`responseXML`属性获取，它们的使用相对不频繁。

- responseText：无论返回的数据类型是什么，响应主体的内容都会保存在`responseText`属性中；
- responseXML：只对 XML 数据有效，若响应主体是非 XML 数据，该属性值为`null`；
- response：通常配合`responseType`使用。若指定了`XMLHttpRequest`实例的`responseType`属性，则将响应内容转换为该属性所指定的格式并返回，否则按默认情况处理。

**示例**:  response 与 responseType 的使用

```javascript
var xhr = new XMLHttpRequest();
xhr.responseType = "json";// 指定响应主体的数据格式为 json
xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        oTime.innerText = xhr.response.date;
    }
};
xhr.open("GET", "/statics/demosource/demo_get_json.php");
xhr.send();
```

**值得注意的是**:  若指定了`responseType`的值为非 "text" 或 非空（如示例3），则`responseText`属性就会失效，因此时的响应主体已不再是 "text" 文本形式，继续使用它获取响应主体浏览器会给出相应的报错信息.



### 查询 HTTP 响应头

> 在`XMLHttpRequest`对象上，可通过`getAllResponseHeaders`和`getResponseHeader`方法查询响应头信息。

* `getAllResponseHeaders`方法返回可查询的全部响应头信息

* `getResponseHeader(headerName)`方法查询单一响应头信息,需要传入一个指定头名称的参数字符串

```javascript
xhr.getAllResponseHeaders();//返回全部响应头信息
xhr.getResponseHeader("Content-Type");//返回指定名称的单个响应头信息
```

**说明**:  由于`XMLHttpRequest`会自动处理 cookie，将 cookie 从`getAllResponseHeaders`方法返回的响应头集合中过滤掉，并且如果给`getResponseHeader`方法传递 "Set-Cookie" 或 "Set-Cookie2"，则返回 null。



### Ajax 的同步响应

`XMLHttpRequest`对象的`open`方法的第三个参数用来规定脚本是否以 异步方式 调用 Ajax 请求；

准确地说，这个参数用来规定 脚本是否以异步方式 **处理 HTTP 响应**。

> 由于 HTTP 请求的性质，异步处理 HTTP 响应是最好的方式,但如果将`false`作为第三个参数传递给`open`方法，那么调用`send`方法将 阻塞 后续脚本的执行 **直到 HTTP 请求完成**。此时不再需要监听`readystatechange`事件，因为`send`方法后面的代码一定会等到 HTTP 请求完成后再执行。

**应该避免让`XMLHttpRequest`实现同步请求.因为 JavaScript 脚本是单线程的，当`send`方法阻塞脚本时，往往会导致整个页面被冻结.如果连接的服务器响应慢、网速慢等，那么用户的操作将受到影响.**



### abort 中止请求

> 若 HTTP 请求的时间超出预期，可以调用`XMLHttpRequest`对象上的`abort`方法来中止 HTTP 请求。

**示例**:  实现超时

```javascript
var xhr = new XMLHttpRequest();
var timer = null;    // 用于存储定时器标识

xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        clearTimeout(timer);    // 未超时则取消定时器
    }
};

xhr.open("GET", "/statics/demosource/demo_get_json.php");
xhr.send();

// 2秒后中止此次 GET 请求
timer = setTimeout(function(){
    xhr.abort();
}, 2000)
```


## GET请求
### GET 请求的数据传递

> GET 请求一般用于信息获取，它没有请求主体，而是使用 URL 传递参数（即：传递数据给后台）。

GET 传递参数的方式，分两步：

1. 对所需发送的数据(具有名称和值)执行普通的 URL 编码并用 "&" 拼接,如 "name=tom&age=18"；
2. 由于参数附加在 URL 地址后，因此最前面需添加 "?",表示 URL 的 查询参数 开始.


示例：传递表单数据

```javascript
// GET请求的后端接口
var url = "/statics/demosource/demo_get_json.php";
// 获取用户输入的表单数据
var country = document.getElementById("country").value,
    city = document.getElementById("city").value;
// 调整参数格式并加入URL尾
var queryURL = url + "?" + "country=" + country + "&city=" + city;
// 发起GET请求
ajaxGet(queryURL);
```

> 因 JavaScript 对象数据是键值对的形式，因此在 Ajax 的应用中，传递的数据通常直接来自一个 JavaScript 对象，这时只需遍历这个 JavaScript 对象，将每一个键值对按 "名称=值" 的形式进行拼接即可。



### GET 请求的缓存问题

>  GET 请求的结果会被浏览器缓存.这时，如果 GET 请求的 URL 不变，那么请求的结果就是浏览器的缓存

**解决方法**:  实时改变 GET 请求的 URL，只要 URL 不同，就不会取到浏览器的缓存结果。

**具体做法**:  在 URL 末尾添加时间戳参数,达到实时改变请求 URL 的目的.

```javascript
// 在请求参数的最后附加精确到毫秒值时间戳参数:
var queryURL = "/statics/demosource/demo_get_json.php" + "?" + "t=" + new Date().getTime();
```



## POST请求

> POST 请求一般用于修改服务器上的资源，它需要发送一个请求主体，客户端传递给服务器的数据就包含在这个请求主体中。

`"Content-Type"`请求头用于设置请求主体的编码格式。

### 表单编码的 POST 请求

1. 对所需发送的数据（具有名称和值）执行普通的 URL 编码，即：像 GET 请求那样拼接为 名/值 对的形式；
2. 将`"Content-Type"`请求头的值设置为`"application/x-www-form-urlencoded"`。

**示例：**

```javascript
// 获取用户输入的表单数据
var country = document.getElementById("country").value,
    city = document.getElementById("city").value;
// 将数据拼接为 名/值对 的形式
var query = "country=" + country + "&city=" + city;
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    // ... ... 省略事件处理程序
}
// 指定 POST 请求
xhr.open("POST", "/statics/demosource/demo_post_json.php");
// 设置请求主体的编码方法
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
// 发送请求主体（数据）
xhr.send(query);
```



### JSON 编码的 POST 请求

> 使用`JSON.stringify`原生 API 即可实现 JSON 编码，比表单编码更加快捷。

关键步骤：

1. `"Content-Type"`请求头的值需要为`"application/json"`；
2. 对请求主体进行序列化，在 JavaScript 中可使用`JSON.stringify`完成这步操作。

```javascript
// 获取用户输入的表单数据
var country = document.getElementById("country").value,
    city = document.getElementById("city").value;
// 将数据转换为 JavaScript 对象
var data = {
    country : country,
    city : city
}
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    // ... ... 省略事件处理程序
}
// 指定 POST 请求
xhr.open("POST", "/statics/demosource/demo_json_data.php");
// 设置请求主体的编码方法
xhr.setRequestHeader("Content-Type", "application/json");
// 编码请求主体并发送
xhr.send(JSON.stringify(data));
```



## GET与POST的对比

**GET 请求：**

1. 一般用于信息获取：通过发送一个请求来取得服务器上的资源；
2. 数据包含在 URL 地址中；
3. 数据量受 URL 的长度限制；
4. 不安全：浏览器的 URL 可以直接看到，明文传输；
5. GET 请求会被缓存；
6. GET 没有请求主体，请求速度相对较快。

**POST 请求：**

1. 一般用于修改服务器上的资源：向指定资源提交数据，后端处理请求后往往会导致服务器 建立新的资源 或 修改已有资源；
2. 数据包含在请求主体中；
3. 没有数据量限制，可在服务器的配置里进行限制；
4. 只是比 GET 安全，实际上也是不安全的：可通过开发者工具或者抓包看到，明文传输；
5. POST 请求不会被缓存；
6. POST 相对稳定、可靠：可发送包含未知字符的内容。

> HTTP 协议里并没有限制 GET 和 POST 的长度，GET 的最大长度限制是因为浏览器和 Web 服务器对 URL 的长度限制，不同的浏览器和 Web 服务器限制的最大长度不一样，它们所限制的是整个 URL 的长度，而不仅仅是查询参数的数据长度。



## jQuery 中的 Ajax

> jQuery 是一个 JavaScript 工具库，它封装了 JavaScript 常用的功能代码，包括我们刚刚学完的 Ajax。


使用原生 JavaScript 代码实现的 Ajax 请求 繁琐且重复度高. jQuery 中的 Ajax封装度高,且提供了多种接口,方便发送 Ajax 请求.

**jQuery 中，Ajax 常见的请求方式有以下几种：**

- `$.ajax(url, options)`
- `$.get(url, data, callback, dataType)`
- `$.post(url, data, callback, dataType)`
- `$.getJSON(url, data, callback)`
- `$.getScript(url, callback)`
- jQuery元素`.load(url, data, callback)`

示例：
```javascript
// 使用jQuery发起ajax请求
$.ajax("/statics/demosource/demo_get_json.php", {
    //请求类型
    type: "GET",
    //要发送的数据
    data: {
        country: country,
        city: city
    },
    //数据格式
    dataType: "json",
    //请求成功后执行(res为响应成功返回的数据).
    success: function (res) {
        oIpt_country.innerText = res.params.country;
        oIpt_city.innerText = res.params.city;
    },
    //请求失败后执行(这里的res为响应失败返回的数据)
    error: function (res) {
        alert("请求失败：" + res.status);
    }
});//AJAX结束
```



## fetch

> Fetch API 是随 ES6 发展而出现的一个 JavaScript 原生接口，与 Ajax 一样允许开发者异步发起 HTTP 请求，但却以更加简单明了的调用方式、基于 Promise 的数据处理方式被称作是 Ajax 的替代品。



**示例：**

```javascript
fetch("/statics/demosource/demo_json_data.php", {
    method: "POST",
    header: new Headers({"Content-Type" : "application/json"}),
    body: JSON.stringify(data)
})
.then(function (res) {
    return res.ok ? res.json() : Promise.reject(res);})
.then(function (data) {
    oIpt_country.innerText = data.country;
    oIpt_city.innerText = data.city;
})
.catch(function (res) {
    alert("请求失败：" + res.status);
})
```

Fetch 与 Promise 都是 ES6 提出的 API,在不支持的浏览器中使用就需要依次引入 es6-promise与 Fetch Polyfill

> `XMLHttpRequest`对象其实就是 HTTP 规范在客户端 JavaScript 中的实现，一次 HTTP 请求，就对应着一个`XMLHttpRequest`实例，在这个实例上，你可以取到 HTTP 协议中规定的各种协议属性.



## JSON简介



### JSON 是什么

> JSON = JavaScript Object Notation，意思是：JavaScript 对象表示法，是一种轻量级的数据交换格式。

在 JSON 出现之前,用 XML在网络上交换数据通常要加 DTD、XSD、XPath、XSLT 等复杂规范, 过于繁琐、冗长.

为解决这个问题，出现了 JSON 这种轻量级的数据交换格式.与 XML 相比,它数据结构更清晰,易于阅读和编写,同时也易于机器解析和生成，有效地提升了网络传输数据的效率.



### JSON 与 JavaScript 的关系

JSON 是 JavaScript 的子集，它利用了 JavaScript 中的一些模式来表示结构化数据，是在 JavaScript 中读写结构化数据的更好方式。 

**JSON是一种独立于编程语言的数据格式**,并不是只有 JavaScript 才能使用 JSON,很多编程语言都有针对 JSON 的解析器和序列化器.



### JSON 语法规则

JSON 的语法可以表示以下三种类型的值：

- **简单值**:  使用与 JavaScript 相同的语法，可在 JSON 中表示`number`、`string`、`boolean` 与 `null`，但不支持`undefined`；
- **对象**:  对象作为一种复杂数据类型， 表示的是一组无序的键值对，而每个键值对中的值可以是简单值，也可以是复杂数据类型的值；
- **数组**:  数组也是一种复杂数据类型，表示一组有序的值的列表，数组的值也可以是任意类型 —— 简单值、对象 或 数组。

**JSON 字符串必须使用 双引号.单引号会导致语法错误.**

在实际应用中JSON 更多地用来表示更复杂的数据结构.



## JSON 对象

**JSON 表示对象**：属性值可以是简单值，也可以是复杂类型值

**与 JavaScript 的字面量相比，JSON 对象的属性名必须加双引号**

```json
{
    "name" : "Alan",
    "age" : 29,
    "child" : {
        "name" : "Tim",
        "age" : 7    
    }
}
```



### JSON 数组

在 JSON 中，可以采用与JavaScript同样的语法来表示数组

把数组和对象结合起来，可以构成更复杂的数据集合

```javascript
[
    {
        "title" : "Professional JavaScript",
        "authors" : ["Nicholas C. Zakas"],
        "year" : 2011
    },
    {
        "title" : "Professional Ajax",         
        "authors" : [
            "Nicholas C. Zakas",
            "Jeremy McPeak",
            "Joe Fawcett"],
        year: 2008
    }
]
```



### JavaScript 内置的 JSON 对象

> ECMAScript 5 定义了一个原生的 JSON 对象，可把 JavaScript 对象序列化为 JSON 字符串，或把 JSON 字符串解析为原生的 JavaScript 值。

**常用方法：**

* `JSON.stringify()`： JS对象   ---序列化--->	JSON 字符串；
* `JSON.parse()`：         JSON字符串	-----解析---->	JS对象。



