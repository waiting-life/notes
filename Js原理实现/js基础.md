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

## 函数防抖函数节流

### 函数防抖

### 函数节流
