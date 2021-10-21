## 传统Ajax

Asynchronous JavaScript and XML，利用JavaScript 执行异步网络请求 ，而不需要重新刷新页面。

隶属于原生JS，核心使用 **XMLHttpRequest** 对象，多个请求之间如果有先后关系的话，就会出现回调地狱。（回调地狱？？反正很惨就是了）

Ajax使用XMLHttpRequest对象取得新数据，然后再通过 DOM 将新数据插入到页面中。

外，虽然名字中包含 XML 的成分，但 Ajax 通信与数据格式无关; 这种技术就是无须刷新页面即可从服务器取得数据，但不一定是 XML 数据。（也可以是JSON格式等）

对于IE7+和其他浏览器，可以直接使用XMLHttpRequest对象，对于IE6及以前的浏览器，使用ActiveXObject对象。

根据浏览器支持情况兼容：

```javascript
var xhr;
if (window.XMLHttpRequest) {
    xhr = new XMLHttpRequest();
} else {
    xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
```

**启动请求：**

```javascript
xhr.open(method, url, boolean);//请求方式、请求url、是否异步请求，默认true是异步     
xhr.send();
```

XHR异步请求案例：

```javascript
function success(text) {
    console.log(text);
}

function fail(code) {
   console.log(code);
}

var xhr = new XMLHttpRequest();     // 新建XMLHttpRequest对象
xhr.onreadystatechange = function () { 
    // 状态发生变化时，函数被回调
    if (xhr.readyState === 4) { // 成功完成
        // 判断响应结果:
        if (xhr.status === 200) {
            // 成功，通过responseText拿到响应的文本:
            return success(xhr.responseText);
        } else {
            // 失败，根据响应码判断失败原因:
            return fail(xhr.status);
        }
    } else {
        // HTTP请求还在继续...
    }
}

// 发送请求:
xhr.open('get', '/api/categories');
xhr.send(null);
```









## JQuery Ajax

是对原生XHR的封装，还增添了对JSONP的支持。是针对MVC的编程。

**缺点：**

是基于XHR原生开发的，目前已有的fetch可替代。本身是针对mvc的编程模式，不太适合目前mvvm的编程模式。jQuery本身比较大，如果单纯的使用ajax可以自己封装一个，不然会影响性能体验。

（后面是不是也要记一下 MVC 和MVVM）

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210514093629023.png" alt="image-20210514093629023" style="zoom:50%;" />



```javascript
$.ajax({
        url:"",
        type:"GET",
        contentType: '',
        async:true,
        data:{},
        dataType:"",
        success: function(){
        }
});
```



## axios

axios是一款基于XHR对象封装的网络请求工具，同时支持浏览器和Nodejs，继承了Promise。

浏览器中是基于XHR；Node中是基于http模块。

它本身具有以下特征：
 1.从浏览器中创建 XMLHttpRequest
 2.支持 Promise API
 3.客户端支持防止CSRF
 4.提供了一些并发请求的接口（重要，方便了很多的操作）
 5.从 node.js 创建 http 请求
 6.拦截请求和响应
 7.转换请求和响应数据
 8.取消请求
 9.自动转换JSON数据

**安装和使用**

```javascript
//安装
$ npm install axios

//使用
axios.get(url,[options]).then().catch()
axios.post(url,[body],[options]).then().catch()
...
axios(options).then().catch()
```



**axios响应对象**

```javascript
{
    //此属性,才是接口的响应,我们需要的数据
    data:{},
    //响应的状态码
    status:200,
    //服务器响应信息
    statusText:'ok',
    //服务器响应的头
    headers:{},
    //为请求提供的配置信息
    config:{}   
}
```



**axios请求对象**

```javascript
{
  // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // 默认是 get

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // 默认的

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

  // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // 默认的

  // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认的

  // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认的
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: : {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}

```







```javascript
// 第一种写法
axios.get('/user?id=12345&name=xiaoming')
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});

// 第二种写法
axios.get('/user', {
    params: {
      id: '12345'，
      name: 'xiaoming'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});

// 第三种写法
axios({
    url: '/user',
    method: 'get',
    params: {
      id: '12345'，
      name: 'xiaoming'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});

```



执行多个并发请求

```javascript
function getUserAccount() {
  return axios.get('/user/12345');
}
 
function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}
 
axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
}));
```



**拦截器**

在请求发出之前或响应被 then 或 catch 处理前拦截它们做预处理。

```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
  }, function (error) {
    // 对请求错误做些什么
  });
 
// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
  }, function (error) {
    // 对响应错误做点什么
  });

```



**axios在Vue中全局使用**

```javascript
//将axios挂载到Vue的原型上即可

//main.js
import Vue from 'vue';
import axios from 'axios'
Vue.prototype.$axios=axios;

//我在项目中一般喜欢把接口都封装在apis文件中之后,然后全局调用接口
//方法都是可以,看个人情况
//引入api,将api挂载到vue原型,方便调用
import http from '@/utils/apis'
Vue.prototype.$http = http
```

