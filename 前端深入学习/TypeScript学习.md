[ts学习](https://juejin.cn/post/6974713100826050591)

# TypeScript学习

### 基础类型

```ts
let a: number = 111
let b: string = '11'
let c: boolean = true 

let d: undefined = undefined
let e: null = null 

let arr: number[] = [1, 1, 2]
let arr2: Array<number> = [1, 1, 2]

type A = {
  name: string;
  age?: number;
}
interface B {
  name: string;
  flag: boolean
}

let obj: A = {name: 'aaa', age: 20}
let obj2: B = {name: 'aaa', flag: true}
let obj3: {name: string} = {name: 'aaa'}

let q: string | number | string[] = 'aaa' // 联合类型
q = 233 
q = ['aa']

let fn: (name: string) => number = function(name: number) {
  return '111'
}

let fn2: (name: string, age: number) => {name: string, age: number} = (name: string, age: number) => {
  return {
    name,
    age
  }
}

const fn3 = (name: string[]) => {

}

```
#### 布尔值



#### 数组

```tsx
const arr: number[] = [1, 2, 3, 4];

<ul>
  {arr.map((item) => (
    <li>{item}</li>
  ))}
</ul>

const arr1: Array<string> = ['cpp', 'wqj', 'xz'];

<ul>
  {arr1.map((item) => (
    <li>{item}</li>
  ))}
</ul>
```

#### 元组Tuple

+ 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同（上面定义数组的方式数组里面的每一个元素都会是同一种类型）
+ 比如你可以定义一对值分别为string和number类型的元组
+ 元组可以储存多种数据类型，但是元组长度不可变。

```tsx
let person: [string, number] = ['cpp', 22]
person = ['cjz', 2]
// person = ['cpp', 22, '男']  // error: 不能将类型“[string, number, string]”分配给类型“[string, number]”。
```



#### 枚举








#### unknown: 
1. Funknown实际上就是一个类型安全的any
2. unknown类型的变量不能直接赋值给其他变量
```ts
// 想要将unknown赋值给其他变量，可以这样
let e:unknown
let s:string
// s = e
if(typeof e === 'string') {
    s = e
}

// 或者类型断言
s = e as string
s = <string>e

```

#### void:
1. void用来表示空，以函数为例，就是表示函数的没有返回值，有返回值得时候就会报错
2. 但是返回undefined或者null不会报错
```ts
function fn(): void {
    return undefined
}
function fn1(): void {
    return null
}
```
#### never：
1. never表示永远不会返回结果
2. undefined和null也会报错
3. throw new Error()    没有返回值
```ts
function fn2(): never {
    throw new Error('出错了')
}
```



#### 变量声明


### 接口

#### 类型断言：可以告诉解析器变量的实际类型

语法：
    变量 as 类型
    <类型>变量
```ts
let s: string

```
```ts

interface SquareConfig {
    color?: string;
    width?: number;
}
function createSquare(config: SquareConfig) : { color: string; area: number } {
    let newSquare = { color: "white", area: 10 }
    if(config.color) {
        newSquare.color = config.color
    } 
    if(config.width) {
        newSquare.area = config.width * config.width
    }
    return newSquare
}

// 本来colour会报错，用了类型断言后会避开错误不报错
let mySquare = createSquare({ colour: 'black', width: 20 } as SquareConfig)
console.log(mySquare)

```


#### readonly

**readonly  VS const**
+ 最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 const，若做为属性则使用readonly


#### 额外的属性检查
```ts
interface PersonConfig{
    name: string;
    age: number;
    sex: string | number;
    job?: string;
    // 另一种避开错误的方法
    [propName: string]: any
}
function createPerson(config: PersonConfig) {
    console.log(config.name, config.age, config.sex)
    if(config.job) {
        console.log(config.job)
    }
    console.log(config.color)
} 
createPerson({ name: 'cpp', age: 2, sex: 'nan'})

```

#### 另外一种避开这种检查的方法： 将这个对象赋值给另一个变量：因为personConfig不会经过额外属性检查， 所以编译器不会报错
```ts
interface PersonConfig{
    name: string;
    age: number;
    sex: string | number;
    job?: string;
    [propName: string]: any
}
function createPerson(config: PersonConfig) {
    console.log(config.name, config.age, config.sex)
    if(config.job) {
        console.log(config.job)
    }
    console.log(config.color)
} 
createPerson({ name: 'cpp', age: 2, sex: 'nan'})

let personOptions = { name: 'cpp', age: 2, sex: 'nan', colour: "red", width: 100 };
let myPerson = createPerson(personOptions);

```

#### 函数类型

+ 接口能够描述JavaScript中对象拥有的各种各样的外形，除了描述带有属性的普通对象外，接口也可以描述函数类型

+ 为了使用接口表示函数类型，我们需要给接口定义一个调用签名。他就像是一个还有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。
```ts
interface SearchFunc {
    // 前面参数的类型，后面函数返回值的类型
    (source: string, subString: string): boolean
}
let mySearch: SearchFunc = function (source: string, subString: string) {
    let result = source.search(subString)
    console.log(result)
    return result > 1
}
console.log(mySearch('abcdef', 'e'))

```

#### 可索引的类型
```ts
// 可索引的类型
// 与使用接口描述函数类型差不多，我们也可以描述那些能够"通过索引得到"的类型。
// 可索引类型剧有一个索引签名，它描述了对象索引的类型，还有相应的索引返回值的类型
interface StringArray {
  [index: number]: string;
}
// 我们定义了StringArray接口，它具有索引签名。 这个索引签名表示了当用 number去索引StringArray时会得到string类型的返回值。
let myArray: StringArray = ['cppp', 'cjz'];
let myStr: string = myArray[0];
console.log(myStr); // cppp
```
1. TypeScript支持两种索引签名：数字和字符串。

2. 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值的字类型。

3. 这是因为当使用number来索引时，JavaScript会将它转换成string然后再去索引对象
4. 索引签名是只读的



#### 类类型

##### 实现接口

+ 接口描述了类的公共部分，而不是公共和私有两部分
+ 当一个类实现了一个接口时，只对其实例部分进行类型检查。 constructor存在于类的静态部分，所以不在检查的范围内



#### 继承接口

和类一样，接口也可以互相继承。这让我们能够从一个接口里复制成员到另一个接口里。





一个接口可以继承多个接口，创建出多个接口的合成接口





#### 混合类型



#### 接口继承类

当接口继承了一个类类型时，它会继承类的成员但不包括其实现。就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。



### 函数

```tsx
let myAdd: (baseValue: number, increment: number) => number = function (
  x: number,
  y: number,
): number {
  return x + y;
};


const A = (x: number, y: number): number => {
  return x + y;
};
```

#### 可选参数与默认参数

+ TypeScript里的每个函数参数都是必须的。

+ 可选参数必须跟在必选参数的后面
+ 在Typescript我们也可以为参数提供一个默认值，当用户没有传递参数或者传递的是undefined的时候
+ 在所有必选参数后面的带默认初始化的参数都是可选的
+ 与可选参数一样，在调用的以后可以省略，也就是说，可选参数与末尾的默认参数共享参数类型
+ 与普通可选参数不同的是，带默认值的参数可以不需要放在必须参数的后面，如果带默认值的参数出现在必须参数前面，用户必须明确的传入undefined值来获得默认值



#### 剩余参数







**上面例子里，我们定义了StringArray接口，它具有索引签名。 这个索引签名表示了当用 number去索引StringArray时会得到string类型的返回值。TypeScript支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 这是因为当使用 number来索引时，JavaScript会将它转换成string然后再去索引对象。 也就是说用 100（一个number）去索引等同于使用"100"（一个string）去索引，因此两者需要保持一致。**

#### 类类型
##### 实现接口


### 类

#### 理解private：当成员被标记成 private时，它就不能在声明它的类的外部访问
```ts
class Animal {
    private name: string
    constructor(theName: string) {
        this.name = theName
    }
}
const a = new Animal('cat')
// 会报错
console.log(a.name)
```

### 函数
#### 为函数定义类型
```ts
function add(x: number, y: number) : number {
    return x + y
}
const result = add(1, 2)
console.log(result)

function add(x: number, y: string) : string {
    return x + y
}
const result = add(1, 'a')
console.log(result)


// 只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。
let myAdd: (x: number, y: number) => number = function(x: number, y: number) {
    return x + y
}
const result1 = myAdd(1, 2)
console.log(result1)


// TypeScript里的每个函数参数都是必须的。 这不是指不能传递 null或undefined作为参数，而是说编译器检查用户是否为每个参数都传入了值
function buildName(firstName: string, lastName: string) {
    return firstName + lastName
}
let result2 = buildName('cpp')
let result3 = buildName('c', 'pp')
let result4 = buildName('c', 'pp', 'l')


// 在TypeScript里我们可以在参数名旁使用 ?实现可选参数的功能： 比如，我们想让last name是可选的
// 可选参数必须跟在必须参数后面。
function buildName2(firstName: string, lastName?: string) {
    if(lastName) {
        return firstName + lastName
    } else {
        return firstName
    }
}
let result5 = buildName2('cpp')
console.log(result5)


// 在所有必须参数后面的带默认初始化的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。 也就是说可选参数与末尾的默认参数共享参数类型。
// 与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。 如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined值来获得默认值。 例如，我们重写最后一个例子，让 firstName是带默认值的参数：

```

#### 可选参数

+ 输入多余的（或者少于要求的）参数是不允许的。所以定义可选的参数

**注意**：

1. 可选参数必须跟在必须参数后面（**可选参数后面不允许再出现必选参数了**）
2. 如果带默认值的参数出现在必须参数前面，用户必须明确的传入undefined值来获得默认值
3. typescript会将添加了默认值的参数识别为可选参数

#### 剩余参数

+ es6中，可以使用`...rest`的方式获取函数中的剩余参数（rest参数）





#### 重载

+ 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理

比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`。

```tsx

 // 重载
  // const reverse = (x: number | string): number | string | void => {
  //   if (typeof x === 'number') {
  //     return Number(x.toString().split('').reverse().join(''));
  //   } else if (typeof x === 'string') {
  //     return x.split('').reverse().join('');
  //   }
  // };
  // 然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。

  // 我们可以使用重载定义多个 reverse 的函数类型：
  // 我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示
  function reverse(x: number): number;
  function reverse(x: string): string;
  function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
      return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
      return x.split('').reverse().join('');
    }
  }
