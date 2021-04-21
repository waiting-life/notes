# promise

+ 读文件是异步，无法决定顺序

```js
const fs = require('fs')
// 异步顺序不一定谁先谁后
fs.readFile('./data/a.txt', (err, data) => {
  if (err) {
    // return console.log('读取失败')

    // 抛出异常
    //  1. 阻止程序的执行
    //  2. 把错误消息打印到控制台
    throw err
  } 
  console.log(data.toString())
})
fs.readFile('./data/b.txt', (err, data) => {
  if (err) {
    // return console.log('读取失败')

    // 抛出异常
    //  1. 阻止程序的执行
    //  2. 把错误消息打印到控制台
    throw err
  } 
  console.log(data.toString())
})
fs.readFile('./data/c.txt', (err, data) => {
  if (err) {
    // return console.log('读取失败')

    // 抛出异常
    //  1. 阻止程序的执行
    //  2. 把错误消息打印到控制台
    throw err
  } 
  console.log(data.toString())
})
```

+ 利用回调嵌套保证顺序先后 callback ell

```js
const fs = require('fs')
// 回调函数，回调地狱，按顺序执行,顺序不会变
fs.readFile('./data/a.txt', 'utf8', (err, data) => {
  if (err) {
    // return console.log('读取失败')

    // 抛出异常
    //  1. 阻止程序的执行
    //  2. 把错误消息打印到控制台
    throw err
  } 
  console.log(data)
  fs.readFile('./data/b.txt', 'utf8', (err, data) => {
    if (err) {
      // return console.log('读取失败')

      // 抛出异常
      //  1. 阻止程序的执行
      //  2. 把错误消息打印到控制台
      throw err
    } 
    console.log(data)
    fs.readFile('./data/c.txt', 'utf8', (err, data) => {
      if (err) {
        // return console.log('读取失败')

        // 抛出异常
        //  1. 阻止程序的执行
        //  2. 把错误消息打印到控制台
        throw err
      } 
      console.log(data)
    })
  })
})
```

为了解决以上编码方式（回调地狱嵌套）带来的问题，所以es6中新增了一个API，`promise`

+ promise基本语法

```js
// 在 Ecmascript6中新增了一个API promise
// Promise是一个构造函数

// 创建 Promise 容器
// 1.给别人一个承诺 ipromise you
//    promise一旦创建，就会立即执行里面的代码,承诺本身不是异步的，它里面的任务是异步的
const fs = require('fs')
const p1 = new Promise((resolve, reject) => {
  fs.readFile('./data/a.txt', 'utf8', (err, data) => {
    if (err) {
      // 失败了，承诺容器中的任务失败了
      // console.log(err)
      // 把容器的pending状态变为rejected
      reject(err)
    } else {
      // 承诺容器中的任务成功了
      // console.log(data)
      // 把容器的pending状态改为成功resolved
      resolve(data)
    }
  })
})
p1.then((data) => {
  console.log(data)
}, (err) => {
  console.log('读取文件失败了', err)
})
```

+ promise 链式调用

```js
const fs = require('fs')
const p1 = new Promise((resolve, reject) => {
  fs.readFile('./data/a.txt', 'utf8', (err, data) => {
    if (err) {
      // 失败了，承诺容器中的任务失败了
      // console.log(err)
      // 把容器的pending状态变为rejected
      reject(err)
    } else {
      // 承诺容器中的任务成功了
      // console.log(data)
      // 把容器的pending状态改为成功resolved
      resolve(data)
    }
  })
})
const p2 = new Promise((resolve, reject) => {
  fs.readFile('./data/b.txt', 'utf8', (err, data) => {
    if (err) {
      // 失败了，承诺容器中的任务失败了
      // console.log(err)
      // 把容器的pending状态变为rejected
      reject(err)
    } else {
      // 承诺容器中的任务成功了
      // console.log(data)
      // 把容器的pending状态改为成功resolved
      resolve(data)
    }
  })
})
const p3 = new Promise((resolve, reject) => {
  fs.readFile('./data/c.txt', 'utf8', (err, data) => {
    if (err) {
      // 失败了，承诺容器中的任务失败了
      // console.log(err)
      // 把容器的pending状态变为rejected
      reject(err)
    } else {
      // 承诺容器中的任务成功了
      // console.log(data)
      // 把容器的pending状态改为成功resolved
      resolve(data)
    }
  })
})
p1
.then((data) => {
  console.log(data)
  // return 一个promise对象
  // 当前函数中的 return 的结果就可以在后面的 then 中的 function 接收
  return p2
}, (err) => {
  console.log('读取文件失败了', err)
})
.then(data => {
  console.log(data)
  return p3
})
.then(data => {
  console.log(data)
})
```

