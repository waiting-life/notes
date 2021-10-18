# Vue3学习

## Vue3.0学习笔记

### 准备

#### 创建项目

1. 使用vue-cli创建项目



2. 使用vite创建

vite：新一代前端构建工具

```shell
npm init vite-app vue3_test_vite
cd vue3_test_vite
npm i
npm run dev
```



#### 文件介绍

1. main.js入口文件

```js
// 引入的不再是vue构造函数了，引入的是一个名为createApp的工厂函数
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')

// 如下
// 创建应用实例对象-app（类似于之前vue2的vm， 但比vm更“轻”）
const app = createApp(App)
// 挂载
app.mount('#app')

```

2. vue3组件中的模板结构可以没有根标签

### 常用Composition API(组合式API)

#### 1. setup

+ **理解**：vue3中的一个新的配置项，值为一个函数。

+ setup是所有**composition API**（组合API）表演的舞台

+ 组合中所用到的：数据、方法等等，均要配置在setup中

+ setup函数的两种返回值：

  1. **若返回一个对象，则对象中的属性、方法、在模板中均可以直接使用**

  ```vue
  <template>
    <div>
      <h1>我是APP组件</h1>
      <div>name: {{ name }}</div>
      <div>age: {{ age }}</div>
      <button @click="sayHello">打招呼</button>
    </div>
  </template>
  
  <script>
  export default {
    name: "App",
    // 此处只是测试一下setup，暂时不考虑响应性
    setup() {
      // 数据
      let name = "cpp";
      let age = 18;
  
      // 方法
      function sayHello() {
        console.log(`hello, 我叫${name}, 我今年${age}岁了`);
      }
      sayHello();
      // + setup函数的两种返回值：
      // 1. 若返回一个对象，则对象中的属性、方法、在模板中均可以直接使用(重点)
      // 2. 若返回一个渲染函数，则可以自定义渲染内容
      return {
        name,
        age,
        sayHello,
      };
    },
  };
  </script>
  
  ```

  

  2. 若返回一个渲染函数，则可以自定义渲染内容

  ```vue
  <template>
    <div>
      <h1>我是APP组件</h1>
      <div>name: {{ name }}</div>
      <div>age: {{ age }}</div>
      <button @click="sayHello">打招呼</button>
    </div>
  </template>
  
  <script>
  import {h} from 'vue'
  export default {
    name: "App",
    // 此处只是测试一下setup，暂时不考虑响应性
    setup() {
      // 数据
      let name = "cpp";
      let age = 18;
  
      // 方法
      function sayHello() {
        console.log(`hello, 我叫${name}, 我今年${age}岁了`);
      }
      sayHello();
      // + setup函数的两种返回值：
      // 1. 若返回一个对象，则对象中的属性、方法、在模板中均可以直接使用(重点)
      // 2. 若返回一个渲染函数，则可以自定义渲染内容
      // return {
      //   name,
      //   age,
      //   sayHello,
      // };
  
      // 返回一个渲染函数
      return () => h("h1", "vue3练习");
    },
  };
  </script>
  
  ```

  

+ 注意点：

  1. 尽量不要与vue2配置混用
     + vue2配置（data， method，computed。。。）中可以访问到setup中的属性、方法
     + 但在setup中不能访问到vue2的配置
     + 如果有重名，setup优先
  2. setup不能是一个async函数，因为返回值不再是return的对象，而是promise，模板看不到return对象中的属性

#### 2. ref函数

+ **作用**：定义一个响应式的数据
+ **语法**：`const xxx = ref(initValue)`
  + 创建一个包含响应式数据的**引用对象**(reference对象，简称ref对象)
  + JS中的操作数据`xxx.value`
  + 模板中读取数据，不需要`.value`,直接`<div>{xxx}</div>`
+ **备注**：
  + 接收的数据可以是：基本类型、也可以是对象类型
  + 基本类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`
  + 对象类型的数据：内部求助了vue3中的一个新函数——`reactive`函数

在模版上展现的时候不需要`.value`, vue解析的时候发现是一个ref对象，会自动的通过`.value`读取其值

```vue
<template>
  <div>
    <h1>我是APP组件</h1>
    <div>name: {{ name }}</div>
    <div>age: {{ age }}</div>
    <div>工作种类: {{ job.type }}</div>
    <div>工资： {{ job.salary }}</div>
    <button @click="sayHello">打招呼</button>
    <button @click="changeInfo">修改信息</button>
  </div>