```



### 类型断言

+ 类型断言可以用来手动用来指定一个值的类型（值 as 类型）

#### 类型断言的用途

**将一个联合类型断言为其中一个类型**

+ 当typescript不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型中共有的方法

+ 使用类型断言时一定要格外小心，尽量避免断言后调用方法或引用深层属性，以减少不必要的运行时错误。

- 联合类型可以被断言为其中一个类型
- 父类可以被断言为子类
- 任何类型都可以被断言为 any
- any 可以被断言为任何类型

+ `Animal` 兼容 `Cat` 时，它们就可以互相进行类型断言了
+ 要使得 `A` 能够被断言为 `B`，只需要 `A` 兼容 `B` 或 `B` 兼容 `A` 即可



#### 双重断言





**注意**：

1. 类型断言不是类型转换，它不会真的影响到变量的类型。





### 声明文件

+ 当引入第三方库时，我们还需要引入它的声明文件，才能获得对应的代码补全，接口提示等功能。

#### 声明语句



#### 声明文件

我们会把声明语句单独的放到一个单独的文件，声明文件必须以`.d.ts`为后缀

#### 第三方声明文件

更推荐的是使用·`@types`统一管理第三方库的声明文件

#### 书写声明文件

当一个第三方库没有提供声明文件时，我们就需要自己书写声明文件了。





### 内置对象

Javascript中有很多内置对象，它们可以直接在TypeScript中做定义好了的类型

#### ECMAScript 的内置对象

``Boolean`， `Error`， `Date`， `RegExp`等

