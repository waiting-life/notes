### new

```js
function Fn(name, age) {
  this.name = name;
  this.age = age;
}
function myNew(Fn, ...args) {
  const obj = Object.create(Fn.prototype);
  const result = Fn.call(obj, ...args);
  return result instanceof Object ? result : obj;
}
const result = myNew(Fn, "cpp", 22);
console.log(result);
const result2 = new Fn("cpp", 22);
console.log(result2);
```

### call

```js
Function.prototype._call = function (context, ...args) {
  context = context || window;
  context.fn = this;
  const result = context.fn(...args);
  delete context.fn;
  return result;
};
const obj = {
  name: "cjz",
  age: 20,
};

var name = "ccc";
function getName() {
  console.log(this.name);
}
getName(); //  ccc
undefined;
getName._call(obj); //  cjz
```

### apply

```js
Function.prototype._apply = function (context, args = []) {
  context = context || window;
  context.fn = this;
  let result;
  if (Array.isArray(args)) {
    result = context.fn(...args);
  } else {
    result = context.fn();
  }
  delete context.fn;
  return result;
};
```

### bind

```js
Function.prototype._bind = function (context, ...args) {
  return (...newArgs) => {
    return this.call(context, ...args, ...newArgs);
  };
};
```

**练习**

```js
const obj1 = {
  name: "cpp",
  age: 222,
  getName() {
    console.log(this.name);
  },
};
// 将obj1传给this，this.name为obj1.name
obj1.getName(); //  cpp
```


## 函数
### 深拷贝浅拷贝
#### 浅拷贝
```js
```
#### 深拷贝

**注意**：Object.entries()
```js
const arr = [1, 2, 3]
for(const [key, value] of Object.entries(arr)) {
    console.log(key, value)
}
// 0 1
// 1 2
// 2 3
const obj = {
    name: 'cpp',
    age: 22,
    hobbies: ['game', 'music']
}
for(const [key, value] of Object.entries(obj)) {
    console.log(key, value)
}
// name cpp
// age 22
// hobbies (2) ['game', 'music']
```
```js
// 简单版本
function deepClone(source) {
    let target = null
    if(typeof source === 'object' && source !== null) {
        target = Array.isArray(source) ? [] : {}
        for(const [key, value] of Object.entries(source)) {
            target[key] = deepClone(value)
        }
    } else {
        target = source
    }
    return target
}
// 但无法解决循环引用的问题
const parent = {
    id: 1,
    name: 'father',
    children: []
}
parent.children.push(parent)
deepClone(parent)  // 报错，栈溢出

// 复杂版本
// 使用weakMap解决循环引用
function deepClone(source, hash = new WeakMap()) {
    let target = null
    if(hash.has(source)) {
        return hash.get(source)
    }
    if(typeof source === 'object' && typeof source !== null) {
        target = Array.isArray(source) ? [] : {}
        hash.set(source, target)
        for(const [key, value] of Object.entries(source)) {
            target[key] = deepClone(value, hash)
        }
    } else {
        target = source
    }
    return target
}


```

### 函数防抖函数节流
#### 函数防抖
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>函数防抖</title>
</head>
<body>
  <input type="text"/>
  <script>
    const input = document.querySelector('input')
    input.addEventListener('input', debounce(fn, 2000))
    function fn() {
      console.log(this.value)
    }
    function debounce(fn, delay) {
      let timer = null
      return function() {
        if(timer) clearTimeout(timer)
        timer = setTimeout(() => {
          fn.call(this)
        }, delay);
      }
    }
  </script>
</body>
</html>
```
#### 函数节流
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>函数节流</title>
  <style>
    body {
      height: 3000px;
      background-color: aqua;
    }
  </style>
</head>
<body>
  <script>
    // window.addEventListener('scroll', fn)
    window.addEventListener('scroll', throttle(fn, 500))
    function fn() {
      console.log(this)
    }
    function throttle(fn, delay) {
      let canRun = true
      return function() {
        if(!canRun) return false
        canRun = false
        setTimeout(() => {
          fn.call(this)
          canRun = true
        }, delay);
      }
    }
  </script>
</body>
</html>
```