</template>

<script>
// import {h} from 'vue'
import { ref } from "vue";
export default {
  name: "App",
  // 此处只是测试一下setup，暂时不考虑响应性
  setup() {
    // 数据,这么定义的数据不是响应式的
    // let name = "cpp";
    // let age = 18;

    // RefImpl{ value ....} 也是通过get  set实现的响应式
    let name = ref("cpp");
    let age = ref(18);
    // Proxy  ref里面是对象的时候，是通过Proxy代理对象
    let job = ref({
      type: "前端工程师",
      salary: "30k",
    });

    // 方法
    function sayHello() {
      console.log(`hello, 我叫${name.value}, 我今年${age.value}岁了`);
    }
    sayHello();
    function changeInfo() {
      name.value = "cjz";
      age.value = 20;
      job.value.type = "UI设计师";
      job.value.salary = "60k";
      console.log(name, age);
      console.log(job.value);
    }

    // + setup函数的两种返回值：
    // 1. 若返回一个对象，则对象中的属性、方法、在模板中均可以直接使用(重点)
    // 2. 若返回一个渲染函数，则可以自定义渲染内容
    return {
      name,
      age,
      job,
      sayHello,
      changeInfo,
    };
  },
};
</script>

```

#### 3. reactive函数

+ **作用**：定义一个对象类型的响应式数据（基本类型别用它，用`ref`函数）
+ **语法**：`const 代理对象 = reactive(被代理对象)`，接收一个对象(或数组)，返回一个**代理对象(Proxy的实例对象，简称Proxy对象)**
+ reactive函数定义的响应式数据是“深层次的”
+ 内部基于es6的Proxy实现，通过代理对象操作源对象内部数据都是响应式的

```vue
<template>
  <div>
    <h1>我是APP组件</h1>
    <div>name: {{ person.name }}</div>
    <div>age: {{ person.age }}</div>
    <div>工作种类: {{ person.job.type }}</div>
    <div>工资： {{ person.job.salary }}</div>
    <div>c: {{ person.job.a.b.c }}</div>
    <div>爱好：{{ person.hobbies }}</div>
    <button @click="sayHello">打招呼</button>
    <button @click="changeInfo">修改信息</button>
  </div>
</template>

<script>
import { reactive } from "vue";
export default {
  name: "App",
  setup() {
    // reactive 没有不能接收基本类型，可以通过把基本类型放在一个整体的对象里面
    // 合并
    const person = reactive({
      name: "cpp",
      age: 2,
      job: {
        type: "前端工程师",
        salary: "30k",
        a: {
          b: {
            c: 666,
          },
        },
      },
      hobbies: ["游戏", "动漫", "音乐"],
    });

    function changeInfo() {
      person.name = "cjz";
      person.age = 20;
      person.job.type = "UI设计师";
      person.job.salary = "60k";
      person.job.a.b.c = 999;
      person.hobbies[0] = "学习";
    }

    // + setup函数的两种返回值：
    // 1. 若返回一个对象，则对象中的属性、方法、在模板中均可以直接使用(重点)
    // 2. 若返回一个渲染函数，则可以自定义渲染内容
    return {
      person,
      changeInfo,
    };
  },
};
</script>

```

#### 4. Vue3的响应式

##### Vue2.0的响应式

1. **实现原理**：

+ 对象类型：通过`Object.defineProperty()`对属性的读取、修改进行拦截（数据劫持）
+ 数据类型：通过重写更新数据的一系列方法来实现拦截。（对数组的变更方法进行了包裹）

```js
Object.defineProperty(data, 'count', {
  get() {},
  set() {}
})
```

2. **存在问题**：

+ 新增属性、删除属性，界面不会更新
+ 直接通过下标修改数组，界面不会更新

##### Vue3.0的响应式

1. **实现原理**：

+ 通过Proxy(代理)：拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等。
+ 通过Reflect(反射)：对别代理对象的属性进行操作。
+ MDN文档中描述的Proxy与Reflect

**模拟vue3中实现响应式**

**window.Proxy**(代理)

```js
// window.proxy
const person = {
	name: 'cpp',
  age: 2
}
const p = new Proxy(person, {
  // 读取
  get(target, propName) {
    console.log(`有人读取了p的${propName}属性`, target, propName)
    return target[propName]
  },
  // 修改 / 增加
  set(target, propName, value) {
  	console.log(`有人修改了p的${propName}属性,我要去更新界面了！`)
    target[propName] = value
	},
  // 删除
  deleteProperty(target, propName) {
    console.log(`有人删除了p的${propName}属性，我要去更新界面了！`)
    return delete target[propName]
  }  
})