#### DOM和BOM的内置对象

`Document`， `HTMLElement`， `Event`，`NodeList`等






### 泛型
#### 代码理解

**需要一种方法使返回值的类型与传入参数的类型是相同的**

我们给identity添加了类型变量`T`。 `T`帮助我们捕获用户传入的类型（比如：`number`），之后我们就可以使用这个类型。 之后我们再次使用了 `T`当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 

+ 可重用性

```ts
// identity函数。 这个函数会返回任何传入它的值。
function identity(arg: number): number {
    return arg;
}
// 或者，我们使用any类型来定义函数：
// 使用any类型会导致这个函数可以接收任何类型的arg参数
// 这样就丢失了一些信息：传入的类型与返回的类型应该是相同的。如果我们传入一个数字，我们只知道任何类型的值都有可能被返回。
function identity1(arg: any): any {
    return arg;
}

// 我们使用了 类型变量，它是一种特殊的变量，只用于表示类型而不是值。
// 我们给identity3添加了类型变量T。 T帮助我们捕获用户传入的类型
// 我们把这个版本的identity3函数叫做泛型，因为它可以适用于多个类型。 不同于使用 any，它不会丢失信息，像第一个例子那像保持准确性，传入数值类型并返回数值类型。
function identity3<T>(arg: T): T {
    return arg;
}

// 定义了泛型函数后，可以用两种方法使用。
// 第一种是，传入所有的参数，包含类型参数：
let result6 = identity3<string>('cpp')
console.log(result6)

// 第二种方法更普遍。利用了类型推论 -- 即编译器会根据传入的参数自动地帮助我们确定T的类型：
let result7 = identity3('cpp')
console.log(result7)

```
**注意我们没必要使用尖括号（<>）来明确地传入类型；编译器可以查看myString的值，然后把T设置为它的类型。**