## 数组方法
### 数组方法实现
```js
// find
const arr = [1, 2, 3, 4]
Array.prototype._find = function (fn, thisArg) {
    if(fn.constructor !== Function) {
        throw new Error(fn +'is not a function')
    }
    for(let i = 0; i < this.length; i++) {
        if(fn.call(thisArg, this[i], i, this)) {
            return this[i]
        }
    }
}

arr._find(item => item ===1) // 1

// forEach
Array.prototype._forEach = function(fn, thisArg) {
    if(fn.constructor !== Function) {
        throw new Error(fn +'is not a function')
    }

    for(let i = 0; i < this.length; i++) {
        fn.call(thisArg, this[i], i, this)
    }
}
arr._forEach(item => {
    console.log(item)   
})
// 1
// 2
// 3
// 4

// map
Array.prototype._map = function(fn, thisArg) {
    if(fn.constructor !== Function) {
        throw new Error(fn +'is not a function')
    }
    const newArr = []
    for(let i = 0; i < this.length; i++) {
        newArr.push(fn.call(thisArg, this[i], i, this))
    }
    return newArr
}
arr._map(item => item * item) // [1, 4, 9, 16]

// filter
Array.prototype._filter = function(fn, thisArg) {
    if(fn.constructor !== Function) {
        throw new Error(fn +'is not a function')
    }
    const newArr = []
    for(let i = 0; i < this.length; i++) {
        if(fn.call(thisArg, this[i], i, this)) {
            newArr.push(this[i])
        }
    }
    return newArr
}
arr._filter(item => item < 3) // [1, 2]
```
### 判断数组

### 数组去重
```js
function unique(arr) {
    let newArr = []
    for(const item of arr) {
        if(!newArr.includes(item)) {
            newArr.push(item)
        } 
    }
    return newArr
}

[...new Set(arr)]

Array.from(...new Set(arr))
```
### 打平数组
```js
function flatten(arr, result = []) {
    for (const item of arr) {
        if (Array.isArray(item)) {
            flatten(item, result )
        } else {
            result.push(item)
        }
    }
    return result;
}

function flatten2(arr) {
    let newArr = []
    arr.forEach(item => {
        if(Array.isArray(item)) {
            newArr = newArr.concat(flatten2(item)) // 函数调用生成一个新的作用域
        } else {
            newArr.push(item)
        }
    })
    return newArr
}
```


## 原型相关
### instanceof实现
```js
function myInstanceof(a, b) {
    if(typeof a !== Object || typeof a === null) return false
    let proto = Object.getPrototypeOf(a)
    while(true) {
        if(proto === null) return false
        if(proto === b.prototype) return true
        proto = Object.getPrototypeOf(proto)
    }
}
```
### 继承
#### es5继承
```js
function Person(name, age){
    this.name = name
    this.age = age
    this.setAge = function(age) {
        this.age = age
    }
}

// 寄生虫继承
function Teacher2(count, ...args) {
    Person.call(this, ...args)
    this.count = count
    this.setCount = function(count) {
        this.count = count
    }
}
Teacher2.prototype = Object.create(Person.prototype)
Teacher2.prototype.constructor = Teacher2

const t2 = new Teacher2(33, 'cjz', 22)
t2.name
// 'cjz'
t2.age
// 22
t2.count
// 33
```
#### class继承
```js
// class继承  Person  Teacher
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    // 原型上
    setAge(age) {
        this.age = age
    }
}
class Teacher extends Person {
    constructor(name, age, count) {
        super(name, age)
        this.count = count
    }
    setCount(count) {
        this.count = count
    }
}

const t = new Teacher('cpp', 22, 50)
t.name
// 'cpp'
t.age
// 22
t.setAge(24)
t.age
//24
t.count
// 50
t.setCount(60)
t.count
// 60
```


### 实现私有变量
#### 提前约定好私有变量

#### 比较好的方法是结合闭包+Symbol
如题目：创建一个 Person 类，其包含公有属性 name 和私有属性 age 以及公有方法 setAge ；创建一个 Teacher 类，使其继承 Person ，并包含私有属性 studentCount 和私有方法 setStudentCount 。

