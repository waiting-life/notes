

## 类型

### Object

1. **isPrototypeOf()**

**`isPrototypeOf()`** 方法用于测试一个对象是否存在于另一个对象的原型链上。





2. **Object.create()**

**`Object.create()`**方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 （请打开浏览器控制台以查看运行结果。





3. **Object.assign()**

**`Object.assign()`** 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);

target
// {a: 1, b: 4, c: 5}
returnedTarget
// {a: 1, b: 4, c: 5}
```





4. **Object.defineProperty()**

**`Object.defineProperty()`** 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。





5. **Object.defineProperties()**

**`Object.defineProperties()`** 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。



6. **Object.entries()**

**`Object.entries()`**`方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。





7. **Object.freeze()**

**`Object.freeze()`** 方法可以**冻结**一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。`freeze()` 返回和传入的参数相同的对象。



8. **Object.fromEntries()**

**`Object.fromEntries()`**方法把键值对列表转换为一个对象。

```js
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);
const obj = Object.fromEntries(entries);

console.log(obj);
// {foo: 'bar', baz: 42}
```



9. **Object.getOwnPropertyDescriptor()**

**`Object.getOwnPropertyDescriptor()`** 方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

```js
const person = {
    name: 'cpp'
}
const descriptor1 = Object.getOwnPropertyDescriptor(person, 'name')

console.log(descriptor1)
// {value: 'cpp', writable: true, enumerable: true, configurable: true}
console.log(descriptor1.value)
// cpp
console.log(descriptor1.configurable)
// true
console.log(descriptor1.writable)
// true
console.log(descriptor1.enumerable)
// true
```



10. **Object.getOwnPropertyDescriptors()**

**`Object.getOwnPropertyDescriptors()`** 方法用来获取一个对象的所有自身属性的描述符。

```js
```





11. **Object.getPrototypeOf()**

**`Object.getPrototypeOf()`** 方法返回指定对象的原型（内部`[[Prototype]]`属性的值）。

```js
const prototype1 = {}
const object1 = Object.create(prototype1)
console.log(Object.getPrototypeOf(object1) === prototype1)
VM1965:3 true
```



12. **Object.getOwnPropertyNames()**

**`Object.getOwnPropertyNames()`**方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。

```js
const person = {
    name: 'cpp',
    age: 22
}
const result = Object.getOwnPropertyNames(person)
result
// ['name', 'age']
```



13. **Object.getOwnPropertySymbols()**

**`Object.getOwnPropertySymbols()`**方法返回一个给定对象自身的所有 Symbol 属性的数组。







14. **hasOwnProperty()**

**`hasOwnProperty()`**方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

```js
const person = {
    name: 'cpp',
    age: 22
}

person.hasOwnProperty('name')
// true
person.hasOwnProperty('age')
// true
person.hasOwnProperty('height')
// false
```



15. **Object.is()**

`**Object.is()**` 方法判断两个值是否为[同一个值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness)。

`Object.is()` 方法判断两个值是否为[同一个值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness)。如果满足以下条件则两个值相等:

- 都是 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)
- 都是 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)
- 都是 `true` 或 `false`
- 都是相同长度的字符串且相同字符按相同顺序排列
- 都是相同对象（意味着每个对象有同一个引用）
- 都是数字且
  - 都是 `+0`
  - 都是 `-0`
  - 都是 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)
  - 或都是非零而且非 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 且为同一个值

与[`==` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators) 运算*不同。* `==` 运算符在判断相等前对两边的变量(如果它们不是同一类型) 进行强制转换 (这种行为的结果会将 `"" == false` 判断为 `true`), 而 `Object.is`不会强制转换两边的值。

与[`===` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators) 运算也不相同。 `===` 运算符 (也包括 `==` 运算符) 将数字 `-0` 和 `+0` 视为相等 ，而将[`Number.NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/NaN) 与[`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)视为不相等.



16. **Object.isExtensible()**

**`Object.isExtensible()`**方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。



```js
// 新对象默认是可扩展的.
var empty = {};
Object.isExtensible(empty); // === true

// ...可以变的不可扩展.
Object.preventExtensions(empty);
Object.isExtensible(empty); // === false

// 密封对象是不可扩展的.
var sealed = Object.seal({});
Object.isExtensible(sealed); // === false

// 冻结对象也是不可扩展.
var frozen = Object.freeze({});
Object.isExtensible(frozen); // === false
```



17. **Object.isFrozen()**