#### 使用泛型变量

```tsx
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
```



#### 泛型类型

+ 可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。

```ts
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <U>(arg: U) => U = identity;


// 可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。
const identity = <T,>(name: T) => name;
const myIdentity: <U>(name: U) => U = identity;
```

+ 可以使用带有调用签名的对象字面量来定义泛型函数

```tsx
const identity = <T,>(name: T) => name;
// 可以使用带有调用签名的对象字面量来定义泛型函数：
const myIdentity: { <T>(name: T): T } = identity;
```

+ 泛型接口

```tsx
interface GenericIdentityFn {
  <T>(arg: T): T;
}
const identity = <T,>(arg: T) => arg;
const myIdentity: GenericIdentityFn = identity;
```



+ 我们可能想把泛型参数当作整个接口的一个参数。这样我们就能清楚的知道具体是哪个泛型类型

```tsx
interface GenericIdentityFn<T> {
  (arg: T): T;
}
const identity = <T,>(arg: T) => arg;
const myIdentity: GenericIdentityFn<string> = identity;
```

#### 泛型类

**泛型类使用(<>)括起泛型类型，跟在类名后面**



#### 注意

**在`.tsx`文件中使用箭头函数和泛型**

1. 对通用参数使用扩展

```tsx
// 1. 对通用参数使用扩展
const identity = <T extends string>(name: T) => name;       
```

2. 使用尾逗号

```tsx
// 2. 使用尾逗号
const identity = <T,>(name: T) => name 
```







#### 泛型约束

+ 我们有时候想操作谋类型的一组值，并且我们知道这组值具有什么样的属性。

+ 比如我们想要限制函数去处理任意带有.length属性的所有类型，只要传入的类型有这个属性，
+ 为此，我们定义一个接口来描述约束条件。创建一个包含.length属性的接口，使用这个接口和`extends`关键字来实现约束

```tsx
interface LengthWise {
  length: number;
}
function loggingIdentity<T extends LengthWise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
const { length, value } = loggingIdentity({ length: 10, value: 222 });

<p>
  {length}: {value}
</p>
```

1. 在泛型约束中使用类型参数



2. 在泛型里使用类类型







### 枚举

#### 数字枚举

+ 支持反向映射，可以根据所引值反向获得枚举类型

1. 带初始值

```tsx
enum Direction {
  UP = 1,
  DOWN,
  LEFT,
  RIGHT,
}

<div>{Direction.UP}</div>		
<div>{Direction.DOWN}</div>
<div>{Direction.LEFT}</div>
<div>{Direction.RIGHT}</div>

// 1
// 2
// 3
// 4
```



