1. js数据类型、区别 
基本类型:number,string,boolean,null,undefined,
引用类型:object

2. 说说typeof和instanceof
3. 说说new操作符
4. 说说事件循环

当我们点击了一个元素的时候，这个点击事件首先会从外层元素开始向内传播，叫做事件捕获，到达目标元素后，又会从目标元素开始向外冒泡
5. Promise 的用法？了解 allSettled 方法么，怎么实现？
6. 闭包

闭包是一个有权访问函数作用域的函数。因为外层作用域无法读取内层作用域的变量，所以我们可以在内层作用域中返回一个函数，该函数可以读取到内层作用域声明的变量
7. ES5 实现继承的方法

比如我们声明两个构造函数Animal，Person
Animal.call(this, ...args)
通过Person.prototype = Object.create(Animal.prototype), new Person的时候，该实例的__proto__属性指向Person.prototype, Person.prototype.___proto__又指向了Animal.prototype
Person.prototype.constructor = Person

实现继承
   我们希望子类的原型的__proto__指向父类的原型对象Person.prototype = Object.create(Animal.prototype)
   子类的构造函数要复用父类构造函数Animal.call(this, ...args)
1. 说说作用域链

作用域有全局作用域和函数作用域，全局作用域中声明的变量在函数作用域中也可以使用，但是函数作用域中声明的变量在全局中无法读取
当我们使用一个变量的时候，首先会在当前作用域中找有没有这个变量，有的话直接使用该作用域中的值，
如果没有的话就会在上一级作用域中找，直到全局作用域
9. 函数式编程与面向对象的区别，优缺点