// Reflect，vue3的实现
const person = {
	name: 'cpp',
  age: 2
}
const p = new Proxy(person, {
  // 读取
  get(target, propName) {
    console.log(`有人读取了p的${propName}属性`, target, propName)
    return Reflect.get(target, propName)
  },
  // 修改 / 增加
  set(target, propName, value) {
  	console.log(`有人修改了p的${propName}属性,我要去更新界面了！`)
    Reflect.set(target, propName, value)
	},
  // 删除
  deleteProperty(target, propName) {
    console.log(`有人删除了p的${propName}属性，我要去更新界面了！`)
    return Reflect.deleteProperty(target, propName)
  }  
})
```



**window.Reflect**(反射)

```js
Reflect.get()
Reflect.set()
// 1. 通过Object.defineProperty去操作

// 2. 通过Reflect.defineProperty去操作
```

#### 5. reactive对比ref

+ **从定义数据的角度对比**
  + ref用来定义：基本数据类型
  + reactive用来定义：对象(或数组)类型数据

备注：ref也可以用来定义对象(或数组)类型数据，它内部会自动通过`reactive`转为对象

+ **从原理角度对比：**
  + ref通过`Object.defineProperty()`的`get`与`set`来实现响应式(数据劫持)
  + reactive通过使用**Proxy**来实现响应式（数据劫持），并通过**Reflect**操作源对象内部的数据
+ **从使用角度对比：**
  + ref定义的数据：操作数据需要`.value`，读取数据时模板中直接读取不需要`.value`
  + reactive定义的数据：操作数据与读取数据，均不需要`.value`

#### 6. setup两个注意点

+ setup的执行时机
  + 在beforeCreate之前执行一次，this是undefined
+ setup的参数
  + props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性
  + context：上下文对象
    + attrs：值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性，相当于`this.$attrs`
    + slots：收到的插槽内容，相当于`this.$slots`
    + Emit: 分发自定义事件的函数，相当于`this.$emit`

```vue
// APP.vue
<template>
  <Demo job="前端开发" love="wqj" @hello="showHelloMsg" />
</template>

<script>
import Demo from "./components/Demo";
export default {
  name: "App",
  components: {
    Demo,
  },
  setup() {
    function showHelloMsg(value) {
      console.log(`你好啊，你触发了hello事件，我收到的参数是:${value}`);
    }
    return {
      showHelloMsg,
    };
  },
};
</script>

// Demo.vue
<template>
  <div>
    <h1>Demo组件</h1>
    <h2>姓名： {{ person.name }}</h2>
    <h2>年龄： {{ person.age }}</h2>
    <button @click="test">测试触发一下Demo组件的Hello事件</button>
  </div>
  </div>
</template>

<script>
import { reactive } from "vue";
export default {
  name: "Demo",
  props: ["job", "love"],
  // emits
  emits: ["hello"],
  setup(props, context) {
    // attrs
    console.log("---setup---", context.attrs);
    const person = reactive({
      name: "cpp",
      age: 18,
    });
    
    // emits
    function test() {
      // 1. 触发的事件 2. 参数值
      context.emit("hello", "cpp");
    }
    
    // slots  `v-slot:`
    
    return {
      person,
    };
  },
};
</script>
```



#### 7. 计算属性与监视

##### 1. 计算属性computed

```js
// 计算属性——简写（没有考虑计算属性被修改的情况）
const person = reactive({
  name: "cpp",
  age: 18,
  firstName: "cheng",
  lastName: "jz",
});
person.fullName = computed(() => {
  return person.firstName + person.lastName;
});
function test() {
  // 1. 触发的事件 2. 参数值
  context.emit("hello", "cpp");
}
return {
  person,
  test,
};