2. 不带初始值

```tsx
enum Direction {
  UP,
  DOWN,
  LEFT,
  RIGHT,
}

<div>{Direction.UP}</div>
<div>{Direction.DOWN}</div>
<div>{Direction.LEFT}</div>
<div>{Direction.RIGHT}</div>

// 0
// 1
// 2
// 3
```

#### 字符串枚举

+ 不支持反向映射

```ts
enum Color {
  Red = 'Red',
  Green = 'Green',
  Blue = 'Blue'
}
const c = Color.Red   // Red
```



**注意**：

不带初始化器的枚举或者被放在第一的位置，或者被放在使用了数字常量或其他常量初始化了的枚举后面

=错误写法=

```tsx
enum E {
  A = 'c',
  B,
}
```

=应改为=

```tsx
enum E {
  B,
  A = 'c',
}

<div>{E.A}</div>
<div>{E.B}</div>
```



#### 常量枚举

它是适用const关键字修饰的枚举，也称const枚举，不同于常规的枚举，它们在编译阶段会被删除，只保留使用到的枚举成员值。

```ts
const enum Color {
  Red,
  Green,
  Blue
}
let c = [Color.Red, Color.Green, Color.Blue]
```

#### 异构枚举

字符串和数字混合的枚举

#### 使用枚举

通过枚举的属性来访问枚举成员，和枚举的名字来访问枚举类型



```ts
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
console.log(Direction.Up, Direction.Down, Direction.Left, Direction.Right)

enum Direction1 {
    Up,
    Down,
    Left,
    Right,
}
console.log(Direction1.Up, Direction1.Down, Direction1.Left, Direction1.Right)

enum Direction2 {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}
console.log(Direction2.Up, Direction2.Down, Direction2.Left, Direction2.Right)
```

**每个枚举成员都带有一个值，它可以是 常量或 计算出来的。 当满足如下条件时，枚举成员被当作是常量：**

+ 它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值 0
```ts
enum E {
    X
}
console.log(E.X)
```

+ 它不带有初始化器且它之前的枚举成员是一个 数字常量。 这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。
```ts
enum E1 {
    X, Y, Z
}
enum E2 {
    A = 1, B, C
}
console.log(E1.X, E1.Y, E1.Z)   // 0 1 2
console.log(E2.B, E2.C) // 2 3
```

```tsx
enum E4 {
    A,
    B,
    C = 'C',
    D = 'D',
    E,
    F,
  }
console.log(E4.A, E4.B, E4.C, E4.D, E4.E, E4.F)
// 这样子会报错



// 不带初始化器的枚举或者被放在第一的位置，或者被放在使用了数字常量或其它常量初始化了的枚举后面。
 enum E4 {
    A,
    B,
    C = 'C',
    D = 'D',
    E = 8,
    F,
  }
console.log(E4.A, E4.B, E4.C, E4.D, E4.E, E4.F)
// 0 1 "C" "D" 8 9
// 这样子不会
```

**注意**：

1. 不带初始化器的枚举或者被放在第一的位置，或者被放在使用了数字常量或其它常量初始化了的枚举后面。

字面量成员

const枚举成员

计算枚举成员

数字枚举的缺点





### 类型兼容性

+ 这里要检查`y`是否能赋值给`x`，编译器检查`x`中的每个属性，看是否能在`y`中也找到对应属性。
+ 在这个例子中，`y`必须包含名字是`name`的`string`类型成员。`y`满足条件，因此赋值正确。
+ `y`有个额外的`location`属性，但这不会引发错误。 只有目标类型（这里是`Named`）的成员会被一一检查是否兼容。
+ 这个比较过程是递归进行的，检查每个成员及子成员。

```tsx
let x: Named;
let y = { name: 'Alice', location: 'Seattle' };
x = y;
```

#### 比较两个函数

+ 要查看`x`是否能赋值给`y`，首先看它们的参数列表。 `x`的每个参数必须能在`y`里找到对应类型的参数。 
+ 参数的名字相同与否无所谓，只看它们的类型
+  这里，`x`的每个参数在`y`中都能找到对应的参数，所以允许赋值。
+  这里，`x`的每个参数在`y`中都能找到对应的参数，所以允许赋值。