+ 封装promise，promise版本的readFile

```js
const fs = require('fs')
function pReadFile(filePath) {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, 'utf8', (err, res) => {
      if (err) {
        reject(err)
      } else {
        resolve(res)
      }
    })
  }) 
}
pReadFile('./data/a.txt')
  .then(res => {
    console.log(res)
    return pReadFile('./data/b.txt')
  })
  .then(res => {
    console.log(res)
    return pReadFile('./data/c.txt')
  })
  .then(res => {
    console.log(res)
  })
```

## 一、概念

1. promise是异步编程的一种解决方案。
2. Promise新建后就会立即执行

**特点**：

1. promise对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态，只有异步操作的结果，可以决定当前是哪一种状态。任何其他操作都无法改变这个状态。
2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。

**有了promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。**

**Promise缺点**：

1. 无法取消`Promise`,一旦新建它就会立即执行，无法中途取消。
2. 如果不设置回调函数，`Promise`内部的的错误，不会反应到外部。
3. 当处于`pending`状态时，无法得知目前进展到哪一个阶段。



**补充**：

`setTimeout()`方法参数

function：是在到期时间之后执行的函数。

code：这是一个可选语法，可以使用字符串而不是function，在delay毫秒之后编译和执行字符串（**不推荐**）

delay：可选，延迟的毫秒数

`arg1,...,argN`可选：一旦定时器到期，它们会作为参数传递给function

## 二、基本用法

`Promise`实例状态变为`resolved`，就会触发`then`方法绑定的回调函数。

## 三、方法

#### 1. Promise.prototype.then()

**概念**：它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数，它们都是可选的

+ `then`方法返回的是一个新的`Promise`实例,因此可以采用链式调用。
  + 第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。
  + 采用链式的`then`，可以指定一组按照次序调用的会带哦函数。这时前一个回调函数可能返回的还是一个`Promise`对象（即有异步操作）这时后一个回调函数，就会等待该`Promise`对象的状态发生变化，才会被调用。

```js
const p1 = new Promise(resolve => {
    resolve(111)
})
p1.then(value => value *2)
  .then(value => value)
  .catch(err => err)
Promise {<fulfilled>: 222}

console.log(p1.then(value => value *2).catch(err => err))
// Promise {<pending>}
```



#### 2. Promise.prototype.catch()

**概念**：是`.then(null, rejection)或.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

+ 如果异步操作抛出错误，状态就会变为`rejected`，就会调用`catch()`方法指定的回调函数，处理这个错误。
+ 另外`then()`方法指定的回调函数，如果运行中抛出错误，也会被`catch()`方法捕获。

```js
p.then((val) => console.log('fulfilled:', val))
  .catch((err) => console.log('rejected', err));

// 等同于
p.then((val) => console.log('fulfilled:', val))
  .then(null, (err) => console.log("rejected:", err));
```

+ 如果Promise状态已经变成`resolved`，再抛出错误是无效的。

```js
const p = new Promise((resolve, reject) => {
    resolve('ok')
    throw new Error('test')
})
undefined
p.then(value => console.log(value))
 .catch(err => console.log(err))
VM70002:1 ok
```

+ Promise对象的错误具有”冒泡“性质，会一直向后传递，直到被捕获为止。也就是说错误总是会被下一个`catch`语句捕获。

```js
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```

+ `catch()`方法返回的还是一个Promise对象，因此后面还可以接着调用`then()`方法。

```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing()
.catch(function(error) {
  console.log('oh no', error);
})
.then(function() {
  console.log('carry on');
});
// oh no [ReferenceError: x is not defined]
// carry on
```

+ `catch()`方法之中还能再抛出错误。

```js
const someAsyncThing = function() {
    return new Promise((resolve, reject) => {
        // x没有声明，会报错
        resolve(x + 2)
    })
}
someAsyncThing()
    .then(() => someOtherAsyncThing())
    .catch(err => {
       console.log(err)
    	// y也没有声明
        y + 3
    })
    .catch(err => console.log(err))
// ReferenceError: x is not defined
// ReferenceError: y is not defined

