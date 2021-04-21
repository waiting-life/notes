在javascript中，若省略var关键字而直接赋值，那么这个变量为全局变量，哪怕是在function、里定义的

http-server有缓存，要network里面关闭缓存才能显示变化

let 关键字：块级作用域，暂时性死区（在声明前引用这个值的话，会出现死区），不允许重复声明

const关键字，类似于let，用于定义常量，不允许重复赋值

<img src="D:\工作日报\工作日报5\定义变量.png" alt="定义变量" style="zoom:50%;" />

大部分情况下使用const

确实需要重复赋值，使用let

字符串的扩展：

模板字符串：``,${}

函数的扩展：

箭头函数：

es5：function(x) {}

es6: (x)=> {}

参数列表只有一个参数时箭头函数括号可以省略x=> { return x*x}

函数体里面只有一个return语句时，可以省略return和大括号x=>x*x

如果return的是一个对象的话，不能直接省略大括号和return。    x=>{return {result: x*x}}

可以用括号括起来 x=> ({result: x*x})

函数里面有arguments（对象，类数组,非常适合动态参数的场景），this

this：执行环境

如果函数在全局作用域中执行，通常指向window对象

如果作为对象的一个方法被调用，this指向对应的对象（调用的时候知道的）

函数的方法：bind(),接受一个对象或者一个环境，会返回一个新的函数，可以改变函数的this指向

​						apply(),传参是一个数组

​						call()，传参是一个一个

map()方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值

map() 方法按照原始数组元素顺序依次处理元素。

注意： map() 不会对空数组进行检测。

注意： map() 不会改变原始数组。

箭头函数

this的指向，声明时的this，不能当作构造函数，没有arguments

默认参数：只有在传参为undefined或者没有传的时候会生效

带默认值的参数最好放在参数的尾部



扩展参数

当传参的个数超出了定义的个数，多出来的将变成扩展参数数组

取代arguments对象，arguments是一个对象不是数组，类数组

只能放在参数列表尾部

使用箭头函数像要用不定长参数时，用扩展参数...args，args是一个数组

数组的扩展

Math.max接受的参数是一组数

apply方法会把一个数组转化成参数列表传给要传的函数

扩展运算符：

将一个数组转换为“值的序列”

用在函数传参中

用在数组定义中

浅拷贝



数组的解构赋值：基于位置的匹配，默认值，剩余值（类似函数参数），嵌套解构



新增静态属性

Array.from()转化为数组

Array.of()

数组上新增原型方法

find()接受一个回调函数，存在返回元素本身，不存在返回undefined

findIndex()

arr.fill('lovecpp')//用这个字符串填充这个数组

includes()

arr.flat()将数组打平，参数是打平的最大层级



<img src="D:\工作日报\工作日报5\flat.png" alt="flat" style="zoom:67%;" />



<img src="D:\工作日报\工作日报5\es6方法.png" alt="es6方法" style="zoom:67%;" />

<img src="D:\工作日报\工作日报5\assign.png" alt="assign" style="zoom:60%;" />

Object.assign会更改第一个参数，后面的参数会往第一个参数填充

解决方法：在第一个参数前面建一个新的空对象

```js
const obj1 = {

  a: 1,

  b: 2,

  c: 3

}
```



```js
const obj2 = {

  c: 4,

  d: 5,

  e: 6

}
```



```js
const obj3 = Object.assign(

  {},

  obj1,

  obj2,

)
console.log(obj3,obj1,obj2)
```





扩展运算符和Object.assign的区别额，扩展运算符不会更改

Object.keys()

Object.entries()

Object.values()

const obj1 = {
    a: 1,
    b: 2,
    c: 3
}

console.log(
    Object.keys(obj1),  //取对象的可遍历键
    Object.values(obj1),  //取值
    Object.entries(obj1)  //取键值对，每个键值对是用数组来表示的
)

Object.fromEntries()相当于做了一层浅拷贝，生成一个新的对象，是entries的逆动作

const obj1 = {

  a: 1,

  b: 2,

  c: 3

}

const e = Object.entries(obj1).map(entry => [    

  entry[0],entry[1]*entry[1]  //第0个位置是键，第一个位置是值

])

const newObj = Object.fromEntries(e)

console.log(obj1,e,newObj)



Object .is()，能区分NaN还有能判断0，-0

Object.is(NaN,NaN)
true
NaN === NaN
false
Object.is(0,-0)
false
0 === -0
true



console.log(Number.isNaN(NaN))
true

function isNegativeZero(n) {
    return 0 === n &&(1/n) === -Infinity
}

console.log(isNegativeZero(0))
false
console.log(isNegativeZero(-0))
true



定义类的新方式

static关键字用于定义类的静态方法

super关键字用于在子类的构造函数中，调用父类的构造函数

新的数据解构

Map()    : set(),has(),delete(),size,clear(),keys()

Set():键与值相同的Map，数组去重



symbol

互不相同的“字符串”，可作为属性名

可取代字符串常量

实现类的私有属性

# Es6

``` js
function A({x = 1000, y} = {}) {
    console.log(x, y)
}
```



``` js
let x = 1
let A, B, C
{
    let x = 2
    A = function () {
        x = 100
    }

    B = function() {
        console.log(x)
    }
}

{
    let x = 3
    A()
    C = function() {
        console.log(x)
    }
}
```

**x_x**

```javascript
let lastWord = 'last word';

const a = {
  'first word': 'hello',
  [lastWord]: 'world'
};

a['first word'] // "hello"
a[lastWord] // "world"
a['last word'] // "world"
```





注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串`[object Object]`，这一点要特别小心。

```javascript
const keyA = {a: 1};
const keyB = {b: 2};

const myObject = {
  [keyA]: 'valueA',
  [keyB]: 'valueB'
};

myObject // Object {[object Object]: "valueB"}
```

上面代码中，`[keyA]`和`[keyB]`得到的都是`[object Object]`，所以`[keyB]`会把`[keyA]`覆盖掉，而`myObject`最后只有一个`[object Object]`属性。





数组的扩展方法：

**flta，flatMap**