```tsx
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // OK
x = y; // Error
```

下面来看看如何处理返回值类型，创建两个仅是返回值类型不同的函数：

```ts
let x = () => ({name: 'Alice'});
let y = () => ({name: 'Alice', location: 'Seattle'});

x = y; // OK
y = x; // Error, because x() lacks a location property
```

### 高级类型

#### 交叉类型

交叉类型是将多个类型合并为一个类型，我们可以把现有的多种类型叠加到一起成为一种类型

 例如， `Person & Serializable & Loggable`同时是 `Person` *和* `Serializable` *和* `Loggable`。 就是说这个类型的对象同时拥有了这三种类型的成员。

```tsx
type a = {
  name: string;
};
type b = {
  age: number;
};
let o: a & b = {
  name: 'aka',
  age: 20,
};
const o1: a & b = {
  age: 222,
  name: 'cjz',
};

<div>
  {o.name}
  {o.age}
  {o1.name}
  {o1.age}
</div>
```



#### 联合类型

联合类型表示一个值可以是几种类型之一。 我们用竖线（ `|`）分隔每个类型，所以 `number | string | boolean`表示一个值可以是 `number`， `string`，或 `boolean`。

```tsx
type a = {
  name: string;
};
type b = {
  age: number;
};
const o: a | b = {
  name: 'cpp',
};

<div>
  {o.name}
</div>
```

+ 如果一个值是联合类型，我们只能访问此联合类型的所有类型里共有的成员

```tsx
//  我们不能确定一个 Bird | Fish类型的变量是否有 fly方法。 如果变量在运行时是 Fish类型，那么调用 pet.fly()就出错了。
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

function getSmallPet(): Fish | Bird {
    // ...
}

let pet = getSmallPet();
pet.layEggs(); // okay
pet.swim();    // errors
```



+ 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**。

```tsx
// 报错！ length 不是 string 和 number 的共有属性，所以会报错。
function getLength(something: string | number): number {
 	return something.length;
}

//访问 string 和 number 的共有属性是没问题的
function getString(something: string | number): string {
 	return something.toString();
}

```



#### 类型保护与区分类型

1. **用户自定义的类型保护**

假若我们一旦检查过类型，就能在之后的每个分支里清楚地知道 `pet`的类型的话就好了。

TypeScript里的 *类型保护*机制让它成为了现实，类型保护就是一些表达式，它们会在运行时检查以确保在某个作用域里的类型。 要定义一个类型保护，我们只要简单地定义一个函数，它的返回值是一个 *类型谓词*

```tsx
function isFish(pet: Fish | Bird): pet is Fish {
  return (<Fish>pet).swim !== undefined;
}   
```

 在这个例子里， `pet is Fish`就是类型谓词。 谓词为 `parameterName is Type`这种形式，` parameterName`必须是来自于当前函数签名里的一个参数名。  

每当使用一些变量调用 `isFish`时，TypeScript会将变量缩减为那个具体的类型，只要这个类型与变量的原始类型是兼容的。

```ts
// 'swim' 和 'fly' 调用都没有问题了

if (isFish(pet)) {
    pet.swim();
}
else {
    pet.fly();
}
```

注意TypeScript不仅知道在 `if`分支里 `pet`是 `Fish`类型； 它还清楚在 `else`分支里，一定 *不是* `Fish`类型，一定是 `Bird`类型。

2. **`typeof`类型保护**

使用联合类型书写 `padLeft`代码

```tsx
// typeof 类型保护
function padLeft(value: string, padding: string | number) {
  if (typeof padding === 'number') {
    return Array(padding + 1).join(' ') + value;
  }
  if (typeof padding === 'string') {
    return padding + value;
  }
  throw new Error(`Expected string or number, got '${padding}'.`);
}
const pdl = padLeft1('Hello', '3333');

<div>{pdl}</div>
```

3. **instanceof类型保护**

`instanceof`类型保护是通过构造函数来细化



#### 可以为null的类型

TypeScript有两种特殊的类型：`null`和`undefined`