**封装一个axios**

```javascript
//fetch.js

//引入axios 
import axios from 'axios';

//创建一个axios的实例对象
var instance = axios.create({
    //baseUrl 自己按实际情况配置
    baseUrl: "http://localhost:8080",
    //设置超时时间 ms
    timeout: 7000,
    headers: {
        'Content-Type': 'application/json;charset=UTF-8'
    }
});

// 封装请求拦截器
instance.interceptors.request.use((config) => {
    // 在这里做一些检测或者包装等处理
    // console.log('请求拦截', config)
    // 鉴权 token添加
    config.headers.Authorization = localStorage.getItem('token') || ''
    return config
})

// 封装响应拦截器
instance.interceptors.response.use((response) => {
    //按自己实际情况配置
    // 请求成功
    // console.log('响应拦截', response)
    // 数据过滤，根据后端标识字符来进行数据
    if (response.data && response.data.err == 0) {
        return response.data.data
    } 
}, (error) => {
    // 请求失败
    return Promise.reject(error)
})

// 暴露axios实例
export default instance;


```

原文： https://blog.csdn.net/liuqiao0327/article/details/106894445







<img src="https://pic4.zhimg.com/80/v2-1169b286580351161459e46d9df84def_1440w.jpg" alt="img" style="zoom: 33%;" />





**现在无脑使用axios即可，Jquery老迈笨拙，fetch年轻稚嫩，只有Axios正当其年！**

哈哈 无脑接受观点，具体的咱也不知道，咱现在还学不了那么深入





## fetch

ES6提供的，一个底层的请求API，用来替代XHR。

不需要安装，可以当作全局变量来使用，它是挂在window对象上的。

但是存在兼容性问题，建议安装一个 Polyfill

```javascript
//使用
fetch(url).then(response=>response.json()).then().catch();
fetch(url).then(response=>response.text()).then().catch();
```

Fetch是基于promise设计的，支持async/await

fetch号称是AJAX的替代品，是在ES6出现的，使用了ES6中的promise对象.

fetch 属于原始JS，代码结构也比较简单

更加底层，提供的API丰富？

跨域处理？

fetch目前存在很多问题？？？





**使用 Fetch**

Fetch API 提供了一个 JavaScript 接口，用于访问和操纵 HTTP 管道的一些具体部分，如请求和响应。它还提供了一个全局 [`fetch()`](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/fetch) 方法，该方法提供了一种简单，合理的方式来**跨网络异步获取资源**。

```javascript
//最简单的请求
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```





```javascript
//使用定义的函数，将参数 url 和 data 传入
postData('http://example.com/answer', {answer: 42})
  .then(data => console.log(data)) // JSON from `response.json()` call
  .catch(error => console.error(error))

//定义一个函数，里面存放了一个使用fetch发送请求的模板
function postData(url, data) {
  // Default options are marked with *
  return fetch(url, {
    body: JSON.stringify(data), // 必须要和header里的 'Content-Type' 保持一致
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, *omit
    headers: {
      'user-agent': 'Mozilla/4.0 MDN Example',
      'content-type': 'application/json'
    },
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, cors, *same-origin
    redirect: 'follow', // manual, *follow, error
    referrer: 'no-referrer', // *client, no-referrer
  })
  .then(response => response.json()) // parses response to JSON
}
```



使用fetch() 上传JSON数据

```javascript
var url = 'https://example.com/profile';
var data = {username: 'example'};

fetch(url, {
  method: 'POST', // or 'PUT'
  body: JSON.stringify(data), // data can be `string` or {object}!
  headers: new Headers({
    'Content-Type': 'application/json'
  })
}).then(res => res.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```





上传文件

```javascript
var formData = new FormData();
var fileField = document.querySelector("input[type='file']");

formData.append('username', 'abc123');
formData.append('avatar', fileField.files[0]);

fetch('https://example.com/profile/avatar', {
  method: 'PUT',
  body: formData
})
.then(response => response.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```









## 各个流行框架中的网络请求工具

### Angularjs

使用内置的http模块

### Vuejs

使用Vue Resource、fetch、axios

#### Vue Resource

Vue1.x内置的插件，和axios相似；Vue2.x时官方不再维护，推荐使用axios，但还可以继续用。

安装、使用

```javascript
//安装 在终端：
$ npm install vue-resource


在main.js文件中
//引入vue
import Vue from 'vue'
//引入vue-resource
import VueResource from 'vue-resource'

//使用
Vue.http或者this.$http
```

常用API

```javascript
this.$http.get(url,[options]).then().catch();
this.$http.post(url,[body],options).then().catch();
this.$http(options).then().catch();
```



### react

使用fetch、axios