**方法一**
```js

const [Person, Teacher] = (function() {
    const _age = Symbol('age')
    const _studentCount = Symbol('studentCount')
    const _setStudentCount = Symbol('setStudentCount')
    class Person {
        constructor(name, age) {
            this.name = name
            this[_age] = age
        }
        setAge(age) {
            this[_age] = age
        }
        getAge() {
            return this[_age]
        }
    }

    class Teacher extends Person {
        constructor(name, age, count) {
            super(name, age)
            this[_studentCount] = count
        }
        [_setStudentCount](count) {
            this[_studentCount] = count
        }
        getStudentCount() {
            return this[_studentCount]
        }
    }
    return [Person, Teacher]
})()
const t = new Teacher('cpp', 22, 40)
t.name
// 'cpp'
t.getAge()
// 22
t.getStudentCount()
// 40

t.setAge(24)
t.getAge()
// 24
```
**方法二**
```js
const Person = (function() {
    const _age = Symbol('age')
    function Person(name, age) {
        this.name = name
        this[_age] = age
    }
    Person.prototype.setAge = function(age) {
        this[_age] = age
    }
    return Person
})()

const Teacher = (function() {
    const _studentCount = Symbol('studentCount')
    const _setStudentCount = Symbol('setStudentCount')
    function Teacher(count, ...args) {
        this[_studentCount] = count
        Person.call(this, ...args)
    }
    Teacher.prototype[_setStudentCount] = function(count) {
        this[_studentCount] = count
    }
    return Teacher
})()
// 继承
Teacher.prototype = Object.create(Person.prototype)
Teacher.prototype.constructor = Teacher
ƒ Teacher(count, ...args) {
        this[_studentCount] = count
        Person.call(this, ...args)
    }
const t = new Teacher(44, 'cpp', 22)
t.setAge(20)
t
```

## ES6
### Promise

#### Promise.all实现
```js
Promise._all = function (pArr) {
    return new Promise((resolve, reject) => {
      const newArr = []
      const len = pArr.length
      let count = 0
      pArr.forEach((p, i) => {
          p.then(value => {
              newArr[i] = value
              count++
              if(count === len) {
                  resolve(newArr)
              }
          }, err => reject(err))
      })
  }) 
}
let p1 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        console.log(1);
        resolve(1)
    }, 3000)
})
let p2 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        console.log(2);
        resolve(2)
    }, 2000)
})
let p3 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        console.log(3);
        resolve(3)
    }, 1000)
})
Promise._all([p1, p2, p3])
```
#### Promise.race实现
```js
Promise._race = function(promiseArr) {
    return new Promise((resolve, reject) => {
        promiseArr.forEach((promise) => {
            promise.then(value => {
                resolve(value)
            }, reason => {
                reject(reason)
            })
        })
    })
}
let p1 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        console.log(1);
        resolve(1)
    }, 3000)
})
let p2 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        console.log(2);
        resolve(2)
    }, 2000)
})
let p3 = new Promise((resolve,reject)=>{
    setTimeout(() => {
        console.log(3);
        resolve(3)
    }, 1000)
})
Promise._race([p1, p2, p3])
```

###  发布订阅
#### 实现
```js
class EventEmitter {
    constructor() {
        this.list = {}
    }
    on(name, fn, type = 1) {
        if(!this.list[name]) {
            this.list[name] = []
        }
        this.list[name].push([fn, type])
    }
    emit(name, ...args) {
        const fns = this.list[name]
        if (!fns || fns.length === 0) return
        fns.forEach((fn, index)=> {
            fn[0](...args)
            if(fn[1] === 0) {
                this.list[name].splice(index, 1)
            }
        })
    }
}

let bus = new EventEmitter()
bus.on('click', (value) => {
     console.log(value, 111)   
}, 1)
bus.on('click', (value) => {
    console.log(value, 222)
}, 0)
undefined
bus.emit('click', 'test')
// test 111
// test 222

bus
```
#### 添加once、remove函数
```js

class EventEmitter {
    constructor() {
        this.list = {}
    }
    on(name, fn, type = 1) {
        if(!this.list[name]) {
            this.list[name] = []
        }
        this.list[name].push([fn, type])
    }
    once(name, fn) {
        this.on(name, fn, 0)
    }
    emit(name, ...args) {
        const fns = this.list[name]
        fns.forEach((fn, index)=> {
            fn[0](...args)
            if(fn[1] === 0) {
                this.list[name].splice(index, 1)
            }
        })
    }
    remove(name, func) {
        const fns = this.list[name]
        if(!fns) return 
        fns.forEach((fn, index) => {
            if(fn[0] === func) {
                this.list[name].splice(index, 1)
            }
        })
        
    }
}

let bus = new EventEmitter()
bus.on('click', (value) => {
     console.log(value, 111)   
}, 1)
bus.on('click', (value) => {
    console.log(value, 222)
}, 0)
bus.emit('click', 'test')
// test 111
// test 222
```