##### 1. 可选参数和可选属性

使用了 `--strictNullChecks`，**可选参数会被自动地加上 `| undefined`**

```tsx
function fn(x: number, y?: number) {
  return x + (y || 0)
}
fn(1, 2)
fn(1, undefined)
fn(1)
// fn(1, null) //  类型“null”的参数不能赋给类型“number | undefined”的参数。
```

可选属性也会有同样的结果

```tsx
let obj: { a: string; b?: number } = {
  a: 'cpp',
};
console.log(obj.a, obj.b);
obj.a = 'cjz'
// obj.a = undefined  // 不能将类型“undefined”分配给类型“string”
obj.b= 12
obj.b = undefined
```

##### 2. 类型保护和类型断言

由于可以为null的类型是通过联合类型实现，那么你需要使用类型保护来去除null

```ts
// 类型保护与类型断言
// 由于可以为null的类型是通过联合类型来实现的，所以你需要使用类型保护来去除null
function f(sn: string | null):string {
  if(sn === null) {
    return "default"
  } else {
    return sn
  }
}
// 这里很明显去除了null，也可以使用短路运算符
function f1(sn: string | null): string {
  return sn || "default"
}

```

如果编译器不能去除null和undefined，可以使用类型断言手动去除，语法是添加`!`后缀

`a!`从`a`里面取出了`null`和`undefined`

```ts
function broken(name: string | null): string {
  function postfix(epithet: string) {
    return name.charAt(0) + '. the' + epithet; // error: 对象可能为 "null"
  }
  name = name || 'cpp'
  return postfix('wqj')
}

function broken(name: string | null): string {
  function postfix(epithet: string) {
    return name!.charAt(0) + '. the' + epithet;
  }
  name = name || 'cpp';
  return postfix('wqj');
}
const result = broken('cjz');
console.log(result);
```

**注意**：

本例使用了嵌套函数，因为编译器无法去除嵌套函数的null（除非是立即调用的函数表达式），因为它无法跟踪对嵌套函数的调用，尤其是你将内层函数作为外层函数的返回值。如果无法知道函数在哪里被调用，就无法知道调用时name的类型。

#### 类型别名

类型别名会给一个类型起一个新名字。类型别名有时候和接口很像，但是可以作用于原始值，联合类型，元组，以及其他任何需要手写的类型。

起别名不会新建一个类型 - 它创建了一个新 *名字*来引用那个类型。 给原始类型起别名通常没什么用，尽管可以做为文档的一种形式使用。

```ts
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
  if (typeof n === 'string') {
    return n;
  } else {
    return n();
  }
}
const name = getName('www');
console.log(name);
```

#### 字符串字面量类型

字符串字面量类型允许你指定字符串必须的固定值。





字符串字面量类型还可以用于区分函数重载



#### 数字字面量类型





#### 可辨识类型

可以合并单例类型，联合类型，类型保护和类型别名来创建一个叫做 *可辨识联合*的高级模式，它也称做 *标签联合*或 *代数数据类型*。

1. 具有普通的单例类型属性— *可辨识的特征*。
2. 一个类型别名包含了那些类型的联合— *联合*。
3. 此属性上的类型保护。

```ts
interface Square {
    kind: "square";
    size: number;
}
interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
interface Circle {
    kind: "circle";
    radius: number;
}
```

首先我们声明了将要联合的接口。 每个接口都有 `kind`属性但有不同的字符串字面量类型。 `kind`属性称做 *可辨识的特征*或 *标签*。 其它的属性则特定于各个接口。 注意，目前各个接口间是没有联系的。 下面我们把它们联合到一起：

```ts
type Shape = Square | Rectangle | Circle;
```

现在我们使用可辨识联合:

```ts
function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
    }
}
```

**完整性检查**

当没有涵盖所有可辨识联合的变化时，我们想让编译器可以通知我们，



#### 索引类型

使用了索引类型，编译器就能够检查你用了动态属性名的代码。

### 模块



### 三斜线指令

+ 三斜线指令是包含单个XML标签的单行注释。注释的内容会作为编译器的指令使用
+ 三斜线指令告诉编译器在编译过程中要引入额外的文件



