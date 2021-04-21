



# axios

## 一、axios

1. axios是基于promise的对ajax的一种封装。
2. ajax是基于mvc
3. axios 基于mvvm

## 二、axios入门

### axios的基本使用

1. **axios发送get请求**

```javascript
//使用默认方式发送请求
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.0/axios.min.js"></script>  <script src="index.js"></script>
  <script>
    axios({
      url: 'http://152.136.185.210:8000/api/w6/home/multidata'
    }).then(res=>{
      console.log(res)
    })
  </script>
//默认使用get请求方式
```

```javascript
//指定请求方式为get的无参请求
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.0/axios.min.js"></script>  <script src="index.js"></script>
  <script>
    axios({
      url: 'http://152.136.185.210:8000/api/w6/home/multidata',
      method: 'get' //设置请求方式
    }).then(res=>{
      console.log(res)
    })
  </script>

```

```javascript
//axios发送get方式的有参请求,可以直接在url里面用 ? 连接
 axios({
      url: 'http://152.136.185.210:8000/api/w6/home/multidata?type=pop&page=1'
      method: 'GET' //设置请求方式
      // method: 'post' //设置请求方式
    }).then(res=>{
      console.log(res)
    })
```

```javascript
//axios发送get方式请求其他形式 ，也可以在params里面
 axios({
      url: 'http://152.136.185.210:8000/api/w6/home/multidata',
      method: 'GET', //设置请求方式
      // method: 'post' //设置请求方式
      params: {
        type: 'pop',
        page: '1'
      }
    }).then(res=>{
      console.log(res)
    })
```

2. **axios发送post请求**

```javascript
//使用post方式发
//post没有可用的路径例子暂时，随便意思一下
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.0/axios.min.js"></script>  <script src="index.js"></script>
  <script>
    axios({
      url: 'http://152.136.185.210:8000/api/w6/home/multidata',
      // method: 'GET' //设置请求方式
      method: 'post' //设置请求方式
    }).then(res=>{
      console.log(res)
    })
  </script>
```

```javascript
axios({
      url: '',
      method: 'post'
      params: {
          
      }
    }).then(res=>{
      console.log(res)
    })
```

```javascript
//使用axios发送带有参数的post请求，使用data传递
axios({
      url: '',
      method: 'post'
      data: {
          
      }
    }).then(res=>{
      console.log(res)
    })
//axios使用post携带参数请求使用默认application/json

//解决方式一：params属性进行数据的传递
//解决方式二：'name=张三'
//解决方式三：服务器端给接收的参数加上@requestBody注解
```

### axios的请求方式

```javascript
//使用axios.get方式发送无参请求
axios.get( 'http://152.136.185.210:8000/api/w6/home/multidata').then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log('timeout')
      console.log(err)
    })
```

```javascript
//使用axios.get方式发送有参请求，路径后拼接
axios.get( 'http://152.136.185.210:8000/api/w6/home/multidata?type=pop&page=1').then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log('timeout')
      console.log(err)
    })
```

```javascript
//使用axios.get方式发送有参请求，params
axios.get( 'http://152.136.185.210:8000/api/w6/home/multidata',
      {
        params:{
          type:'pop',
          page: '1'
      }
    }).then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log('timeout')
      console.log(err)
    })
```

```javascript
//使用axios.post方式发送无参请求
 axios.post( 'http://152.136.185.210:8000/api/w6/home').then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log('timeout')
      console.log(err)
    }) 

```

```javascript

//使用axios.post 方式发送有参请求
//axios.post,传参的时候用这个方法，只修改前端代码
    // axios.post( 'http://152.136.185.210:8000/api/w6/home/multidata','type=pop&page=1').then(res=> {
    //   console.log(res)
    // }).catch(err=> {
    //   console.log('timeout')
    //   console.log(err)
    // }) 
//发送post请求携带参数，直接使用"type=pop&page=1"
//解决方式二：'name=张三'
```

```javascript
//使用data传递数据，后台需要将axios自动转换的json转换为为java对象，修改后台代码解决
axios.post,传参的时候用这个方法，
    axios.post( 'http://152.136.185.210:8000/api/w6/home/multidata',{params: {type:"pop",page: "1"}}).then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log('timeout')
      console.log(err)
    }) 
//解决方式三：服务器端给接收的参数加上@requestBody注解
//接收的参数添加@requestBody
```

## 三、axios的并发请求

```javascript
//axios并发请求
    axios.all( [
      axios.get('http://152.136.185.210:8000/api/w6/home/multidata'),
      axios.get('http://152.136.185.210:8000/api/w6/home/multidata?type=pop&page=1')
    ]).then(res=> {
      console.log(res)//输出来是一个数组，根据下标再选择
      console.log(res[0])
      console.log(res[1])
    }).catch(err=> {
      console.log(err)
    })
```

```javascript
//使用spread函数处理axios并发请求
    axios.all( [
      axios.get('http://152.136.185.210:8000/api/w6/home/multidata'),
      axios.get('http://152.136.185.210:8000/api/w6/home/multidata?type=pop&page=1')
    ]).then(
      axios.spread((res1,res2)=> { //axios.spread会自动将数组分割出来
        console.log(res1)
        console.log(res2)
      })
    ).catch(err=> {
      console.log(err)
    })
//
```

