# axios知识点及vue的使用axios方式

### 简介

 Axios 是一个基于 **Promise** 的 HTTP 库，可以用在浏览器和 node.js 中 ，它提供了一个更强大、更灵活的功能集。 

### 特性

- 在浏览器中创建 XMLHttpRequests 
- 在 node.js 则创建 http 请求
- 支持 **Promise** API
- 支持拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF

### 浏览器支持

 支持Chrome、火狐、Edge、IE8+等浏览器 

### 安装

 使用 npm安装: 

```ini
$ npm install axios
```

 使用 bower: 

```ini
$ bower install axios
```

 或者直接使用 cdn: 

```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

### 使用举例

 执行 `GET` 请求  注：推荐使用啥方法详情

```javascript
// 为给定 ID 的 user 创建请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// GET 参数可以放到params里（推荐） get使用params参数内容
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 还可以使用ECMAScript 2017里的async/await，添加 `async` keyword to your outer function/method.
//使用async/await使用详情
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

>  async/await 是 ECMAScript 2017新提供的功能 ，Internet Explorer 和一些旧的浏览器并不支持 

 执行 `POST` 请求 

```javascript
//post使用的是对象，这里也要注意
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

 执行多个并发请求 axios.all

```javascript
//注意axios.all的使用流程等
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

#### axios API

 可以通过向 `axios` 传递相关配置来创建请求 。**请求图片要考虑下为什么，是什么原因？**

##### axios(config)

- ```javascript
  // 发送 POST 请求
  axios({
    method: 'post',
    url: '/user/12345',
    data: {
      firstName: 'Fred',
      lastName: 'Flintstone'
    }
  });
  //  GET 请求远程图片 response参数
  axios({
    method:'get',
    url:'http://bit.ly/2mTM3nY',
    responseType:'stream'
  })
    .then(function(response) {
    response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
  });
  ```

##### axios(url[, config])

```javascript
// 发送 GET 请求（默认的方法）
axios('/user/12345');
```

#### 请求方法的别名

 为方便使用，官方为所有支持的请求方法提供了别名，可以直接使用别名来发起请求： 

- **axios.request(config)**
- **axios.get(url[, config])**
- **axios.delete(url[, config])**
- **axios.head(url[, config])**
- **axios.post(url[, data[, config]])**
- **axios.put(url[, data[, config]])**
- **axios.patch(url[, data[, config]])**

### 并发

处理并发请求的助手函数

- **axios.all(iterable)**
- **axios.spread(callback)**

### 创建实例

 可以使用自定义配置新建一个 axios 实例 

#### axios.create([config])