```

#### 3. Promise.prototype.finally()

**概念**：

`finally()`方法用于指定不管Promise对象最后状态如何，都会执行的操作。

```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

上面代码中，不管`promise`最后的状态，在执行完`then`、或`catch`指定的回调函数以后，都会执行`finally`方法指定的回调函数

+ `finally`方法的回调函数不接受任何参数，这意味着没有办法知道，前面的Promise状态到底是fulfilled还是rejected。这表明finally方法里的操作，应该是与状态无关的，不依赖于Promise的执行结果。
+ `finally`本质上是`then`方法的特特例

```js
promise
.finally(() => {
  // 语句
});

// 等同于
promise
.then(
  result => {
    // 语句
    return result;
  },
  error => {
    // 语句
    throw error;
  }
);
```

+ finally方法总是会返回原来的值。

#### 4. Promise.all()

**概念**：

`Promise.all()`方法用于将多个Promise实例，包装成一个新的Promise实例。

+ `Promise.all()`方法接受一个数组作为参数。数组里面的每一项都是Promise实例。如果不是，就会调用`Promise.resolve()`方法，将参数转为Promise实例。Promise.all()方法的参数可以不是数组，但必须有Iterator接口，且返回的每个成员都是Promise实例。

```javascript
const p = Promise.all([p1, p2, p3]);
```

1. 只有p1， p2，p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
2. 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

**注意**：

如果作为参数的Promise实例，自己定义了catch方法，那么它一旦别rejected并不会触发Promise.all()的catch方法。

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]
```

上面代码中，`p1`会`resolved`，`p2`首先会`rejected`，但是`p2`有自己的`catch`方法，该方法返回的是一个新的`Promise`实例，`p2`指向的实际上是这个实例，该实例执行完`catch`方法后，也会变成`resolved`，导致`Promise.all()`方法参数里面的两个实例都会`resolved`，因此会调用`then`方法指定的回调函数，而不会调用`catch`方法指定的回调函数。



如果`p2`没有指定自己的`catch`方法，就会调用`Promise.all()`的`catch`方法

```javascript
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// Error: 报错了
```

#### 5. Promise.race()

**概念**：

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```javascript
const p = Promise.race([p1, p2, p3]);
```

上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数

## 四、实现

#### 1. 实现Promise.all函数

```js
Promise._all = function(promiseArr) {
    return new Promise((resolve, reject) => {
        let newPromiseArr = []
        let count = 0 
        let length = promiseArr.length
        promiseArr.forEach((promise, index) => {
            promise.then(value => {
                newPromiseArr[index] = value 
                count++
                if(count === length) {
                    resolve(newPromiseArr)
                }
            }, err => reject(err))
        })
    })
}
let p1 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        
        resolve(100)
    }, 3000)
})
let p2 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        
        resolve(200)
    }, 2000)
})
let p3 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        
        resolve(300)
    }, 1000)
})
Promise._all([p1, p2, p3]).then(value => console.log(value))
// [100, 200, 300]
```

#### 2. 实现Promise.race()函数

```js
Promise._race = function(promiseArr) {
    return new Promise((resolve, reject) => {
        promiseArr.forEach(promise => {
            promise.then(value => resolve(value), err => reject(err))
        })
    })
}

let p1 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        
        resolve(100)
    }, 3000)
})
let p2 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        
        resolve(200)
    }, 2000)
})
let p3 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        
        resolve(300)
    }, 1000)
})
Promise._race([p1, p2, p3]).then(value => console.log(value))

// 300
```

## 五、其他

```js
const p1 = new Promise((resolve) => resolve(111))
p1.then()
console.log(p1.then())
```



```js
const p1 = new Promise(resolve => resolve(111))

console.log(A())
console.log(p1.then().catch())
async function A() {
    await p1.then().catch()
    
}


async function B() {
    return p1
}
```

```js
async function A() {
    const p1 = new Promise(resolve => resolve(111))
    await p1.then(() => {
        console.log(1)
    }).then(() => console.log(2))
    console.log(3)
}
A()
new Promise((resolve, reject) => {
    console.log('promise1')
    resolve()
}).then(() => console.log('promise2'))
  .then(() => console.log('promise3'))
  .then(() => console.log('promise4'))
  .then(() => console.log('promise5'))
// promise1
// 1
// promise2
// 2
// promise3
// 3
// promise4
// promise5
```