## 四、axios的全局配置

```javascript
//axios的全局配置
    axios.defaults.baseURL = 'http://152.136.185.210:8000/api/w6'
    axios.defaults.timeout = 5000
    axios.get('home/multidata').then(res=> {//在全局配置基础上去网络请求
      console.log(res)
    }).catch(err=> {
      console.log(err)
    })

// axios.post(url).then(res=> [
    //   console.log(res)
    // ])
```

## 五、axios的实例

```javascript
//创建axios的实例
    let newAxios = axios.create({
      baseURL: 'http://152.136.185.210:8000/api/w6',
      timeout: 5000
    })
    let newAxios1 = axios.create({
      baseURL: 'http://152.136.185.210:8000/api/w6',
      timeout: 5000 
    })

    let newAxios2 = axios.create({
      baseURL: 'http://152.136.185.210:8000/api/w6',
      timeout: 50  //时间太短，肯定超时
    })
    newAxios({
      url: 'home/multidata'
    }).then(res=> {
      console.log(res)
    })

    newAxios1({
      url: 'home/multidata?type=pop&page=1'
    }).then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log(err)
    })
    newAxios2({
      url: 'home/multidata'
    }).then(res=> {
      console.log(res)
    }).catch(err=> {
      console.log(err)
    })   //超时没有请求到数据
```

## 六、axios拦截器

```
axios给我们提供了两大类拦截器，一种是请求方向的拦截（成功请求，失败的）
另一种是响应方向的（成功的，失败的）
```

```
拦截器的作用：用于在我们网络请求的时候，在发起请求或者响应时对操作进行响应的处理
```

​	   **请求时**

+ 发起请求时可以添加网页加载的动画

+ 使用token认证的时候，可以在请求之前判断用户有没有token，强制登陆

  **响应时**

+ 响应的时候，可以进行响应的数据处理

```javascript
//请求方向拦截器
axios.interceptors.request.use(config=> {
      console.log('进入拦截器')
      console.log(config)
      return config   //拦截完放行请求
    },err => {
      console.log("请求方向失败")
      console.log(err)
    })

    axios.get('http://152.136.185.210:8000/api/w6/home/multidata').then(res=> {
      console.log(res)
    })
```

```javascript
//响应方向拦截器
  axios.interceptors.response.use(config => {
      console.log(config);
      console.log('进入响应拦截器')
      return config.data
    },err => {
      console.log(err)
      console.log('响应方向失败')
    })

    axios.get('http://152.136.185.210:8000/api/w6/home/multidata').then(res=> {
      console.log(res)
    })
```

## 七、axios在vue中的模块封装

1. 第一种方法

```javascript
//封装位置
import axios from 'axios'
//只有一个参数时
export function request(config,success,fail) {
  axios({
    url: config
  }).then(res=> [
    success(res)
  ]).catch(err => {
    fail(err)
  })
}

//调用者位置
import {request} from 'network/request'
let config = 'http://152.136.185.210:8000/api/w6/home/multidata'
function success(res) {
  console.log(res)
}
function fail(err) {
  console.log(err)
}
request(config,success,fail)

```

2. 第二种方法

```javascript
//config为一个对象
//封装位置
import axios from 'axios'
//只有一个参数时
export function request(config) {
  axios.defaults.baseURL = 'http://152.136.185.210:8000/api/w6'
  axios(config.url).then(res=> {
    config.success(res)
  }).catch(err=> {
    config.fail(err)
  })
}
//调用者位置
import {request} from 'network/request'
let config = {
  url: 'home/multidata',
  success(res){ 
   console.log(res)
  },
  fail(err) {
   console.log(err)
  }
 }
request(config)
```

3. 第三种方式

```javascript
//封装位置，返回一个promise
export function request(config) {
  let newAxios = axios.create({
    baseURL: 'http://152.136.185.210:8000/api/w6',
    timeout: 5000
  })
  return new Promise((resolve,reject) => {
    newAxios(config).then(res=> {
      resolve(res)
    }).catch(err=> {
      reject(err)
    })
  })
}
//调用者位置
import {request} from 'network/request'
request('home/multidata').then(res=> {  //request调用为一个promise
  console.log(res)
}).catch(err => {
  console.log(err)
})
```

4. 第四种方式

```javascript
//封装位置，创建一个axios实例。axios本身返回一个promise
export function request(config) {
  let newAxios = axios.create({
    baseURL: 'http://152.136.185.210:8000/api/w6',
    timeout: 5000
  })
  return newAxios(config)
}
//调用位置
import {request} from 'network/request'
request('home/multidata').then(res=> {
  console.log(res)
})
```

## 八、直观总结补充

+ **未封装**

1. **axios发送并发请求**