// 计算属性——完整写法（考虑读和写）
person.fullName = computed({
  get() {
    return person.firstName + "-" + person.lastName;
  },
  set(value) {
    const nameArr = value.split("-");
    person.firstName = nameArr[0];
    person.lastName = nameArr[1];
  },
});
```

##### 2. watch

**注意**

+ 两个小坑
  + 监视reactive定义的响应式数据时：oldValue无法正确获取，强制开启了深度监视（deep配置失效）
  + 监视reactive定义的响应式数据中的某个属性时：deep配置有效

```vue
<template>
  <div>
    <h2>当前求和为： {{ sum }}</h2>
    <button @click="sum++">点我+1</button>
    <br />
    <h2>当前的信息为：{{ msg }}</h2>
    <button @click="msg += '!'">修改信息</button>
    <br />
    <h2>姓名：{{ person.name }}</h2>
    <h2>年龄：{{ person.age }}</h2>
    <h2>薪资：{{ person.job.j1.salary }}K</h2>
    <button @click="person.name += '~'">修改姓名</button>
    <button @click="person.age++">修改年龄</button>
    <button @click="person.job.j1.salary++">涨薪</button>
  </div>
</template>

<script>
import { ref, watch, reactive } from "vue";

export default {
  name: "ComputedWatch",
  setup() {
    let sum = ref(0);
    let msg = ref("你好啊");
    const person = reactive({
      name: "cpp",
      age: 2,
      job: {
        j1: {
          salary: "30",
        },
      },
    });
    // 情况一： 监视ref定义的一个响应式数据
    // 接收第三个参数，配置
    watch(
      sum,
      (newValue, oldValue) => {
        console.log("sum变了", newValue, oldValue);
      },
      { immediate: true }
    );

    // 情况二： 监视ref所定义的多个响应式数据
    watch([sum, msg], (newValue, oldValue) => {
      console.log("sum或者msg改变了", newValue, oldValue);
    });

    // 情况三：监视reactive定义的响应式数据中的全部属性
    // 注意：
    //    1. 此处无法获得正确的oldValue
    //    2. 强制开启了深度监视(deep配置无效)
    watch(
      person,
      (newValue, oldValue) => {
        console.log("person变化了", newValue, oldValue);
      },
      { deep: false }
    ); // 此处的deep配置无效

    // 情况四： 监视reactive所定义的响应式数据中的某个属性
    watch(
      () => person.age,
      (newValue, oldValue) => {
        console.log("person的age变化了", newValue, oldValue);
      }
    );

    // 情况四： 监视reactive所定义的响应式数据中的某些属性
    watch([() => person.age, () => person.name], (newValue, oldValue) => {
      console.log("person的age或name变化了", newValue, oldValue);
    });

    // 特殊情况
    watch(
      () => person.job,
      (newValue, oldValue) => {
        console.log("person的job变化了", newValue, oldValue);
      },
      {
        deep: true,
      }
    ); // 此处由于监视的是reactive所定义的对象中的某个属性，所以deep配置有效
    return {
      sum,
      msg,
      person,
    };
  },
};
</script>
```



##### 3. watchEffect函数

+ watch的套路是：既要指明监视的属性，也要指明监视的回调。
+ watchEffect的套路是： 不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。
+ watchEffect有点像computed：
  + 但computed注重的是计算出来的值（回调函数的返回值），所以必须要写返回值
  + 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

```js
// watchEffect所指定的回调中用到的数据只要发生变化。则直接重新执行回调
watchEffect(() => {
  const x1 = sum.value
  const x2 = person.age
  console.log('watchEffect配置的回调执行了')
})
```

```vue
<template>
  <div>
    <h2>当前求和为： {{ sum }}</h2>
    <button @click="sum++">点我+1</button>
    <br />
    <h2>当前的信息为：{{ msg }}</h2>
    <button @click="msg += '!'">修改信息</button>
    <br />
    <h2>姓名：{{ person.name }}</h2>
    <h2>年龄：{{ person.age }}</h2>
    <h2>薪资：{{ person.job.j1.salary }}K</h2>
    <button @click="person.name += '~'">修改姓名</button>
    <button @click="person.age++">修改年龄</button>
    <button @click="person.job.j1.salary++">涨薪</button>
  </div>
</template>

<script>
import { ref, reactive, watchEffect } from "vue";

