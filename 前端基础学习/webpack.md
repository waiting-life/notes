## webpack学习

### 一、概念

#### 1. 入口(entry)

**入口起点**指示webpack应该是用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。默认值为`./src`

#### 2. 出口(output)

**output**属性告诉webpack在那里输出它所创建的bundles，以及如何明明这些文件，默认值为`/dist`

#### 3. loader

**loader**让webpack能够处理那些js文件(webpack自身只理解js)。loader可以将所有类型的文件转换为webpack能够处理的有效模块。

在更高层面，在 webpack 的配置中 **loader** 有两个目标：

1. `test` 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
2. `use` 属性，表示进行转换时，应该使用哪个 loader。

#### 4. 插件plugins

loader被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境变量。

### 二、简单的构建项目

+ webpack默认配置里面入口就是src目录，出口就是dist

#### 1. 如果不想让src作为入口文件

在webpack.config.js里面配置

```js
const path = require('path')
module.exports = {
  entry: './app.js',  // 工程资源的入口
  output: {
    path: path.join(__dirname, 'dist'), // path必须是绝对路径
    filename: 'bundle.js'
  }
}
```

#### 2. webpack-dev-server

**安装**：

```shell
局部安装：npm install --save-dev webpack-dev-server

全局安装：npm install -g webpack-dev-server
```

**注意**：

webpack-cli的版本问题

```js
对于 webpack-cli 3.x:

"scripts": { "start:dev": "webpack-dev-server" }

对于 webpack-cli 4.x:

"scripts": { "start:dev": "webpack serve" }
```



+ webpack-dev-server 会开启一个服务器，默认是8080
+ webpack-dev-server不会生成一个实际的文件，不会在dist文件夹下生成一个实际的文件，可以理解为最终的资源只存在于内存当中，当浏览器发送请求的时候，会从内存中去加载，然后返回打包后的资源结果，它不会占用dist下的实际资源。

修改服务端口

**webpack.config.js**

```js
const path = require('path')
module.exports = {
  entry: './app.js',  // 工程资源的入口
  output: {
    path: path.join(__dirname, 'dist'), // path必须是绝对路径
    filename: 'bundle.js'
  },
  devServer: {
    port: 3000, // 指定服务端口，默认是8080
    publicPath: 'dist'
  }
}
```

### 三、文件加载器loader

```js
const path = require('path')
const UglifyJSPlugin = require('uglifyjs-webpack-plugin')
module.exports = {
  entry: './app.js',  // 工程资源的入口
  output: {
    path: path.join(__dirname, 'dist'), // path必须是绝对路径
    filename: 'bundle.js'
  },
  devServer: {
    port: 3000, // 指定服务端口，默认是8080
    publicPath: 'dist'
  },

  //  文件加载器loader 
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader', // 这里注意顺序问题，sytyle-loader要在css-loader前面
          // 加载顺序和实际上写的顺序是相反的,放到后面的优先加载
          'css-loader'
        ]
      }
    ]
  },
  plugins: [
    new UglifyJSPlugin()
  ]
}



```



### 四、plugin



### 五、babel



### 六、热更新

在不刷新页面的情况下，内容更新了

### 七、打包结果优化

```js
const TerserPlugin = require('terser-webpack-plugin')
const BundleAnalyZerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin

module.exports = {
    optimization: {
        minimizer: [new TerserPlugin({
            // 加快构建速度
            cache: true,
            terserOptions: {
                compress: {
                    unused: true,
                    drop_debugger: true,
                    drop_console: true,
                    drop_code: true
                }
            }
        })]
    },
    plugins: [
        new BundleAnalyZerPlugin()
    ]
}
```



webpack分析器

```shell
npm install webpack-bundle-analyzer
```