```js
const arr = [1,2,3,4,5]
// 只有一个返回值，所以省略return语句
const arr1 = arr.map(n => [
    n,
    ...new Array(n).fill(0)
])
console.log(arr1)
// [1, 0]
// [2, 0, 0]
// [3, 0, 0, 0]
// [4, 0, 0, 0, 0]
// [5, 0, 0, 0, 0, 0]


//用flat打平数组
const arr2 = arr.map(n => [
    n,
    ...new Array(n).fill(0)
]).flat()
console.log(arr2)
// [1, 0, 2, 0, 0, 3, 0, 0, 0, 4, 0, 0, 0, 0, 5, 0, 0, 0, 0, 0]


// flatMap方法，先map，再flat
const arr3 = arr.flatMap(n => [
    n,
    ...new Array(n).fill(0)
])
console.log(arr3)
// [1, 0, 2, 0, 0, 3, 0, 0, 0, 4, 0, 0, 0, 0, 5, 0, 0, 0, 0, 0]
```





**Object.assign**

```js
const obj1 = {
    name: 'cpp',
    age: 2
}
const obj2 = {
    name: 'cjz',
    love: 'wqj'
}
Object.assign({}, obj1, obj2)
// {name: "cjz", age: 2, love: "wqj"}
```



## 字符串的扩展





### 模板字符串

+ 如果大括号中的值不是字符串，将按照一般的规则转为字符串。比如，打括号中是一个对象，将默认调用对象的```toString```方法。
+ 如果·模板字符串中的变量没有声明，将报错。
+ 模板字符串还能嵌套。



- 第一个参数：`['Hello ', ' world ', '']`
- 第二个参数: 15
- 第三个参数：50

```js
let a = 5;
let b = 10;

function tag(s, v1, v2) {
  console.log(s[0]);
  console.log(s[1]);
  console.log(s[2]);
  console.log(v1);
  console.log(v2);

  return "OK";
}

tag`Hello ${ a + b } world ${ a * b}`;
// 等同于tag(['Hello ', ' world ', ''], 15, 50)

// "Hello "
// " world "
// ""
// 15
// 50
// "OK"
```

**注意**：模板处理函数的第一个参数，还有一个raw属性。比如下面，tag函数的第一个参数·strings，有一个raw属性·，也指向一个数组。该数组的成员与strings数组完全一致。

```js
function tag(strings) {
    console.log(strings.raw[0])
}
undefined
tag`first line\nSecond line`
first line\nSecond line
```

## 字符串的新增方法

#### 1. String.fromCharCode()

ES5提供```String.fromCharCode()```方法，用于从Unicode码点返回对应字符，但是这个方法不能识别大于```0xFFFF```的字符。

```js
String.fromCharCode(0x20bb7)
"ஷ"   // 未识别
```

ES6提供了```String.fromCodePoint()```方法，可以识别大于```0xFFF```的字符

```js
String.fromCodePoint(0x20bb7)
"𠮷"	// 识别
```

**注意**：```fromCodePoint```方法定义在```String```对象上，而```codePointAt```方法定义在字符串的实例对象上。

#### 2. String.raw()

ES6为原生的String对象，提供了一个```raw()```方法。该方法返回一个斜杠都被转义的字符串。往往用于模板字符串的处理方法。

```js
String.raw`hi\n${2+3}!`
"hi\n5!"
//实际返回"hi\\n5!"，显示的是转义后的结果"hi\n5!"
```

如果原字符串的斜杠已经被转义，那么```String.raw()```会进行再次转义。

```js
String.raw`hi\\n`
//返回"hi\\\\n"
"hi\\n"

String.raw`hi\\n` === "hi\\\\n"
true
```

#### 3. 实例方法：codePointAt()

JavaScript 内部，字符以 UTF-16 的格式储存，每个字符固定为`2`个字节。对于那些需要`4`个字节储存的字符（Unicode 码点大于`0xFFFF`的字符），JavaScript 会认为它们是两个字符。

ES6提供了```codePointAt()```方法，能够正确处理4个字节储存的字符。返回一个字符的码点。

```js
for (let ch of s) {
    console.log(ch.codePointAt(0).toString(16))
}
20bb7
61

```

#### 5. 实例方法： includes(),startWith(),endsWith()

传统上，JavaScript只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了三种新方法

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

**注意**：这三个方法都支持第二个参数，表示开始搜索的位置。

#### 6. 实例方法repeat

**repeat**方法返回一个新的字符串，表示将原字符串重复**n**次

#### 7. 实例方法padStart(),padEnd()

```js
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串

```js
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```

#### 8. 实例方法： trimStart()，trimEnd()

**trimStart()**消除字符串头部的空格

**trimEnd()**消除尾部的空格。

**注意**：都返回新字符串

#### 9. 实例方法：matchAll()

**matchAll()**返回一个正则表达式在当前字符串的所有匹配。

#### 10. 实例方法：replaceAll()

字符串的实例方法**replace()**方法只能替换第一个匹配

**replaceAll()**方法，可以一次性替换所有匹配。

## 正则的扩展





## 数值的的扩展

#### 1. 二进制和八进制表示法

####  

ES6 提供了二进制和八进制数值的新的写法，分别用前缀`0b`（或`0B`）和`0o`（或`0O`）表示。

#### 2. Number.isFinite(),Number.isNaN()

它们与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断。

而这两个新方法只对数值有效。

+ Number,isFinite()对于非数值一律返回false

+ Number.sNaN()只有对于NaN才返回true，非NaN一律返回false。

  

#### 3. Number.parseInt(),Number.parseFloat()

ES6将全局方法parseInt()和parseFloat(),移植到Number对象上面，行为完全保持不变

#### 4. Number.isInteger()

Number.isInteger()用来判断一个数值是否为整数。

**注意**：如果对数据精度的要求较高，不建议使用Number.isInteger()判断一个数值是否为整数。

#### 5. Math对象的扩展



**1. Math.trunc()**方法用于去除一个数的小数部分，返回整数部分。

**2. Math.sign()**方法用来判断一个数到底是证书、负数、还是零。

**3. Math.cbrt()**方法用于计算一个数的立方根。

**4. 对数方法**

+ Math.expm1(x)返回e的x方-1，即Math.exp(x)-1

## 函数的扩展

```javascript
// 写法一
function m1({x = 0, y = 0} = {}) {
  return [x, y];
}