export default {
  name: "WatchEffect",
  setup() {
    let sum = ref(0);
    let msg = ref("你好啊");
    const person = reactive({
      name: "cpp",
      age: 2,
      job: {
        j1: {
          salary: "30",
        },
      },
    });

    watchEffect(() => {
      console.log("watchEffect所指定的回调执行了"); // 用谁就监视谁
      const x1 = sum.value
      const x2 = person.job.j1.salary
      console.log(x1, x2)
    });
    return {
      sum,
      msg,
      person,
    };
  },
};
</script>
```

#### 8. 生命周期

+ **vue3中可以继续使用vue2中的生命周期钩子，但有两个被更名**
  + `beforeDestroy`改名为`beforeMount`
  + `destroyed`改名为`unmounted`

+ **Vue3也提供了Composition API形式的生命周期钩子，与vue2中钩子对应关系如下**
  + `beforeCreate` ===> `setup()`
  + `created` ===> `setup()`
  + `beforeMount` ===> `onBeforeMount`
  + `mounted` ===> `onMounted`
  + `beforeUpdate` ===> `onBeforeUpdate`
  + `updated` ===> `onUpdated`
  + `beforeUnmount` ===> `onbeforeUnmount`
  + `unmounted` ===> `onUnmounted`

**注意**：setup()相当于beforeCreate和created，vue3并没有提供beforeCreate和created的组合式API

```vue
<template> 
    <h1>生命周期</h1>
    <h2>当前求和为：{{sum}}</h2>
    <button @click="sum++">加1</button>
</template>

