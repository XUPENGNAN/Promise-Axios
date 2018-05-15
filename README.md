# Promise-Axios
### 异步操作：
#### 1. Javascript语言的执行环境是"单线程"（single thread)，所谓"单线程"，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务。这种模式的好处是实现起来比较简单，执行环境相对单纯；坏处是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。为了解决这个问题，Javascript语言将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）
#### 2. "同步模式"就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；"异步模式"则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。"异步模式"非常重要。在浏览器端，耗时很长的操作都应该异步执行，避免浏览器失去响应，最好的例子就是Ajax操作。在服务器端，"异步模式"甚至是唯一的模式，因为执行环境是单线程的，如果允许同步执行所有http请求，服务器性能会急剧下降，很快就会失去响应.同步时阻塞的,异步是非阻塞的。
#### 在没有Promise对象时，JS执行异步操作使用的是回调函数，但是回调函数的嵌套会降低代码的可读性。
```
//下式的执行顺序是:hello,xp,函数并非从上到下执行的，而是先执行 hello，再执行 xp
function say(one) {
    one();
}
say(hi);
console.log("xp");

function hi() {
    console.log("hello!");
}
```
### Promise 对象：
#### 是一个容器，是一个对象，它可以获取异步操作的消息。异步编程的一种解决方案，它有三种状态，分别是pending-进行中、resolved-已完成、rejected-已失败。Promise 在一经创建就会马上执行
#### Promise 的构造函数接收一个函数，并且传入两个参数：resolve，reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。resolve是将Promise的状态置为resolved-已完成，reject是将Promise的状态置为rejected-已失败
#### Promise.prototype.then();异步函数接受参数为2个回调函数，第一位已完成的回调函数，第二个为已失败的回调函数。
```
//执行顺序 "hello" , "111"
let one = new Promise((resolve, reject)=>{
        resolve();
}).then(()=>{
    console.log("111");
}.catch(){
    console.log("222");
})
        
console.log("hello");
```

```
//执行顺序 "hello" , "222"
let one = new Promise((resolve, reject)=>{
        reject();
}).then(()=>{
    console.log("111");
}.catch(){
    console.log("222");
})
        
console.log("hello");
```

#### Promise.prototype.catch();用于指定发生错误时的回调函数
```
//执行顺序 "hello" , "mnm"
let one = new Promise((resolve, reject)=>{
        reject();
}).catch(()=>{
    console.log("mnm");
})
        
console.log("hello");
```
#### Promise.prototype.finally();方法用于指定不管 Promise 对象最后状态如何，都会执行的操作
```
//执行顺序 "hello" "111" "mnm"
let one = new Promise((resolve, reject)=>{
        reject();
}).catch(()=>{
    console.log("111);
}).finally(()=>{
    console.log("mnm");
})
        
console.log("hello");
```
#### Promise.all();接受一个数组，该数组都是Promise实例，
#### 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数
#### 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数
```
//['hello','报错了']
const p1 = new Promise((resolve, reject) => {
    resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
    resolve('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then((a)=>{
    console.log(a);
})
.catch(e => console.log(e));
```
#### Promise.race();是将多个 Promise 实例，包装成一个新的 Promise 实例,只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
#### Promise.resolve();将一个实例包装为Promise对象，并且这个对象的状态是 resolved 已完成。
```
let aaa = Promise.resolve();
    aaa.then(()=>{
        console.log("11111");  
});

let bbb = Promise.resolve('foo');
    bbb.then((a)=>{
        console.log(a);
})
```
#### Promise.reject();返回一个新的 Promise 实例，该实例的状态为rejected

### Axios 
#### 特点：
##### 浏览器端发起XMLHttpRequests请求
##### node端发起http请求
##### 支持Promise API
##### 监听请求和返回
##### 转化请求和返回
##### 取消请求
##### 自动转化json数据
##### 客户端支持抵御

#### axios.get(); 
```
//发起一个user请求，参数为给定的ID
axios.get('/user?ID=1234')
.then(function(respone){
    console.log(response);
})
.catch(function(error){
    console.log(error);
});
```

```
axios.get('/user',{
    params:{
        ID:12345
    }
})
.then(function(response){
    console.log(response);
})
.catch(function(error){
    console.log(error)
});
```
#### axios.post();
```
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
#### 执行多个并发请求
```
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
#### options
```
axios({
    url:'',
    methods:'post',                                        //请求方式,默认使用 get 方法
    baseURL: 'https://some-domain.com/api/',               // `baseURL` 将自动加在 `url` 前面
    headers: {'X-Requested-With': 'XMLHttpRequest'},       // `headers` 是即将被发送的自定义请求头
    params: { ID: 12345 },                                 // `params` 是即将与请求一起发送的 URL 参数
    paramsSerializer: function(params) {                   // `paramsSerializer` 是一个负责 `params` 序列化的函数
    return Qs.stringify(params, {arrayFormat: 'brackets'})
    },
    data: {                                                // `data` 是作为请求主体被发送的数据
                                                           // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
    firstName: 'Fred'
    },
    transformRequest: [function (data) {                   //`transformRequest` 允许在向服务器发送前，修改请求数据
    // 对 data 进行任意转换处理
    return data;
    }],
    timeout: 1000,                                         //请求超时设置
    withCredentials: false,                                // 默认的，`withCredentials` 表示跨域请求时是否需要使用凭证
                                                           // 跨域的 CORS 存在以下三种主要场景，分别是 简单请求，预检请求和附带身份凭证的请求 。
    auth: {                                                // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
                                                           // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义  // `Authorization`头
    username: 'janedoe',
    password: 's00pers3cret'

    },
    responseType: 'json',                                  // 服务器的相应数据类型，默认的是Json
    xsrfCookieName: 'XSRF-TOKEN',                          // default
})
```
#### 请求返回的内容，也就是请求
```
{
  data:{},
  status:200,   //从服务器返回的http状态文本
  statusText:'OK',
  //响应头信息
  headers: {},
  //`config`是在请求的时候的一些配置信息
  config: {}
}
```
#### 配置的默认值/defaults
```
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

#### 创建实例并配置实例的默认值
```
// 创建实例时设置配置的默认值
var instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 在实例已创建后修改默认值
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;

```
#### 拦截器:在请求或响应被 then 或 catch 处理前拦截它们
```
// 添加请求拦截器,参数config是请求的配置项
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
