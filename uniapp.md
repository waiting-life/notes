## uniapp学习

### uni-app介绍

**uni-app**是一个使用**Vue.js**开发所有应用的前端框架，可以发布到IOS，Android，H5以及各种小程序等多个平台。

### 配置

单独的页面的配置"navigationBarTitleText"需要加载style里面

```json
{
	"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		{
			"path":"pages/message/message",
			"style": {
				"navigationBarTitleText": "信息页"
			}
		},
		{
			"path": "pages/index/index",
			"style": {
				"navigationBarTitleText": "uni-app"
			}
		}
	],
	"globalStyle": {
		"navigationBarTextStyle": "white",
		"navigationBarTitleText": "uni-app",
		"navigationBarBackgroundColor": "#76EEC6",
		"backgroundColor": "#7CCD7C",
		"enablePullDownRefresh":true,
		"backgroundTextStyle":"light"
	}
}

```

### 配置

#### tabbar配置，position

```json
"position": "top"
//只在小程序中起作用,h5网页不起作用
```

#### condition启动模式配置

### 组件的基本使用

#### text

##### 属性

1. selectable
2. space

#### view

##### 属性

1. hover-class

   **点击的时候触发相应类名的样式**

2. hover-stop-propagation

   **阻止事件冒泡**

3. hover-start-time

   **按下多久之后出现点击态**

4. have-stay-time

   **松开后点击态保留时间**



#### button



#### image

image标签默认的高度300px，宽度为225px

### uni-app的样式

+ rpx：响应式px，一种根据屏幕宽度自适应的动态单位。以750宽的屏幕为基准，750rpx恰好为屏幕宽度

### uni-app中的数据绑定，事件

##### v-bind

##### v-for

##### v-on

事件的传参：如果不传参，输出e为事件对象；如果传入参数，第一个参数·为传入的参数，这时候想拿到事件对象e，在传参的时候传入$event

```html
<button @click="handleClick(1, $event)">点击</button>

```

### uni-app的生命周期

#### 应用的生命周期



#### 页面的生命周期



### 页面的上拉触底和下拉刷新

#### 配置

##### enablePullDownRefresh

##### onReachBottomDistance



#### 页面中

##### onPullDownRefresh

```js
	export default {
		data() {
			return {
				list: ['cpp', 'cjz', 'clg', 'cxz', 'slg']
			}
		},
		onLoad() {
			console.log('onLoad')
		},
        // 下拉刷新的钩子
		onPullDownRefresh() {
			console.log('触发下拉刷新',this)
			const {list} = this
			this.list.push(...list)
			uni.stopPullDownRefresh()
		},
        // 点击按钮触发的下拉刷新
		methods: {
			handlePullDownRefresh() {
				uni.startPullDownRefresh()
			}
		}
	}
```

##### onReachBottom

```js
onReachBottom() {
    console.log('页面触底了')
}
```

### 发送请求

#### uni.request()

### 本地存储

```js
// 存储数据

uni.setStorage({

	key: 'lover',

	data,

	success: (res) => {

		console.log(res)

	}

})

uni.setStorageSync('lover', this.lover)

// 获取本地存储
// 同步
 const result = uni.getStorageSync('lover')
 console.log(result)

// 异步
uni.getStorage({
    key: 'lover',
    success(result) {
        console.log(result)
    },
    fail(err) {
        console.log(err)
    }
})

// 清除本地存储
// uni.removeStorageSync('lover')
// console.log('清除本地存储')

uni.removeStorage({
    key: 'lover',
    success() {
        console.log('本地缓存清除成功')
    }
})
```

### 图片

#### 上传图片

```js
uni.chooseImage({
    count: 5,
    success: (result) => {
        // console.log(result)
        this.imageArr = result.tempFilePaths
    }
})
```



#### 预览图片

```js
// 点击预览图片
handlePreviewImg(current) {
    console.log(current)
    uni.previewImage({
        current,
        urls: this.imageArr
    })
}
```

### 条件注释实现跨段兼容

```html
<!-- #ifdef H5 -->
<view class="h5_seen">我只在h5页面显示</view>
<!-- #endif -->
<!-- #ifdef MP-WEIXIN -->
<view class="mini_app_seen">我只在小程序页面显示</view>
<!-- #endif -->
```

```js
onLoad() {
    // #ifdef H5
    console.log("我希望在h5中打印")
    // #endif
    // #ifdef MP-WEIXIN
    console.log('我希望在微信小程序中打印')
    // #endif
}
```

```css
/* h5中的样式 */
/* #ifdef H5 */
view {
    color: pink;
}
/* #endif */
/* 微信小程序中的样式 */
/* #ifdef MP-WEIXIN */
view {
    color: #2C405A;
}
/* #endif */
```

### uni-app中的导航跳转

#### 声明式跳转

```html
<!-- 声明式导航 -->
<navigator url="/pages/detail/detail">跳转至详情页</navigator>
<navigator url="/pages/detail/detail?id=2&age=22">跳转至详情页，并且传参</navigator>
<navigator url="/pages/detail/detail" open-type="redirect">跳转至详情页并关闭当前页面</navigator>
<!-- tabbar页面 -->
<navigator url="/pages/message/message" open-type="switchTab">跳转至信息页</navigator>

```



#### 编程式跳转



```html
<!-- 编程式导航 -->
<button type="default" @click="toDetailNavigator">跳转到详情页</button>
<button type="default" @click="toDetailRedirect">跳转至详情页并关闭当前页面</button>
<!-- 跳转到tabbar页面 -->
<button type="default" @click="toMessaeg">跳转到信息页</button>
```

```js
methods: {
    toDetailNavigator() {
        uni.navigateTo({
            url: '/pages/detail/detail'
        })
    },
    toMessaeg() {
        // 跳转到tabbar页面，并关闭所有非tabbar页面
        uni.switchTab({
            url: '/pages/message/message'
        })
    },
    toDetailRedirect() {
        uni.redirectTo({
            url: '/pages/detail/detail'
        })
    }
}
```

#### 拿到参数

比如跳转到详情页，可以在详情页的onLoad中拿到参数

```js
onLoad(options) {
    console.log(options)
}
```

### uni-app中创建组件的方式

```html
<test></test>
```

```js
components: {
    test
},
```

#### 组件的生命周期函数

和vue的一样

created的时候：初始化数据

mounted：操作dom

destroyed：清除定时器

#### 组件的通信

##### 1. 父子组件通信

props

$emit

##### 2. 兄弟组件传值

**uni.$emit():**全局触发

**uni.$on()**:全局监听

**A组件**:触发，修改B组件

```js
addCount() {
    uni.$emit('updateCount', 2)
}
```

**B组件:**B组件监听到，执行回调

```js
created() {
    uni.$on('updateCount', (data) => {
        this.count+=data
    })
}
```



### 扩展组件uni-ui

[uni-ui](https://uniapp.dcloud.io/component/README?id=uniui)

### 项目实战