**`Object.isFrozen()`**方法判断一个对象是否被[冻结](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)。





18. **Object.isSealed()**

**`Object.isSealed()`** 方法判断一个对象是否被密封。



19. **Object.keys()**

**`Object.keys()`** 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。





20. **Object.preventExtensions()**

**`Object.preventExtensions()`**方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。





21 . **Object.seal()**

**`Object.seal()`**方法封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要原来是可写的就可以改变。



22. **toLocaleString()**

**`toLocaleString()`** 方法返回一个该对象的字符串表示。此方法被用于派生对象为了特定语言环境的目的（locale-specific purposes）而重载使用。



23. **valueOf()**

**`valueOf()`**方法返回指定对象的原始值。

```js
person.valueOf()
// {name: 'cpp', age: 22}
```



24. **Object.values()**

**`Object.values()`**方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用[`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

```js
Object.values(person)
// ['cpp', 22]
```





**其他**

```js
person.toLocaleString()
'[object Object]'
person.toString()
'[object Object]
```

#### 基本包装类

+ 基本数据类型的值不是对象，因而从逻辑上讲它们不应该有方法或者属性，然而事实并不是我们所想的那样

+ **为了便于操作基本数据类型的值**，JavaScript 中的原始数据类型的值会在后台隐式地被包装为对象，从而引出了**基本包装类型（primitive wrapper type）**这个概念。
+ **除了 null 和 undefined，所有的原始值都有等价的、由对象包装原始值的形式表达**，取而代之，`null` 和 `undefined` 常被当作一个全局对象的全局属性来使用。

```js
const str = "hello world";
str.length;              // 11
str.toUpperCase(); 			 // HELLO WORLD
```

其实后台会自动完成下列的处理：

- 执行到第二行时：
  - 创建 String 类型的一个实例；
  - 在实例上调用指定的**属性**；
  - 销毁这个实例；
- 执行到第三行时：
  - 创建 String 类型的一个实例；
  - 在实例上调用指定的**方法**；
  - 销毁这个实例；

上面的步骤想象成下列 ECMAScript 代码：

```js
// 执行到第二行时
const str = new String("hello world");
str.length;
str = null;

// 执行到第三行时
const str = new String("hello world");
str.toUpperCase();
str = null;
```



##### 引用类型与基本包装类型的区别

+ 引用类型与基本包装类型的**主要区别就是对象的生存期。**

使用 `new` 操作符创建的引用类型的实例，在执行流离开当前作用域之前，会一直保存在**堆内存**中。而后台自动创建的基本包装类型的对象，则只存在一行代码的执行瞬间，然后立即被销毁。这意味着我们不能为基本类型的值添加属性和方法。

```js
const str = 'hello'
str.color = 'red'
console.log(str.color)
// undefined
```



### Array

1. **Array.prototype.at()**

**`at()`** 方法接收一个整数值并返回该索引的项目，允许正数和负数。负整数从数组中的最后一个项目开始倒数。

```js
const arr = [1, 2, 3, 4]
arr.at(2)
// 3
arr.at(-1)
// 4
```



2. **Array.prototype.concat()**

**`concat()`** 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```js
const arr1 = [1, 2, 3]
const arr2 = ['a', 'b']
const result = arr1.concat(arr2)
console.log(result)
//  [1, 2, 3, 'a', 'b']
```



3. **Array.prototype.copyWithin()**

**`copyWithin()`**方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。

```js
```



4. **Array.prototype.entries()**

**`entries()`** 方法返回一个新的**Array Iterator**对象，该对象包含数组中每个索引的键/值对。

```js
const arr1 = ['a', 'b', 'c', 'd', 'e'];
const iterator1 = arr1.entries()

console.log(iterator1.next().value)
// [0, 'a']
console.log(iterator1.next().value)
// [1, 'b']
console.log(iterator1.next().value)
// [2, 'c']
console.log(iterator1.next().value)
// [3, 'd']
console.log(iterator1.next().value)
// [4, 'e']
console.log(iterator1.next().value)
// undefined

```



5. **Array.prototype.every()**

**`every()`** 方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

```js
const arr1 = [1, 2, 3, 4, 5, 6]
arr1.every(item => item > 0)
// true
```

6. **Array.prototype.fill()**

**`fill()`** 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。

参数：

value：用来填充数组元素的值。

start：可选，起始索引，默认值为0。

end：可选，终止索引，默认值为 `this.length`。

```js
[1, 2, 3].fill(4)
// [4, 4, 4]
[1, 2, 3].fill(4, 2, 3)
// [1, 2, 4]
[1, 2, 3].fill(4, 0, 2)
// [4, 4, 3]
```

7. **Array.prototype.filter()**

**`filter()`**方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 





8. **Array.prototype.find()**

**`find()`**方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。





9. **Array.prototype.findIndex()**

**`findIndex()`**方法返回数组中满足提供的测试函数的第一个元素的**索引**。若没有找到对应元素则返回-1。



10. **Array.prototype.flat()**

**`flat()`** 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。



11. **Array.prototype.flatMap()**

**`flatMap()`**方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 连着深度值为1的 [flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 几乎相同，但 `flatMap` 通常在合并成一种方法的效率稍微高一些。

```js
const arr1 = [1, 2, 3, 4];
const result1 = arr1.map(x => [x * 2]);
const result2 = arr1.flatMap(x => [x * 2])
console.log(result1)
// [[2][3][6][8]]
console.log(result2)
// [2, 4, 6, 8]
```



12. **Array.from()**

**`Array.from()`** 方法对一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

```js
// 从String生成数组
Array.from('foo'); 
// ['f', 'o', 'o']


// 从Set生成数组
const set = new Set(['foo', 'bar', 'baz', 'foo']);
Array.from(set);
// ['foo', 'bar', 'baz']

...

```

13. **Array.prototype.includes()**

**`includes()`**方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 `true`，否则返回 `false`。

```js
const array1 = [1, 2, 3];
console.log(array1.includes(2));
// true
const pets = ['cat', 'dog', 'bat'];
console.log(pets.includes('cat'));
// true
console.log(pets.includes('at'));
// false
```

14. **Array.prototype.indexOf()**

**`indexOf()`**方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

```js
const arr1 = ['apple', 'banana', 'orange', 'pear']
arr1.indexOf('apple')
// 0
arr1.indexOf('grape')
// -1
```

15. **Array.isArray()**

**`Array.isArray()`** 用于确定传递的值是否是一个 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)。

```js
Array.isArray([1, 2, 3]);
// true
Array.isArray({foo: 123});
// false
Array.isArray("foobar");
// false
Array.isArray(undefined);
// false
```

16. **Array.prototype.join()**

**`join()`**方法将一个数组（或一个[类数组对象](https://developer.mozilla.org/zh-CN_docs/Web/JavaScript/Guide/Indexed_collections#working_with_array-like_objects)）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符。

```js
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// Fire,Air,Water
console.log(elements.join(''));
// FireAirWater
console.log(elements.join('-'));
// Fire-Air-Water
```

17. **Array.prototype.keys()**

 **`keys()`**方法返回一个包含数组中每个索引键的`**Array Iterator**`对象。

```js
const array1 = ['a', 'b', 'c'];
const iterator = array1.keys();
for (const key of iterator) {
  console.log(key);
}
// 0
// 1
// 2
```

18. **Array.prototype.lastIndexOf()**

**`lastIndexOf()`**方法返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 `fromIndex` 处开始。

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];
console.log(animals.lastIndexOf('Dodo'));
// 3
console.log(animals.lastIndexOf('Tiger'));
// 1
```

19. **Array.of()**

**`Array.of()`** 方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

```js
Array.of(7);       // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```

20. **Array.prototype.pop()**

**`pop()`**方法从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。

```js
```

21. **Array.prototype.reduce()**

**reducer** 函数接收4个参数:

1. Accumulator (acc) (累计器)
2. Current Value (cur) (当前值)
3. Current Index (idx) (当前索引)
4. Source Array (src) (源数组)

**`reduce()`**方法对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其结果汇总为单个返回值。

```js


```

22. **Array.prototype.reduceRight()**

**`reduceRight()`**方法接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值。

```js
const array1 = [[0, 1], [2, 3], [4, 5]].reduceRight(
  (accumulator, currentValue) => accumulator.concat(currentValue)
);

console.log(array1);
// [4, 5, 2, 3, 0, 1]
```

23. **Array.prototype.reverse()**

**`reverse()`** 方法将数组中元素的位置颠倒，并返回该数组。数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个。该方法会改变原数组。

```js
const arr1 = ['one', 'two', 'three'];
arr1.reverse()
// ['three', 'two', 'one']
```

24. **Array.prototype.shift()**

**`shift()`** 方法从数组中删除**第一个**元素，并返回该元素的值。此方法更改数组的长度。







## 原型







## 安全









## 网络协议