```javascript
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

#### 实例方法

 以下是可用的实例方法。指定的配置将与实例的配置合并 

- **axios#request(config)**
- **axios#get(url[, config])**
- **axios#delete(url[, config])**
- **axios#head(url[, config])**
- **axios#post(url[, data[, config]])**
- **axios#put(url[, data[, config]])**
- **axios#patch(url[, data[, config]])**

#### 请求配置项

 下面是创建请求时可用的配置选项，注意只有 `url` 是必需的。如果没有指定 `method`，请求将默认使用 `get` 方法。 

```javascript
{
  // `url` 是用于请求的服务器 URL
  url: "/user",

  // `method` 是创建请求时使用的方法
  method: "get", // 默认是 get

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: "https://some-domain.com/api/",

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 "PUT", "POST" 和 "PATCH" 这几个请求方法
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
  headers: {"X-Requested-With": "XMLHttpRequest"},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: "brackets"})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 "PUT", "POST", 和 "PATCH"
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: "Fred"
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
    username: "janedoe",
    password: "s00pers3cret"
  },

  // `responseType` 表示服务器响应的数据类型，可以是 "arraybuffer", "blob", "document", "json", "text", "stream"
  responseType: "json", // 默认的

  // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: "XSRF-TOKEN", // default

  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: "X-XSRF-TOKEN", // 默认的

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
    return status &gt;= 200 &amp;&amp; status &lt; 300; // 默认的
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // "proxy" 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: "127.0.0.1",
    port: 9000,
    auth: : {
      username: "mikeymike",
      password: "rapunz3l"
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

#### 响应结构

 axios请求的响应包含以下信息： 

```javascript
{
  // `data` 由服务器提供的响应
  data: {},

  // `status`  HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: "OK",

  // `headers` 服务器响应的头
  headers: {},

  // `config` 是为请求提供的配置信息
  config: {}
}
```

 使用 `then` 时，会接收下面这样的响应： 

```javascript
axios.get("/user/12345")
  .then(function(response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

 在使用 **catch** 时，或传递 **rejection callback** 作为 **then** 的第二个参数时，响应可以通过 **error** 对象可被使用，正如在错误处理这一节所讲。 

#### 配置的默认值/defaults

 你可以指定将被用在各个请求的配置默认值 

#### 全局的 axios 默认值

```javascript
axios.defaults.baseURL = "https://api.example.com";
axios.defaults.headers.common["Authorization"] = AUTH_TOKEN;
axios.defaults.headers.post["Content-Type"] = "application/x-www-form-urlencoded";
```

#### 自定义实例默认值

```javascript
// 创建实例时设置配置的默认值
var instance = axios.create({
  baseURL: "https://api.example.com"
});

// 在实例已创建后修改默认值
instance.defaults.headers.common["Authorization"] = AUTH_TOKEN;
```

#### 配置的优先级

配置会以一个优先顺序进行合并。

请求的config > 实例的 `defaults` 属性 > 库默认值：

```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

 如果你想在稍后移除拦截器，可以这样： 

```javascript
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

 可以为自定义 axios 实例添加拦截器 

```javascript
var instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

#### 错误处理

```javascript
axios.get("/user/12345")
  .catch(function (error) {
    if (error.response) {
      // 请求已发出，但服务器响应的状态码不在 2xx 范围内
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log("Error", error.message);
    }
    console.log(error.config);
  });
```

 可以使用 `validateStatus` 配置选项定义一个自定义 HTTP 状态码的错误范围 

```javascript
axios.get("/user/12345", {
  validateStatus: function (status) {
    return status < 500; // 状态码在大于或等于500时才会 reject
  }
})
```

#### 取消请求

 使用 *cancel token* 取消请求 

>  Axios 的 cancel token API 基于cancelable promises proposal 

 可以使用 `CancelToken.source` 工厂方法创建 cancel token，像这样： 

```javascript
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // handle error
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// cancel the request (the message parameter is optional)
source.cancel('Operation canceled by the user.');
```

 还可以通过传递一个 executor 函数到 `CancelToken` 的构造函数来创建 cancel token： 

```javascript
var CancelToken = axios.CancelToken;
var cancel;

axios.get("/user/12345", {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```

>  可以使用同一个 cancel token 取消多个请求 



#### 请求时使用 application/x-www-form-urlencoded

 axios会默认序列化 JavaScript 对象为 JSON. 如果想使用 application/x-www-form-urlencoded 格式，你可以使用下面的配置. 

#### 浏览器

 在浏览器环境，你可以使用 URLSearchParams API :

```javascript
const params = new URLSearchParams();
params.append('param1', 'value1');
params.append('param2', 'value2');
axios.post('/foo', params);
```

>  URLSearchParams不是所有的浏览器均支持 

 除此之外，你可以使用qs库来编码数据: 

```javascript
const qs = require('qs');
axios.post('/foo', qs.stringify({ 'bar': 123 }));

// Or in another way (ES6),

import qs from 'qs';
const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```

 当然，同浏览器一样，你还可以使用 qs library. 

#### Promises

 axios 依赖原生的 ES6 Promise 实现而被支持.
如果你的环境不支持 ES6 Promise，你可以使用 polyfill. 

#### TypeScript支持

 axios 包含 TypeScript definitions. 

```javascript
import axios from "axios";
axios.get("/user?ID=12345");
```



### vue中使用axios的多种方式

 安装其他插件的时候，可以直接在 main.js 中引入并 Vue.use()，但是 axios 并不能 use，只能每个需要发送请求的组件中即时引入 

 为了解决这个问题，有两种开发思路，一是在引入 axios 之后，修改原型链，二是结合 Vuex，封装一个 aciton 

使用npm

npm install axios

使用cdn:

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
#### 解决post方法使用application/x-www-form-urlencoded格式编码数据

1.  设置 headers:{ 'Content-type': 'application/x-www-form-urlencoded'} 

   ```javascript
   axios.post('url',data,{headers:{ 'Content-type': 'application/x-www-form-urlencoded'}})
   
   // 不想在每次请求都设置的话，可以集中设置下
   axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded; charset=UTF-8';
   ```

   

2.  仅仅这样并没有达到想要的效果，post的body主体中还是{"age":10}这样的格
   式，并不是我们想要的query参数。引入Qs，这个库是axios里面包含的，不需要再下载了 

   ```javascript
   import qs from 'qs'
   var data = qs.stringify({"name":"xie"});
   axios.post('url',data).then()
   ```



#### axios默认是不让ajax请求头部携带cookie的

#### axios 解决跨域cookie丢失问题

 设置 axios.defaults.withCredentials = true 即可 

 示例代码： 

```javascript
axios.defaults.withCredentials = true;
var param = new URLSearchParams();
param.append("vCode",vcode);
axios.post('http://localhost',param)
    .then(function(res) {
    var rs=res.data;
    console.log(rs.data);
})
    .catch(function(err) {
    console.log(err);
});
```

#### 配合vue

> axios并没有install 方法，所以是不能使用vue.use()方法的。
>  那么难道每个文件都要来引用一次？解决方法有很多种：
>
> -  结合 vue-axios使用
> - axios 改写为 Vue 的原型属性
> - 结合 Vuex的action

##### 方法1：结合 vue-axios使用

vue-axios

用于将axios集成到Vuejs的小包装器

github: https://github.com/axios/axios

安装： **npm install --save axios vue-axios**

vue-axios是按照vue插件的方式去写的。那么结合vue-axios，就可以去使用vue.use方法了

```javascript
首先在主入口文件main.js中引用

import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios,axios);
```

 之后就可以使用了，在组件文件中的methods里去使用了 

```javascript
getNewsList(){
      this.axios.get('api/getNewsList').then((response)=>{
        this.newsList=response.data.data;
      }).catch((response)=>{
        console.log(response);
      })
    },
```

##### 方法2： axios 改写为 Vue 的原型属性

首先在主入口文件main.js中引用，之后挂在vue的原型链上

**import axios from 'axios'**
**Vue.prototype.$axios= axios**

 在组件中使用 

```javascript
this.$axios.get('api/getNewsList').then((response)=>{
        this.newsList=response.data.data;
      }).catch((response)=>{
        console.log(response);
      })
```

##### 方法3：结合vuex

```javascript
import axios from 'axios'
import { Message } from 'element-ui'
import store from '@/store'
import { getToken } from '@/utils/auth'

// 创建axios实例
const service = axios.create({
  baseURL: process.env.BASE_API, // api的base_url
  timeout: 5000 // 请求超时时间
})

// request拦截器
service.interceptors.request.use(config => {
  // Do something before request is sent
  if (store.getters.token) {
    config.headers['X-Token'] = getToken() // 让每个请求携带token--['X-Token']为自定义key 请根据实际情况自行修改
  }
  return config
}, error => {
  // Do something with request error
  console.log(error) // for debug
  Promise.reject(error)
})

// respone拦截器
service.interceptors.response.use(
  response => response,
  /**
  * 下面的注释为通过response自定义code来标示请求状态，当code返回如下情况为权限有问题，登出并返回到登录页
  * 如通过xmlhttprequest 状态码标识 逻辑可写在下面error中
  */
  //  const res = response.data;
  //     if (res.code !== 20000) {
  //       Message({
  //         message: res.message,
  //         type: 'error',
  //         duration: 5 * 1000
  //       });
  //       // 50008:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;
  //       if (res.code === 50008 || res.code === 50012 || res.code === 50014) {
  //         MessageBox.confirm('你已被登出，可以取消继续留在该页面，或者重新登录', '确定登出', {
  //           confirmButtonText: '重新登录',
  //           cancelButtonText: '取消',
  //           type: 'warning'
  //         }).then(() => {
  //           store.dispatch('FedLogOut').then(() => {
  //             location.reload();// 为了重新实例化vue-router对象 避免bug
  //           });
  //         })
  //       }
  //       return Promise.reject('error');
  //     } else {
  //       return response.data;
  //     }
  error => {
    console.log('err' + error)// for debug
    Message({
      message: error.message,
      type: 'error',
      duration: 5 * 1000
    })
    return Promise.reject(error)
  })

export default service

```

```javascript
import request from '@/utils/request'

//使用
export function getInfo(params) {
  return request({
    url: '/user/info',
    method: 'get',
    params
  });
}
```