```javascript
// axios发送并发请求
axios.all([
  axios({
    url:'http://152.136.185.210:8000/api/w6/home/multidata'
 
  }),
  axios({
    url:'http://152.136.185.210:8000/api/w6/home/data',
    params: {
      type: 'pop',
      page: 1
    }
  })
]).then(results => {
  console.log(results)
  console.log(results[0])
  console.log(results[1])
})

//因为results是个数组，要用下标来获取对应数据
//axios.spread()可以直接将数组分割
axios.all([
  axios({
    url:'http://152.136.185.210:8000/api/w6/home/multidata'
  }),
  axios({
    url:'http://152.136.185.210:8000/api/w6/home/data',
    params: {
      type: 'pop',
      page: 1
    }
  })
]).then(axios.spread((res1,res2)=> {
  console.log(res1)
  console.log(res2)
}))
```

2. **全局配置**

```javascript
//公共的配置用axios.defaults配置
//使用全局的axios和对应的配置在进行网络请求
//不合适
axios.defaults.baseURL = 'http://152.136.185.210:8000/api/w6'
axios.defaults.timeout = 5000
axios.all([
  axios({
    url:'home/multidata'
  }),
  axios({
    url:'home/data',
    params: {
      type: 'pop',
      page: 1
    }
  })
]).then(axios.spread((res1,res2)=> {
  console.log(res1)
  console.log(res2)
}))
```

3. **axios的实例**

```javascript

//有可能会请求多个服务器
const instancel = axios.create({
  baseURL: 'http://152.136.185.210:8000/api/w6',
  timeout: 5000
})

instancel({
  url:'home/multidata'
}).then(res => {
  console.log(res)
})

instancel({
  url: "home/data",
  params: {
    type: 'pop',
    page: 1
  }
}).then(res=> {
  console.log(res)
})

//有其他服务器时
const instancel2 = axios.create({
  baseURL: '其他服务器路径',
  timeout: 5000
})
```

4. **封装axios**

```javascript
//用第三方的东西的时候尽量不要每个里面都引用，可以单独建个文件封装
//封装位置

import axios from 'axios'

export function request(config,success,failure) {
  //1.创建axios实例
  const instance = axios.create({
    baseURL: "http://152.136.185.210:8000/api/w6",
    timeout: 5000
  })

  //发送真正的网络请求
  instance(config)
    .then(res => {
      //回调出去
      success(res)
    })
    .catch(err => {
      //回调出去
      failure(err)
    })
}


//调用位置
import {request} from 'network/request'
request({
  url: "home/multidata"
},res => {
  console.log(res)
},err => {
  console.log(err)
})
```

```javascript
//config为一个对象时
//封装位置
import axios from 'axios'

export function request(config) {
  //1.创建axios实例
  const instance = axios.create({
    baseURL: "http://152.136.185.210:8000/api/w6",
    timeout: 5000
  })

  //发送真正的网络请求
  instance(config.baseConfig)
    .then(res => {
      //回调出去
      config.success(res)
    })
    .catch(err => {
      //回调出去
      config.failure(err)
    })
}

//调用位置
import {request} from 'network/request'
request({
  baseConfig:{
    url: 'home/multidata',
  },
  success:(res)=> {
    console.log(res)
  },
  failure: (err)=> {
    console.log(err)
  }
})
```

```javascript
//promise方案
//封装位置request.js

export function request(config) {
  return new Promise((resolve,reject) => {
  //1.创建axios实例
  const instance = axios.create({
    baseURL: "http://152.136.185.210:8000/api/w6",
    timeout: 5000
  })

  //发送真正的网络请求
  instance(config)
    .then(res => {
      resolve(res)
    })
    .catch(err => {
      reject(err)
    })
  })
}

//调用者位置
import {request} from 'network/request'
request({
  url: "home/multidata"
}).then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```

## 九、最终方案

```javascript
//封装位置
export function request(config) {
  //1.创建axios实例，因为axios实例返回的本身就是一个promise，所以不用自己手动包装promise
  const instance = axios.create({
    baseURL: "http://152.136.185.210:8000/api/w6",
    timeout: 5000
  })

  //发送真正的网络请求
  return instance(config)
}

//调用者位置
import {request} from 'network/request'
request({
  url: "home/multidata"
}).then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```

```javascript
//axios拦截器

//封装位置
export function request(config) {
  //1.创建axios实例
  const instance = axios.create({
    baseURL: "http://152.136.185.210:8000/api/w6",
    timeout: 5000
  })

  //2.axios拦截器
  //2.1请求拦截的作用
  instance.interceptors.request.use(config => {
    console.log('进入拦截器')
    //1.比如config中的一些信息不符合服务器的要求，比如headers里面添加
    //2.比如每次发送网络请求时，都希望在界面中显示一个请求的图标
    //3。某些网络请求（比如登录token），必须携带一些特殊的信息
    console.log(config)
    return config
  },err => {
    console.log(err)
  })
  instance.interceptors.response.use(res => {
    console.log(res.data)
    return res.data
  },err => {
    console.log(err)
  })

  //3.发送真正的网络请求
  return instance(config)
}

//调用位置
import {request} from 'network/request'
request({
  url: "home/multidata"
}).then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```