// 写法二
function m2({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}
```

上面两种写法都对函数的参数设定了默认值，区别是写法一函数参数的默认值是空对象，但是设置了对象解构赋值的默认值；写法二函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值。

```javascript
// 函数没有参数的情况
m1() // [0, 0]
m2() // [0, 0]

// x 和 y 都有值的情况
m1({x: 3, y: 8}) // [3, 8]
m2({x: 3, y: 8}) // [3, 8]

// x 有值，y 无值的情况
m1({x: 3}) // [3, 0]
m2({x: 3}) // [3, undefined]

// x 和 y 都无值的情况
m1({}) // [0, 0];
m2({}) // [undefined, undefined]

m1({z: 3}) // [0, 0]
m2({z: 3}) // [undefined, undefined]
```

#### 参数默认值的位置

通常情况下，定义了默认值的参数，应该是函数的尾参数。

#### 函数的length属性

指定了默认值以后，函数的length属性，将返回没有指定默认值的参数的个数。也就是说，指定了默认值后，length属性将失真。

```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了

```javascript
(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
```

#### 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数就会形成一个单独的作用域。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

**难**

```javascript
var x = 1;
function foo(x, y = function() { x = 2; }) {
  var x = 3;
  y();
  console.log(x);
}

foo() // 3
x // 1
```

#### rest参数

下面是一个 rest 参数代替`arguments`变量的例子。

```javascript
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```

**arguments**对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用**Array.prototype.slice.call**先将其转为数组。rest参数就不存在这个问题，它就是一个真正的数组，数组的方法可以使用。下面是一个利用rest参数改写数组push方法的实例。

```javascript
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3)
push(a,1,2,3)
// 1
// 2
// 3
console.log(a)
// [1, 2, 3]
```

**注意**：ES2016 做了一点修改，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

#### 4. name属性

函数的name属性，返回该函数的函数名。

```js
function foo() {}
// "foo"
```

**注意**：

+ 如果将一个匿名函数赋值给一个变量，ES5 的`name`属性，会返回空字符串，而 ES6 的`name`属性会返回实际的函数名。

+ 如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的`name`属性都返回这个具名函数原本的名字。

+ Function构造函数返回的函数实例，name属性的值为anonymous

```javascript
(new Function).name
// "anonymous"
```

+ `bind`返回的函数，`name`属性值会加上`bound`前缀

```javascript
function foo() {};
foo.bind({}).name 
// "bound foo"

(function(){}).bind({}).name 
// "bound "
```

#### 5. 箭头函数

**注意**：由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

```js
let lg = (name,age) => {name: 'cpp',age: 2}
// 报错

let lg = (name,age) => ({name: 'cpp',age: 2})
// 不报错
```

如果箭头函数只有一行语句，且不需要返回值，可以采用下面的写法，就不用写大括号了

```javascript
let fn = () => void doesNotReturn();
```

```js
const person = {name: 'cpp', age: 2}
const { name, age } = person
name
// "cpp"
age
// 2
const full = ({ name, age } = person ) => `${name} 今年 ${age}岁`
full()
// "cpp 今年 2岁"
```

rest 参数与箭头函数结合的例子

```js
const numbers = (...nums) => nums
numbers(1,2,3,4,5)
// [1, 2, 3, 4, 5]
```

**箭头函数使用注意点**

1. 函数体内**this**对象，就是定义时所在的对象，而不是使用时所在的对象
2. 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3. 不可以使用arguments对象，该对象在函数体内不存在，如果要用，可以用**rest**参数代替
4. 不可以使用**yield**命令，因此箭头函数不能用作**Generator**

```javascript
function foo() {
  return () => {
    return () => {
      return () => {
        console.log('id:', this.id);
      };
    };
  };
}

var f = foo.call({id: 1});

var t1 = f.call({id: 2})()(); // id: 1
var t2 = f().call({id: 3})(); // id: 1
var t3 = f()().call({id: 4}); // id: 1
```

上面代码之中，只有一个`this`，就是函数`foo`的`this`，所以`t1`、`t2`、`t3`都输出同样的结果。因为所有的内层函数都是箭头函数，都没有自己的`this`，它们的`this`其实都是最外层`foo`函数的`this`。

+ 除了`this`，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：`arguments`、`super`、`new.target`。

```javascript
function foo() {
  setTimeout(() => {
    console.log('args:', arguments);
  }, 100);
}

foo(2, 4, 6, 8)
// args: [2, 4, 6, 8]
```

上面代码中，箭头函数内部的变量`arguments`，其实是函数`foo`的`arguments`变量。

+ 另外，由于箭头函数没有自己的`this`，所以当然也就不能用`call()`、`apply()`、`bind()`这些方法去改变`this`的指向。

**不适用场合**

由于箭头函数使得`this`从“动态”变成“静态”，下面两个场合不应该使用箭头函数。

+ 第一个场合是定义对象的方法，且该方法内部包括`this`。

```javascript
const cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}
```

上面代码中，`cat.jumps()`方法是一个箭头函数，这是错误的。调用`cat.jumps()`时，如果是普通函数，该方法内部的`this`指向`cat`；如果写成上面那样的箭头函数，使得`this`指向全局对象，因此不会得到预期结果。这是因为对象不构成单独的作用域，导致`jumps`箭头函数定义时的作用域就是全局作用域。

```js
name = 'cpp'
const obj = {
    name: 'cjz',
    say: () => console.log(this.name)
}
obj.say()
// cpp
```

obj.say()使用箭头函数定义，Javascript引擎的处理方法是，先在全局空间生成这个箭头函数，然后复制给say，这导致箭头函数内部的this指向全局对象。所以输出的是"**cpp**"

等价于

```js
name = 'cpp'
say = ()=>console.log(this.name)
()=>console.log(this.name)
const obj = {
    name: 'cjz',
    say:say
}
obj.say()
// cpp
```

**由于上面这个原因，对象的属性建议使用传统的写法定义，不要用箭头函数定义。**

+ 第二个场合是需要动态`this`的时候，也不应使用箭头函数。

```javascript
var button = document.getElementById('press');
button.addEventListener('click', () => {
  this.classList.toggle('on');
});
```

上面代码运行时，点击按钮会报错，因为`button`的监听函数是一个箭头函数，导致里面的`this`就是全局对象。如果改成普通函数，`this`就会动态指向被点击的按钮对象。





**尾调用**

+ 就是指某个函数的最后一步是调用另一个函数。

```javascript
function f(x){
  return g(x);
```

+ 以下三种情况，都不属于尾调用。

```javascript
// 情况一
function f(x){
  let y = g(x);
  return y;
}

// 情况二
function f(x){
  return g(x) + 1;
}

// 情况三
function f(x){
  g(x);
}
```

上面代码中，情况一是调用函数`g`之后，还有赋值操作，所以不属于尾调用，即使语义完全一样。情况二也属于调用后还有操作，即使写在一行内。情况三等同于下面的代码。

```javascript
function f(x){
  g(x);
  return undefined;
}
```

+ 尾调用不一定出现在函数尾部，只要是最后一步操作即可。

```javascript
function f(x) {
  if (x > 0) {
    return m(x)
  }
  return n(x);
}
```

```javascript
function f() {
  let m = 1;
  let n = 2;
  return g(m + n);
}
f();

// 等同于
function f() {
  return g(3);
}
f();

// 等同于
g(3);
```

上面代码中，如果函数`g`不是尾调用，函数`f`就需要保存内部变量`m`和`n`的值、`g`的调用位置等信息。但由于调用`g`之后，函数`f`就结束了，所以执行到最后一步，完全可以删除`f(x)`的调用帧，只保留`g(3)`的调用帧。

这就叫做“尾调用优化”（Tail call optimization），即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。

注意，只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”。

```javascript
function addOne(a){
  var one = 1;
  function inner(b){
    return b + one;
  }
  return inner(a);
}
```

上面的函数不会进行尾调用优化，因为内层函数`inner`用到了外层函数`addOne`的内部变量`one`。

**尾递归**

函数调用自身，称为递归。如果尾调用自身，就称为尾递归。

#### toString()方法

`toString()`方法返回函数代码本身，以前会省略注释和空格

修改后的`toString()`方法，明确要求返回一模一样的原始代码。

```javascript
function /* foo comment */ foo () {}

foo.toString()
// "function /* foo comment */ foo () {}"
```

## 扩展运算符

**注意**：只有函数调用的时候，扩展运算符才可以放在圆括号中，否则会报错

**扩展运算符的应用**

1. **复制数组**：数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

直接赋值:**b** 并不是 **a** 的克隆，而是指向同一份数据的另一个指针。修改**b**，会直接导致 **a** 的变化。

```js
const a = [1,2]
const b = a
b[0] = 2
console.log(b) // [2, 2]
console.log(a) // [2, 2]
```



使用扩展运算符

```js 
const a = [1,2]
const b = [...a]
console.log(b) // [1, 2]
b[0] = 2
console.log(b) // [2, 2]
console.log(a) // [1, 2]
```

2. **合并数组**

```js
// 这两种方法都是浅拷贝
const arr1 = ['cpp', 'clg']
const arr2 = ['cjz']
const arr3 = ['lg', 'clm']
// ES5的合并
arr1.concat(arr2, arr3）
// ["cpp", "clg", "cjz", "lg", "clm"]     

            
// 扩展运算符的合并
[...arr1, ...arr2, ...arr3]
// ["cpp", "clg", "cjz", "lg", "clm"]           
```

3. **与解构赋值结合**

如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错

4. **字符串**

扩展运算符还可以将字符串转为真正的数组。

```js
[...'cpp']
// ["c", "p", "p"]

```

**注意**：凡是涉及到操作四个字节的 Unicode 字符的函数，都有这个问题。因此，最好都用扩展运算符改写。

5. **实现了Iterator接口的对象**

任何定义了遍历器接口的对象，都可以用扩展运算符转为真正的数组。

```js
const nodeList = document.querySelectorAll('div')
const arr = [...nodeList]
// arr变为数组了
```

6. **Map 和 Set 结构，Generator 函数**

+ 扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。

7. **Array.from()**

`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

和扩展运算符比较:

+ 扩展运算符背后调用的是遍历器接口，如果一个对象没有部署这个接口，就无法转换
+ Array.from还支持类似数组的对象，所谓类似数组，本质特征只有一点，即必须有length属性。
+ 因此，任何有length属性的对象，都可以通过Array.from方法转为数组，而此时扩展运算符就无法转换

```js
Array.from({length: 4})
//[undefined, undefined, undefined, undefined]
```

+ 对于还没有部署该方法的浏览器，可以用`Array.prototype.slice`方法替代

**Array.from()**还接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值返回新的数组。

```javascript
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```

**将数组中布尔值为false的成员转换为0**

```js
Array.from([1, ,2, ,3],(n) => n || 0)
//1, 0, 2, 0, 3]
```

**另一个例子是返回各种数据的类型**

```js
function typeOf() {
    return Array.from(arguments, value => typeof value)
}
typeOf(null, [], NaN)
// ["object", "object", "number"]
```

```javascript
Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
```



```js
Array.from({length: 3},() => 'cpp')
// ["cpp", "cpp", "cpp"]
```



+ 上面代码中，`Array.from`的第一个参数指定了第二个参数运行的次数。这种特性可以让该方法的用法变得非常灵活。

`Array.from()`的另一个应用是，将字符串转为数组，然后返回字符串的长度

```js
Array.from('cpp i love you')
// ["c", "p", "p", " ", "i", " ", "l", "o", "v", "e", " ", "y", "o", "u"]
Array.from('cpp i love you').length
// 14
```

##### Array.of()

`Array.of`总是返回参数值组成的数组。如果没有参数，就返回一个空数组。

```js
Array.of()
// []
Array.of(undefined)
// [undefined]
Array.of(3)
// [3]
Array.of(1,2)
// [1, 2]
```

##### find()和findIndex()

+ 数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找到第一个返回值为true的成员，然后返回该成员。如果没有，返回undefined。
+ `find`方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

+ 数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。

+ 这两个方法都可以接受第二个参数，用来绑定回调函数的`this`对象。

+ 这两个方法都可以发现`NaN`，弥补了数组的`indexOf`方法的不足。

x_x

```js
const lg = {
    name: 'cpp',
    age: 22
}

const lgAge = [11,12,13,22,2].find(item => item === this.age, lg)


console.log(lgAge)
```

```js
[NaN].indexOf(NaN)
// -1
[NaN].findIndex(i => Object.is(NaN,i))
// 0
```

#### fill()

`fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```javascript
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

#### 数组的entries(), keys(), values()

+ ES6 提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。

+ 它们都返回一个遍历器对象,可以用`for...of`循环进行遍历
+ 唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历

#### 数组实例的includes()

`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似

#### 数组实例的flat(), flatMap()

+ `flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1。

```javascript
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
```

+ `flatMap()`方法对原数组的每个成员执行一个函数（相当于执行`Array.prototype.map()`），然后对返回值组成的数组执行`flat()`方法。该方法返回一个新数组，不改变原数组。

```javascript
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```

**注意**：flatMap()只能展开一层

- `forEach()`, `filter()`, `reduce()`, `every()` 和`some()`都会跳过空位。
- `map()`会跳过空位，但会保留这个值
- `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

**ES6明确将空位转为undefined**

**Array.from方法会将数组的空位，转为undefined**

```javascript
Array.from(['a',,'b'])
// [ "a", undefined, "b" ]
```

**扩展运算符（`...`）也会将空位转为`undefined**`

**`for...of`循环也会遍历空位。**

```javascript
let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1
```

`entries()`、`keys()`、`values()`、`find()`和`findIndex()`会将空位处理成`undefined`。



## 对象的扩展



#### 对象的扩展运算符



解构赋值必须是最后一个参数，否则会报错。

扩展运算符的解构赋值，不能复制继承自原型对象的属性。

+ 由于数组是特殊的对象，所以对象的扩展运算符也可以用于数组。

```js
const lg = {...['cpp','cjz','clg']}

console.log(lg)
// {0: "cpp", 1: "cjz", 2: "clg"}
```

+ 如果扩展运算符后面是字符串，它会自动转成一个类似数组的对象，因此返回的不是空对象。

```js
{...'cpp'}
// {0: "c", 1: "p", 2: "p"}
```

+ 对象的扩展运算符等同于使用`Object.assign()`方法

```javascript
let aClone = { ...a };
// 等同于
let aClone = Object.assign({}, a);
```

+ 上面的例子只是拷贝了对象实例的属性，如果想完整克隆一个对象，还拷贝对象原型的属性，可以采用下面的写法。

```javascript
// 写法二
const clone2 = Object.assign(
  Object.create(Object.getPrototypeOf(obj)),
  obj
);
```

##### 扩展运算符可以用于合并两个对象

x_x这个好像后者覆盖前者了

```js
a = {
    name: 'cpp',
    age: 2
}
// {name: "cpp", age: 2}
b = {
    name: 'wqj',
    age: 3
}
// {name: "wqj", age: 3}
const ab = {...a,...b}
ab
// {name: "wqj", age: 3}
```

合并

```js
const a  = {
    name: 'cpp',
    age: 2
}
const b = {
    love: 'game',
    lover: 'wqj'
}
const ab = {...a,...b}
ab
// {name: "cpp", age: 2, love: "game", lover: "wqj"}
```

如果用户自动定义的属性，放在扩展运算符后面，则扩展运算符内部的同名·属性会被覆盖掉

#### 链判断运算符

编程实务中，如果读取对象内部的某个属性，往往需要判断一下该对象是否存在

```javascript
// 正确的写法
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
```

层层判断非常麻烦，因此 [ES2020](https://github.com/tc39/proposal-optional-chaining) 引入了“链判断运算符”（optional chaining operator）`?.`，简化上面的写法。

```javascript
const firstName = message?.body?.user?.firstName || 'default';
const fooValue = myForm.querySelector('input[name=foo]')?.value
```

+ 如果某个属性的值是`null`或`undefined`，有时候需要为它们指定默认值。常见做法是通过`||`运算符指定默认值。

**注意**：属性的值为空字符串或false或0，默认值也会生效。

+ 为了避免这种情况，[ES2020](https://github.com/tc39/proposal-nullish-coalescing) 引入了一个新的 Null 判断运算符`??`。它的行为类似`||`，但是只有运算符左侧的值为`null`或`undefined`时，才会返回右侧的值。

**注意**：默认值只有在左侧属性值为null或undefined时，才会生效。

## 对象的新增方法

#### 1. Object.is()

`Object.is`它用来比较两个两个值是否严格相等。与严格比较运算符(===)的行为基本一致。

不同之处有两个：一是+0不等于-0，二是NaN等于自身。

```js
+0 === -0
// true
NaN === NaN
// false
Object.is(+0,-0)
// false
Object.is(NaN,NaN)
// true
```

#### 2. Object.assign()

`Object.assign()`方法用于对象的合并，将源对象的所有可枚举属性，复制到目标对象。

```js
const lginfo = {
    name: 'cpp',
    age: 2
}
const lglove = {
    love: 'game',
    lover: 'wqj'
}
const lg = {}
Object.assign(lg, lginfo, lglove)
// {name: "cpp", age: 2, love: "game", lover: "wqj"}
console.log(lg)
// {name: "cpp", age: 2, love: "game", lover: "wqj"}
```

`Object.assign()`方法的第一个参数是目标对象，后面的参数都是源对象

**注意**:如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

**重点注意**：

1. **浅拷贝**

`Object.assign()`方法实行的是浅拷贝，而不是深拷贝，也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```js
const lg = {love: {
                    game: '马里奥',
                    lover: 'wqj'
                   }
            }
const copyLg = Object.assign({}, lg)
copyLg.love.game = '动物森友会' 
console.log(copyLg.love)
// {game: "动物森友会", lover: "wqj"}
console.log(lg.love)
// {game: "动物森友会", lover: "wqj"}
```

2. **同名属性的替换**

```js
const lg =  {
    name: 'cpp',
    age: 2
}
const targetLg = {
    name: 'cjz',
    age: 22
}
Object.assign(targetLg, lg)

console.log(targetLg)
// {name: "cpp", age: 2}age: 2name: "cpp"__proto__: Object
```

3. **数组的处理**

**Object.assign()**可以用来处理数组，但是会把数组视为对象。

```js
Object.assign(['haha', 'lala', 'lg'],['cpp', 'clg'])
// ["cpp", "clg", "lg"]
```

`Object.assign()`把数组视为属性名为 0、1、2 的对象

4. **取值函数的处理**





#### 常见用途

1. **为对象添加属性**



2. **为对象添加方法**



3. **克隆对象**



4. **合并对象**



#### Object.getOwnPropertyDescriptors()

ES5 的`Object.getOwnPropertyDescriptor()`方法会返回某个对象属性的描述对象（descriptor）。

ES2017 引入了`Object.getOwnPropertyDescriptors()`方法，返回指定对象所有自身属性（非继承属性）的描述对象。

```js
const lg = {
    name: 'cpp',
    age: 2,
    get gender() {
        return '男'
    }
}
Object.getOwnPropertyDescriptors(lg)
// {name: {…}, age: {…}, gender: {…}}
// age: {value: 2, writable: true, enumerable: true, configurable: true}
// gender: {set: undefined, enumerable: true, configurable: true, get: ƒ}
// name: {value: "cpp", writable: true, enumerable: true, configurable: true}
```

```js
// assign无法正确拷贝get属性和set属性
Object.getOwnPropertyDescriptors(Object.assign({},lg))
// age: {value: 2, writable: true, enumerable: true, configurable: true}
// gender: {value: "男", writable: true, enumerable: true, configurable: true}
// name: {value: "cpp", writable: true, enumerable: true, configurable: true}
```





```js
const copyLg = (target, source) => Object.defineProperties(target, Object.getOwnPropertyDescriptors(source))
copyLg({}, lg)

```

`Object.setPrototypeOf()`（写操作）、`Object.getPrototypeOf()`（读操作）、`Object.create()`（生成操作）代替。



+ `Object.fromEntries()`方法是`Object.entries()`的逆操作，用于将一个键值对数组转为对象

```js
Object.fromEntries([
    ['name', 'cpp'],
    ['age', 2]
])
{name: "cpp", age: 2}
```

## Symbol

```js
const lg = Symbol()
typeof(lg)
// "symbol"
```

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述

```js
const lg = Symbol('cpp')
lg.description
// "cpp"
```

**注意**：Symbol值作为对象属性名时，不能用点运算符



## Set和Map数据结构

#### 1. Set基本用法

+ Set本身是一个构造函数，用来生成Set数据结构，它类似于数组，但是成员的值都是唯一的，没有重复的值。



```js
// 数组去重
[...new Set(array)]

// 去除重复字符串
[...new Set('ababbc')].join('')
// "abc"
```



Set 结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

```javascript
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```

+ 向 Set 加入值的时候，不会发生类型转换，所以`5`和`"5"`是两个不同的值。

```js
let set = new Set()
set.add(5)
// {5}
set.add('5')
// {5, "5"}
set.size
// 2
```

+ 向 Set 加入值时认为`NaN`等于自身

```js
// 添加了两个NaN,但是只会加入一个。这表明在Set内，两个NaN是相等的。
const set = new Set()
const a = NaN
const b = NaN
set.add(a)
set.add(b)
set
// {NaN}
```

+ 另外，两个对象总是不相等的

```js
// 下面代码表示，两个空对象不相等，所以它们被视为两个值
let set = new Set();

set.add({});
set.size // 1

set.add({});
set.size // 2
```

+ Array.from方法可以将Set结构转为数组

```js
const set = new Set([1, 2, 3, 4, 5, 5, 5, 5])
set
// {1, 2, 3, 4, 5}
Array.from(set)
// [1, 2, 3, 4, 5]
```

##### 遍历数组

Set结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员



由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

```js
const set = new Set(['cpp','clg','cjz','lg'])
for(let item of set.keys()) {
    console.log(item)
} 
// cpp
// clg
// cjz
// lg

for(let item of set.values()) {
    console.log(item)
}
// cpp
// clg
// cjz
// lg

for(let item of set.entries()) {
    console.log(item)
}
// ["cpp", "cpp"]
// ["clg", "clg"]
// ["cjz", "cjz"]
// ["lg", "lg"]
```

+ Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的`values`方法。

这意味着，可以省略`values`方法，直接用`for...of`循环遍历 Set。

```js
for(let item of set) {
    console.log(item)
}
// cpp
// clg
// cjz
// lg

```

**forEach()**

Set结构的实例与数组一样，也拥有forEach方法，用于对每个成员执行某种操作，没有返回值。

```js
const set = new Set(['cpp','clg','cjz','lg'])

set.forEach(item => {
    console.log(item)
})
// cpp
// clg
// cjz
// lg
```

forEach()方法还可以有第二个参数，表示绑定处理函数内部的this对象。

如果想在遍历操作中，同步改变原来的 Set 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用`Array.from`方法。

```javascript
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```

#### 2. WeakSet

WeakSet结构与Se类似，也是不重复的值的集合。但是与Set有两个区别。

**(1). **WeakSet的成员只能是对象，而不能是其他类型的值。

**(2.)**WeakSet中的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中。

WeakSet 里面的引用，都不计入垃圾回收机制，WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。

```javascript
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}
```

`a`是一个数组，它有两个成员，也都是数组。将`a`作为 WeakSet 构造函数的参数，`a`的成员会自动成为 WeakSet 的成员。

WeakSet 结构有以下三个方法。

- **WeakSet.prototype.add(value)**：向 WeakSet 实例添加一个新成员。
- **WeakSet.prototype.delete(value)**：清除 WeakSet 实例的指定成员。
- **WeakSet.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。

```javascript
const ws = new WeakSet();
const obj = {};
const foo = {};

ws.add(window);
ws.add(obj);

ws.has(window); // true
ws.has(foo);    // false

ws.delete(window);
ws.has(window);    // false
```

WeakSet 没有`size`属性，没有办法遍历它的成员。

```javascript
ws.size // undefined
ws.forEach // undefined

ws.forEach(function(item){ console.log('WeakSet has ' + item)})
// TypeError: undefined is not a function
```

#### 3. Map

JavaScript的对象，本质上是键值对的集合，但是传统上只能用字符串当作键。

为了解决这个问题，ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```js
const lg = [
    ['name', 'cpp'],
    ['age',2]
]
const map = new Map(lg)
// {"name" => "cpp", "age" => 2}
map.get('name')
// "cpp"
map.get('age')
// 2
```

如果对同一个键多次赋值，后面的值将覆盖前面的值。

```js
map
.set('love', 'game')
.set('love', 'wqj')
// {"name" => "cpp", "age" => 2, "love" => "wqj"}

map.get('love')
// "wqj"
```

Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。

##### Map与其他数据结构的互相转换

1. **Map转为数组**

Map转为数组最常用的方法就是使用扩展运算符。

2. **数组转为Map**



3. **Map转为对象**





## 遍历器接口Iterator和for .... of 循环



Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环,Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环当使用for... of循环遍历某种数据结构时，该循环会自动去寻找Iteator。

一种数据结构只要部署了Iterator接口，我们就称这种数据结构是“可遍历的”

ES6 规定，默认的 Iterator 接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”



原生具备 Iterator 接口的数据结构如下。

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象



**数组的`Symbol.iterator`属性**

```js
const arr = ['cpp','lg']
const iter = arr[Symbol.iterator]()
iter.next()
// {value: "cpp", done: false}
iter.next()
// {value: "lg", done: false}
iter.next()
// {value: undefined, done: true}
```

对于类似数组的对象（存在数值键名和`length`属性），部署 Iterator 接口，有一个简便方法，就是`Symbol.iterator`方法直接引用数组的 Iterator 接口。

#### 调用Iterator接口的场合

1. 解构赋值



2. 扩展运算符



3. yield*

`yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。

4. 其他场合

由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口。下面是一些例子。

- for...of
- Array.from()
- Map(), Set(), WeakMap(), WeakSet()（比如`new Map([['a',1],['b',2]])`）
- Promise.all()
- Promise.race()

一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有 iterator 接口，就可以用`for...of`循环遍历它的成员。也就是说，`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法。

`for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。

数组：

```javascript
const arr = ['red', 'green', 'blue'];

for(let v of arr) {
  console.log(v); // red green blue
}

const obj = {};
obj[Symbol.iterator] = arr[Symbol.iterator].bind(arr);

for(let v of obj) {
  console.log(v); // red green blue
}
```

+ `for...in`循环读取键名
+ `for...of`循环读取键值

如果要通过`for...of`循环，获取数组的索引，可以借助数组实例的`entries`方法和`keys`方法

```js
for(let i of lg.entries()) {
    console.log(i)
}
// [0, "cpp"]
// [1, "cjz"]
// [2, "lg"]
```

`for...of`循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。这一点跟`for...in`循环也不一样。

```javascript
let arr = [3, 5, 7];
arr.foo = 'hello';

for (let i in arr) {
  console.log(i); // "0", "1", "2", "foo"
}

for (let i of arr) {
  console.log(i); //  "3", "5", "7"
}
```

`for...of`循环不会返回数组`arr`的`foo`属性。

并不是所有类似数组的对象都具有 Iterator 接口，一个简便的解决方法，就是使用`Array.from`方法将其转为数组。

## Generator函数

### 一、起步

+ Generator函数是一个状态机，封装了多个内部状态。
+ 执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态。
+ 形式上，Generator函数是一个普通函数，但是有两个特征
  + 一是，`function`关键字，与函数名之间有一个星号。
  + 二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）

```js
function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
}

const hw = helloWorldGenerator();
```

+ 定义了一个`Generator`函数，它内部有两个`yield`表达式（`hello`和`world`），即该函数有三个状态：hello、world、和return语句（结束执行）

+ 调用Generator函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象。也就是遍历器对象。
+ 必须调用遍历器对象的`next`方法，使得指针移向下一个状态，也就是说每次调用`next`方法，内部指针就从函数头部上一次停下来的地方开始执行，直到遇到下一个`yield`表达式为止。
+ **换言之，Generator函数是分段执行的，`yield`表达式是暂停执行的标记，而`next`方法可以恢复执行。**

```js
hw.next()
// {value: "hello", done: false}
hw.next()
// {value: "world", done: false}
hw.next()
// {value: "ending", done: true}
hw.next()
// {value: undefined, done: true}
```

`done`属性的值为`false`时，表示遍历还没结束。

**总结：**调用Generator函数，返回一个遍历器对象，代表Generator函数内部的指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。

+ `value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值，

+ `done`属性是一个布尔值，表示是否遍历结束。

### 二、yield表达式

+ 由于Generator函数返回的遍历器对象，只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数，`yield`表达式就是暂停标志。

遍历器对象的`next`方法的运行逻辑如下。

（1）遇到`yield`表达式，就暂停执行后面的操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。

（2）下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。

（3）如果没有再遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值。

（4）如果该函数没有`return`语句，则返回的对象的`value`属性值为`undefined`



+ Generator可以不用`yield`表达式，这时就成了一个单纯的暂缓执行函数。

```js

function* f() {
    console.log('执行了！')
}
const generator = f()
setTimeout(function () {
    generator.next()
},2000)   // 两秒后next(),执行函数，输出结果

// 执行了      
```

上面代码中，函数`f`如果是普通函数，在为变量`generator`赋值时就会执行。但是，函数`f`是一个 Generator 函数，就变成只有调用`next`方法时，函数`f`才会执行。

+ 任意一个`Symbal.iterator`方法，等于该对象的遍历器生成函数，调用该函数会返回该对象的一个遍历器对象
  + 由于Generator函数就是遍历器生成函数，因此可以把Generator赋值给对象的`Symbol.iterator`属性，从而使得该对象具有Iterator借口。

  ```js
  // Generator 函数赋值给Symbol.iterator属性，
  // 从而使得myIterable对象具有了 Iterator 接口，可以被...运算符遍历了。
  const myIterable = {}
  myIterator[Symbol.iterator] = function* () {
      yield 1;
      yield 2;
      yield 3;
  }
  console.log([...myIterable])
  // [1, 2, 3]
  ```

  + Generator函数执行后，返回一个遍历器对象。该对象本身也具有`Symbal.iterator`属性，执行后返回自身。

  ```js
  function* gen() {
      // some ode
  }
  const g = gen()
  g[Symbol.iterator]() === g 
  // true
  ```

  gen是一个Generator函数，调用它会生成一个遍历器对象g，它的`Symbol.itereator`属性，也是一个遍历器生成函数，执行后返回它自己。

### 三、 next方法的参数

`yield`表达式本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当作上一个`yield`表达式的返回值。

x_x

```js
function* f() {
    for(let i = 0;true; i++) {
        const reset = yield i;
        console.log(reset)
        if(reset) {
            i = -1;
        }
    }
}
const g = f()
g.next()
// {value: 0, done: false}
g.next()
// undefined
// {value: 1, done: false}
g.next()
// undefined
// {value: 2, done: false}
g.next(true)
// true
// {value: 0, done: false}
g.next()
// undefined
// {value: 1, done: false}
```

+ Generator函数从暂停状态恢复运行，它的上下文状态（context）是不变的。通过`next`方法的参数，就有办法在Generator函数运行之后，继续向函数体内部注入值。也就是说，可以在Generator函数开始运行的不同阶段，从外部向内注入不同的值。从而调整函数行为。

```js
function* foo(x) {
    const y = 2 * (yield(x + 1))
    console.log(y)
    const z = yield (y/3)
    console.log(z)
    return (x + y +z)
}

const a = foo(5)
a.next()
// {value: 6, done: false}
a.next(12)
// 24
// {value: 8, done: false}
a.next(24)
// 24
// {value: 53, done: true}
```

```js
function* foo(x) {
    const y = 2 * (yield(x + 1))
    console.log(x)
    console.log(y)
    const z = yield (y/3)
    console.log(x)
    console.log(y)
    console.log(z)
    console.log(x + y + z)
    return (x + y +z)
}
const a = foo(5)
a.next(3)   // 第一次next()传参无效
// {value: 6, done: false}
a.next(3)
// 5
// 6
// {value: 2, done: false}
a.next(3)
// 5
// 6
// 3
// 14
// {value: 14, done: true}done: truevalue: 14__proto__: Object
```

**注意**:由于`next`方法的参数表示上一个`yield`表达式的返回值，所以在第一次使用`next`方法时，传递参数是无效的

#### 三、 for...of循环

`for...of`循环可以自动遍历Generator函数运行时生成的`Iterator`对象，

且此时不再需要调用next方法。

```js
function* foo() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
    return 6;
}
for(let v of foo()) {
    console.log(v)
}
// 1
// 2
// 3
// 4
// 5
```

上面代码使用`for...of`循环，依次显示 5 个`yield`表达式的值。这里需要注意，一旦`next`方法的返回对象的`done`属性为`true`，`for...of`循环就会中止，且不包含该返回对象，所以上面代码的`return`语句返回的`6`，不包括在`for...of`循环之中。

#### 1. next()



#### 2. Generator.prototype.throw()

一旦 Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。如果此后还调用`next`方法，将返回一个`value`属性等于`undefined`、`done`属性等于`true`的对象，即 JavaScript 引擎认为这个 Generator 已经运行结束

#### 3. Generator.prototype.return()

Generator 函数返回的遍历器对象，还有一个`return()`方法，可以返回给定的值，并且终结遍历 Generator 函数。

#### next()、throw()、return() 的共同点

`next()`,`throw()`,`return()`这三个方法本质上是同一件事，它们的作用都是让Generator函数恢复执行，并且使用不同的语句替换`yield`表达式

`next()`是将`yield`表达式替换成一个值。

`throw()`是将`yield`表达式替换成一个值。

`return()`是将`yield`表达式替换成一个`return`语句

#### 4. yield*表达式



+ `Thunk`函数是自动执行Generator函数的一种方法

编译器的“传名调用”实现，往往是将参数放到一个临时函数之中，再将这个临时函数传入函数体。这个临时函数就叫做 Thunk 函数。

```javascript
function f(m) {
  return m * 2;
}

f(x + 5);

// 等同于

var thunk = function () {
  return x + 5;
};

function f(thunk) {
  return thunk() * 2;
}
```

## async和await

`await`命令，正常情况下，`await`命令后面是一个Promise对象，返回该对象的结果，如果不是Promsie对象，就直接返回对应的值。

```javascript
async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123
```

另一种情况是，`await`命令后面是一个`thenable`对象（即定义了`then`方法的对象），那么`await`会将其等同于 Promise 对象。

+ 任何一个`await`语句后面的 Promise 对象变为`reject`状态，那么整个`async`函数都会中断执行。

```javascript
async function f() {
  await Promise.reject('出错了');
  await Promise.resolve('hello world'); // 不会执行
}
```

解决方法：

方法一：

可以将第一个`await`放在`try...catch`结构里面，这样不管这个异步操作是否成功，第二个`await`都会执行。

```javascript
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world
```

方法二：

是`await`后面的 Promise 对象再跟一个`catch`方法，处理前面可能出现的错误。

```javascript
async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// 出错了
// hello world
```

### 错误处理

如果`await`后面的异步操作出错，那么等同于`async`函数返回的 Promise 对象被`reject`。

```javascript
async function f() {
  await new Promise(function (resolve, reject) {
    throw new Error('出错了');
  });
}

f()
.then(v => console.log(v))
.catch(e => console.log(e))
// Error：出错了
```

上面代码中，`async`函数`f`执行后，`await`后面的 Promise 对象会抛出一个错误对象，导致`catch`方法的回调函数被调用，它的参数就是抛出的错误对象。具体的执行机制，可以参考后文的“async 函数的实现原理”。

防止出错的方法，也是将其放在`try...catch`代码块之中。

```javascript
async function f() {
  try {
    await new Promise(function (resolve, reject) {
      throw new Error('出错了');
    });
  } catch(e) {
  }
  return await('hello world');
}
```

如果有多个`await`命令，可以统一放在`try...catch`结构中。

```javascript
async function main() {
  try {
    const val1 = await firstStep();
    const val2 = await secondStep(val1);
    const val3 = await thirdStep(val1, val2);

    console.log('Final: ', val3);
  }
  catch (err) {
    console.error(err);
  }
}
```

### 使用注意点

第一点，前面已经说过，`await`命令后面的`Promise`对象，运行结果可能是`rejected`，所以最好把`await`命令放在`try...catch`代码块中。

```javascript
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}

// 另一种写法

async function myFunction() {
  await somethingThatReturnsAPromise()
  .catch(function (err) {
    console.log(err);
  });
}
```

x_x

```javascript
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

上面两种写法，`getFoo`和`getBar`都是同时触发，这样就会缩短程序的执行时间。