<script>
import {ref,  onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted} from "vue"
export default {
    name: 'Life',
    setup() {
        let sum = ref(0)
         // 如果要将生命周期些在setup中的话，生命周期钩子名字需要改变
        // 通过组合式API的形式去使用生命周期钩子
        onBeforeMount(() => {
            console.log('---onBeforeMounnt---')
        })
        onMounted(() => {
            console.log('---onMounted---')
        }) 
        onBeforeUpdate(() => {
            console.log('---onBeforeUpdate---')
        })
        onUpdated(() => {
            console.log('---onUpdated---')
        }) 
        onBeforeUnmount(() => {
            console.log('---onBeforeUnmount---')
        })
        onUnmounted(() => {
            console.log('---onUnmounted---')
        })
        return {
            sum
        }
        
    },
};
</script>
```

#### 9. 自定义hook函数





#### 10. toRef与toRefs

**1. toRef**

+ 作用：创建一个对象，其value值指向另一个对象中的某个属性
+ 语法：`const name = toRef(person, 'name')`
+ 应用：要将响应式对象中的某个属性单独提供给外部使用时

```vue
<template>
  <div>
    <h2>{{ person }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>年龄：{{ age }}</h2>
    <h2>薪资：{{ salary }}K</h2>
    <button @click="name += '~'">修改姓名</button>
    <button @click="age++">修改年龄</button>
    <button @click="salary++">涨薪</button>
  </div>
</template>

<script>
import { reactive, toRef, ref } from "vue";

export default {
  name: "ToRef",
  setup() {
    const person = reactive({
      name: "cpp",
      age: 2,
      job: {
        j1: {
          salary: 30,
        },
      },
    });

    // 只是把一个字符串赋值给name1了，没有响应式
    const name1 = person.name;
    console.log("%%%", name1);

    // 通过toRef转换为响应式，当我们读取name2.value的时候，还是会去找person里面的name，拥有响应式
    const name2 = toRef(person, "name");
    console.log("%%%", name2);

    return {
      person,
      // 如果是ref的话，修改并不会触发原对象person的修改
      // name: ref(person.name),
      // age: ref(person.age),
      // salary: ref(person.job.j1.salary),

      name: toRef(person, "name"),
      age: toRef(person, "age"),
      salary: toRef(person.job.j1, "salary"),
    };
  },
};
</script>
```



**2. toRefs**

+ 作用：`toRefs`与`toRef`功能一致，但可以批量操作多个ref对象
+ 语法：`toRefs(person)`

```vue
<template>
  <div>
    <h2>{{ person }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>年龄：{{ age }}</h2>
    <h2>薪资：{{ job.j1.salary }}K</h2>
    <button @click="name += '~'">修改姓名</button>
    <button @click="age++">修改年龄</button>
    <button @click="job.j1.salary++">涨薪</button>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";

export default {
  name: "ToRef",
  setup() {
    const person = reactive({
      name: "cpp",
      age: 2,
      job: {
        j1: {
          salary: 30,
        },
      },
    });

    const x = toRefs(person);
    console.log(x);

    return {
      person,
      // 只会返回第一层
      ...toRefs(person),
    };
  },
};
</script>
```



### 其他Composition API

#### 1. shallowReactive与shallowRef

+ `shallowReactive`：只处理对象最外层属性的响应式（浅响应式）
+ `shallowRef`：只处理基本数据类型的响应式，不进行对象的响应式处理
+ 什么时候使用？
  + 如果有一个对象数据，结构比较深，但变化时只是外层属性变化 ===> `shallowReactive`
  + 如果有一个对象数据，后续功能不会修改对象中的属性，而是生新的对象来替换 ===> `shallowRef`

**shallowReactive**:

```vue
// 修改salary的时候页面不会改变
<template>
  <div>
    <h2>{{ person }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>年龄：{{ age }}</h2>
    <h2>薪资：{{ job.j1.salary }}K</h2>
    <button @click="name += '~'">修改姓名</button>
    <button @click="age++">修改年龄</button>
    <button @click="job.j1.salary++">涨薪</button>
  </div>
</template>

<script>
import { shallowReactive, toRefs } from "vue";

export default {
  name: "ToRef",
  setup() {
    const person = shallowReactive({
      name: "cpp",
      age: 2,
      job: {
        j1: {
          salary: 30,
        },
      },
    });

    const x = toRefs(person);
    console.log(x);

    return {
      person,
      // 只会返回第一层
      ...toRefs(person),
    };
  },
};
</script>
```

**shallowRef**:

```vue
<template>
  <div>
    <h2>{{ person }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>年龄：{{ age }}</h2>
    <h2>薪资：{{ job.j1.salary }}K</h2>
    <button @click="name += '~'">修改姓名</button>
    <button @click="age++">修改年龄</button>
    <button @click="job.j1.salary++">涨薪</button>

    <h4>当前a的值为: {{ a }}</h4>
    <h4>当前a1的值为: {{ a1 }}</h4>
    <button @click="a++">修改a值</button>
    <button @click="a1++">修改a1值</button>

    <h4>当前b.c的值为: {{ b.c }}</h4>
    <h4>当前b1.c1的值为: {{ b1.c1 }}</h4>
    <button @click="b.c++">修改b.c值</button>
    <button @click="b1.c1++">修改b1.c1值</button>  // 里面的没有响应式
    <button @click="b1 = { c1: 888 }">点击替换b1</button>  // 第一层都是响应式的

  </div>
</template>

<script>
// import { reactive, ref, toRef, toRefs, shallowRective } from "vue";
import { toRefs, shallowReactive, ref, shallowRef } from "vue";

export default {
  name: "ToRef",
  setup() {
    const person = shallowReactive({
      name: "cpp",
      age: 2,
      job: {
        j1: {
          salary: 30,
        },
      },
    });
    let a = ref(0);
    let a1 = shallowRef(0);
    
    // ref, shallowRef 第一层都是响应式的
    
		// b.value为一个Proxy对象
    let b = ref({
      c: 0,
    });
    console.log("***", b);
    // shallowRef不去处理对象类型的响应式
    // b1.value为一个普通对象
    let b1 = shallowRef({
      c1: 0,
    });
    console.log("***", b1);
    return {
      person,
      a,
      a1,
      b,
      b1,
      ...toRefs(person),
    };
  },
};
</script>
```

#### 2. readonly与shallowReadonly

+ readonly：让一个响应式数据变为只读的（深只读）
+ shallowReadonly：让一个响应式数据变为只读的（浅只读）
+ 应用场景：不希望数据被修改时。



#### 3. toRaw与markRaw

+ toRaw
  + 作用：将一个由`reactive`生成的**响应式对象**转为**普通对象**
  + 使用场景：用于读取响应式对象对应的普通对象，对于这个普通对象所有的操作，不会引起页面更新。
+ markRaw
  + 作用：标记一个对象，使其永远不会再成为响应式对象。
  + 应用场景：
    1. 有些值不应该设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。



#### 4. customRef自定义ref

+ 作用：创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显式控制。

`customRef(track, trigger)`

```vue
```



#### 5. provide与inject

+ 作用：实现组件间通信
+ 套路：父组件有一个`provide`选项来提供数据，后代组件有一个`inject`选项来使用这些数据
+ 具体写法

```vue
```



#### 6. 响应式数据的判断

+ isRef：检查一个值是否为一个ref对象
+ isReactive：检查一个对象是否由reactive擦魂更加爱你的响应式代理
+ isReadonly：检查一个对象是否由readonly常见的只读代理
+ isProxy：检查一个对象是否由reactive或者readonly方法创建的代理

