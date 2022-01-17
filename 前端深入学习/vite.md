## vite学习

### 起步

+ vite通过在一开始将应用中的模块分为**依赖**和**源码**两类，改进了开发服务器启动时间

**依赖**：大多为在开发时不会变动的纯 JavaScript。

**源码**：通常包含一些并非直接是 JavaScript 的文件，需要转换（例如 JSX，CSS 或者 Vue/Svelte 组件），时常会被编辑。同时，并不是所有的源码都需要同时被加载（例如基于路由拆分的代码模块）

+ 在一个 Vite 项目中，`index.html` 在项目最外层而不是在 `public` 文件夹内。这是有意而为之的：在开发期间 Vite 是一个服务器，而 `index.html` 是该 Vite 项目的入口文件。
+ `tsconfig.json` 中 `compilerOptions` 下的一些配置项需要特别注意。

1. `isolatedModules`应该设置为 `true`。

2. 这是因为 `esbuild` 只执行没有类型信息的转译，它并不支持某些特性，如 `const enum` 和隐式类型导入。
3. 必须在 `tsconfig.json` 中的 `compilerOptions` 下设置 `"isolatedModules": true`。如此做，TS 会警告你不要使用隔离（isolated）转译的功能。

### 功能

#### NPM依赖解析和预构建

##### **依赖是强缓存的**

Vite 通过 HTTP 头来缓存请求得到的依赖，所以如果你想要编辑或调试一个依赖，请按照 [这里](https://vitejs.cn/guide/dep-pre-bundling.html#浏览器缓存) 的步骤操作。

#### 模块热重载

Vite 提供了一套原生 ESM 的 [HMR API](https://vitejs.cn/guide/api-hmr.html)。 具有 HMR 功能的框架可以利用该 API 提供即时、准确的更新，而无需重新加载页面或清除应用程序状态。Vite 内置了 HMR 到 [Vue 单文件组件（SFC）](https://github.com/vitejs/vite/tree/main/packages/plugin-vue) 和 [React Fast Refresh](https://github.com/vitejs/vite/tree/main/packages/plugin-react) 中。也通过 [@prefresh/vite](https://github.com/JoviDeCroock/prefresh/tree/main/packages/vite) 对 Preact 实现了官方集成。

#### TypeScript

Vite 天然支持引入 `.ts` 文件。

Vite 仅执行 `.ts` 文件的转译工作，并 **不** 执行任何类型检查。

### TypeScript 编译器选项

`tsconfig.json` 中 `compilerOptions` 下的一些配置项需要特别注意。

`isolatedModules`应该设置为 `true`。

#### JSON

JSON 可以被直接导入 —— 同样支持具名导入

```js
// 导入整个对象
import json from './example.json'
// 对一个根字段使用具名导入 —— 有效帮助 treeshaking！
import { field } from './example.json'
```

####  Glob 导入

Vite 支持使用特殊的 `import.meta.glob` 函数从文件系统导入多个模块：

````js
const modules = import.meta.glob('./dir/*.js')
````

以上将会被转译为下面的样子

```js
// vite 生成的代码
const modules = {
  './dir/foo.js': () => import('./dir/foo.js'),
  './dir/bar.js': () => import('./dir/bar.js')
}

```

你可以遍历 `modules` 对象的 key 值来访问相应的模块：

```js
for (const path in modules) {
  modules[path]().then((mod) => {
    console.log(path, mod)
  })
}
```

匹配到的文件默认是懒加载的，通过动态导入实现，并会在构建时分离为独立的 chunk。如果你倾向于直接引入所有的模块（例如依赖于这些模块中的副作用首先被应用），你可以使用 `import.meta.globEager` 代替：

```js
const modules = import.meta.globEager('./dir/*.js')
```

以上会被转译为下面的样子：

```js
// vite 生成的代码
import * as __glob__0_0 from './dir/foo.js'
import * as __glob__0_1 from './dir/bar.js'
const modules = {
  './dir/foo.js': __glob__0_0,
  './dir/bar.js': __glob__0_1
}
```

- 这只是一个 Vite 独有的功能而不是一个 Web 或 ES 标准
- 该 Glob 模式会被当成导入标识符：必须是相对路径（以 `./` 开头）或绝对路径（以 `/` 开头，相对于项目根目录解析）。
- Glob 匹配是使用 `fast-glob` 来实现的 —— 阅读它的文档来查阅 [支持的 Glob 模式](https://github.com/mrmlnc/fast-glob#pattern-syntax)。
- 你还需注意，glob 的导入不接受变量，你应直接传递字符串模式。

#### WebAssembly

预编译的 `.wasm` 文件可以直接被导入 —— 默认导出一个函数，返回值为所导出 wasm 实例对象的 Promise

#### CSS代码分割

Vite 会自动地将一个异步 chunk 模块中使用到的 CSS 代码抽取出来并为其生成一个单独的文件。这个 CSS 文件将在该异步 chunk 加载完成时自动通过一个 `<link>` 标签载入，该异步 chunk 会保证只在 CSS 加载完毕后再执行，避免发生 [FOUC](https://en.wikipedia.org/wiki/Flash_of_unstyled_content#:~:text=A flash of unstyled content,before all information is retrieved.) 。

如果你更倾向于将所有的 CSS 抽取到一个文件中，你可以通过设置 [`build.cssCodeSplit`](https://vitejs.cn/config/#build-csscodesplit) 为 `false` 来禁用 CSS 代码分割。

#### 预加载指令生成

如果你更倾向于将所有的 CSS 抽取到一个文件中，你可以通过设置 [`build.cssCodeSplit`](https://vitejs.cn/config/#build-csscodesplit) 为 `false` 来禁用 CSS 代码分割。

### 使用插件

#### 添加一个插件

若要使用一个插件，需要将它添加到项目的 `devDependencies` 并在 `vite.config.js` 配置文件中的 `plugins` 数组中引入它。例如，要想为传统浏览器提供支持，可以按下面这样使用官方插件 [@vitejs/plugin-legacy](https://github.com/vitejs/vite/tree/main/packages/plugin-legacy)

```shell
$ npm i -D @vitejs/plugin-legacy
```

```js
// vite.config.js
import legacy from '@vitejs/plugin-legacy'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
    legacy({
      targets: ['defaults', 'not IE 11']
    })
  ]
})
```

#### 查找插件



#### 强制插件排序

为了与某些 Rollup 插件兼容，可能需要强制执行插件的顺序，或者只在构建时使用。这应该是 Vite 插件的实现细节。可以使用 `enforce` 修饰符来强制插件的位置

- `pre`：在 Vite 核心插件之前调用该插件
- 默认：在 Vite 核心插件之后调用该插件
- `post`：在 Vite 构建插件之后调用该插件

```js
// vite.config.js
import image from '@rollup/plugin-image'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
    {
      ...image(),
      enforce: 'pre'
    }
  ]
})
```

#### 按需引用

默认情况下插件在开发 (serve) 和生产 (build) 模式中都会调用。如果插件在服务或构建期间按需使用，请使用 `apply` 属性指明它们仅在 `'build'` 或 `'serve'` 模式时调用：

```js
// vite.config.js
import typescript2 from 'rollup-plugin-typescript2'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
    {
      ...typescript2(),
      apply: 'build'
    }
  ]
})

```

### 依赖预构建

#### 原因

Vite 执行的所谓的“依赖预构建”。这个过程有两个目的:

1. **CommonJS 和 UMD 兼容性:** 开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将作为 CommonJS 或 UMD 发布的依赖项转换为 ESM。

   当转换 CommonJS 依赖时，Vite 会执行智能导入分析，这样即使导出是动态分配的（如 React），按名导入也会符合预期效果：



2. **性能：** Vite 将有许多内部模块的 ESM 依赖关系转换为单个模块，以提高后续页面加载性能。

一些包将它们的 ES 模块构建作为许多单独的文件相互导入。例如，[`lodash-es` 有超过 600 个内置模块](https://unpkg.com/browse/lodash-es/)！当我们执行 `import { debounce } from 'lodash-es'` 时，浏览器同时发出 600 多个 HTTP 请求！尽管服务器在处理这些请求时没有问题，但大量的请求会在浏览器端造成网络拥塞，导致页面的加载速度相当慢。

通过预构建 `lodash-es` 成为一个模块，我们就只需要一个 HTTP 请求了！

#### 自动依赖搜寻

如果没有找到相应的缓存，Vite 将抓取你的源码，并自动寻找引入的依赖项（即 "bare import"，表示期望从 `node_modules` 解析），并将这些依赖项作为预构建包的入口点。预构建通过 `esbuild` 执行，所以它通常非常快。

在服务器已经启动之后，如果遇到一个新的依赖关系导入，而这个依赖关系还没有在缓存中，Vite 将重新运行依赖构建进程并重新加载页面。

#### 缓存

Vite 会将预构建的依赖缓存到 `node_modules/.vite`。它根据几个源来决定是否需要重新运行预构建步骤:

- `package.json` 中的 `dependencies` 列表
- 包管理器的 lockfile，例如 `package-lock.json`, `yarn.lock`，或者 `pnpm-lock.yaml`
- 可能在 `vite.config.js` 相关字段中配置过的

只有在上述其中一项发生更改时，才需要重新运行预构建。

如果出于某些原因，你想要强制 Vite 重新构建依赖，你可以用 `--force` 命令行选项启动开发服务器，或者手动删除 `node_modules/.vite` 目录。

只有在上述其中一项发生更改时，才需要重新运行预构建。

#### 浏览器缓存



### 静态资源处理

#### 将资源引入为URL

服务室引入一个静态资源会返回解析后的公共路径



#### 显示URL引入



#### 将资源引入为字符串



#### 导入脚本作为Woker



#### public目录

如果你有下列这些资源：

+ 不会被源码引用（例如 `robots.txt`）
+ 必须保持原有文件名（没有经过 hash）
+ ...或者你压根不想引入该资源，只是想得到其URL

那么你可以将该资源放在指定的`public`目录中，它位于你的项目根目录。该目录中的资源在开发时能直接通过 `/` 根路径访问到，并且打包时会被完整复制到目标目录的根目录下。



#### new URL(url, import.meta.url)

[import.meta.url](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import.meta) 是一个 ESM 的原生功能，会暴露当前模块的 URL。将它与原生的 [URL 构造器](https://developer.mozilla.org/en-US/docs/Web/API/URL) 组合使用，在一个 JavaScript 模块中，通过相对路径我们就能得到一个被完整解析的静态资源 URL：



### 构建生产版本

#### 浏览器兼容性

可以通过 [`build.target` 配置项](https://vitejs.cn/config/#build-target) 指定构建目标，最低支持 `es2015`。

#### 公共基础路径

如果你需要在嵌套的公共路径下部署项目，只需指定 [`base` 配置项](https://vitejs.cn/config/#base)，然后所有资源的路径都将据此配置重写。这个选项也可以通过命令行参数指定，例如 `vite build --base=/my/public/path/`。

#### 自定义构建

构建过程可以通过多种 [构建配置选项](https://vitejs.cn/config/#build-options) 来自定义构建。具体来说，你可以通过 `build.rollupOptions` 直接调整底层的 [Rollup 选项](https://rollupjs.org/guide/en/#big-list-of-options)：

```js
// vite.config.js
module.exports = defineConfig({
  build: {
    rollupOptions: {
      // https://rollupjs.org/guide/en/#big-list-of-options
    }
  }
})
```

#### 文件变化时重新构建

可以使用`vite build --watch` 来启用 rollup 的监听器。或者，你可以直接通过 `build.watch` 调整底层的 [`WatcherOptions`](https://rollupjs.org/guide/en/#watch-options) 选项：

```js
// vite.config.js
module.exports = defineConfig({
  build: {
    watch: {
      // https://rollupjs.org/guide/en/#watch-options
    }
  }
})
```



### 环境变量与模式

Vite 在一个特殊的 **`import.meta.env`** 对象上暴露环境变量。这里有一些在所有情况下都可以使用的内建变量：

- **`import.meta.env.MODE`**: {string} 应用运行的[模式](https://vitejs.cn/guide/env-and-mode.html#modes)。
- **`import.meta.env.BASE_URL`**: {string} 部署应用时的基本 URL。他由[`base` 配置项](https://vitejs.cn/config/#base)决定。
- **`import.meta.env.PROD`**: {boolean} 应用是否运行在生产环境。
- **`import.meta.env.DEV`**: {boolean} 应用是否运行在开发环境 (永远与 `import.meta.env.PROD`相反)。

#### .env文件







### 配置

#### 配置文件解析

+ 当以命令行方式运行vite时，vite会自动解析项目根目录下名为vite.config.js的文件
+ 注意：即使项目没有在 `package.json` 中开启 `type: "module"`，Vite 也支持在配置文件中使用 ESM 语法。这种情况下，配置文件会在被加载前自动进行预处理
+ 需要注意的是，在 Vite 的 API 中，在开发环境下 `command` 的值为 `serve`（在 CLI 中， `vite dev` 和 `vite serve` 是 `vite` 的别名），而在生产环境下为 `build`（`vite build`）。

#### 共享配置

##### 1. root

**默认：**process.cwd()

项目根目录（`index.html` 文件所在的位置）。可以是一个绝对路径，或者一个相对于该配置文件本身的相对路径。

##### 2. base

开发或生产环境服务的公共基础路径。合法的值包括以下几种：

- 绝对 URL 路径名，例如 `/foo/`
- 完整的 URL，例如 `https://foo.com/`
- 空字符串或 `./`（用于开发环境）

##### 3. mode

##### **默认：** `'development'`（开发模式），`'production'`（生产模式）

在配置中指明将会把 **serve 和 build** 时的模式 **都** 覆盖掉。也可以通过命令行 `--mode` 选项来重写。

##### 4. define

##### 5. plugins



##### 6. publicDir

作为静态资源服务的文件夹。该目录中的文件在开发期间在 `/` 处提供，

##### 7. cacheDir

存储缓存文件的目录。此目录下会存储预打包的依赖项或 vite 生成的某些缓存文件，使用缓存可以提高性能。如需重新生成缓存文件，你可以使用 `--force` 命令行选项或手动删除目录。此选项的值可以是文件的绝对路径，也可以是以项目根目录为基准的相对路径

##### 8. resolve.alias

当使用文件系统路径的别名时，请始终使用绝对路径。相对路径的别名值会原封不动地被使用，因此无法被正常解析。

##### 9. resolve.mainFields

##### 10. resolve.extensions

导入时想要省略的扩展名列表。注意，**不** 建议忽略自定义导入类型的扩展名（例如：`.vue`），因为它会影响 IDE 和类型支持。

##### 11. css.modules

##### 12. css.postcss

##### 13. json.namedExports

是否支持从 `.json` 文件中进行按名导入。

##### 14. json.stringify

若设置为 `true`，导入的 JSON 会被转换为 `export default JSON.parse("...")`，这样会比转译成对象字面量性能更好，尤其是当 JSON 文件较大的时候。

开启此项，则会禁用按名导入。

##### 15. esbuild

+ **类型：** `ESBuildOptions | false`:`ESBuildOptions` 继承自 [ESbuild 转换选项](https://esbuild.github.io/api/#transform-api)。最常见的用例是自定义 JSX：

```js
export default defineConfig({
  esbuild: {
    jsxFactory: 'h',
    jsxFragment: 'Fragment'
  }
})
```

默认情况下，ESbuild 会被应用在 `ts`、`jsx`、`tsx` 文件。你可以通过 `esbuild.include` 和 `esbuild.exclude` 对要处理的文件类型进行配置，这两个配置的类型应为 `string | RegExp | (string | RegExp)[]`。

此外，你还可以通过 `esbuild.jsxInject` 来自动为每一个被 ESbuild 转换的文件注入 JSX helper。

+ 设置为 `false` 来禁用 ESbuild 转换。

##### 16. envDir

用于加载 `.env` 文件的目录。可以是一个绝对路径，也可以是相对于项目根的路径

##### 17. envPrefix

以 `envPrefix` 开头的环境变量会通过 import.meta.env 暴露在你的客户端源码中



#### 开发服务器选项

##### 1. server.host

指定服务器应该监听哪个 IP 地址。 如果将此设置为 `0.0.0.0` 或者 `true` 将监听所有地址，包括局域网和公网地址。

##### 2. server.port

指定开发服务器端口。注意：如果端口已经被使用，Vite 会自动尝试下一个可用的端口，所以这可能不是开发服务器最终监听的实际端口。

##### 3. server.strictPort

设为 `true` 时若端口已被占用则会直接退出，而不是尝试下一个可用端口。

##### 4. server.https

启用 TLS + HTTP/2。注意：当 [`server.proxy` 选项](https://vitejs.cn/config/#server-proxy) 也被使用时，将会仅使用 TLS。

这个值也可以是一个传递给 `https.createServer()` 的 [选项对象](https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener)。

##### 5. server.open

在开发服务器启动时自动在浏览器中打开应用程序。当此值为字符串时，会被用作 URL 的路径名。若你想指定喜欢的浏览器打开服务器，你可以设置环境变量 `process.env.BROWSER`（例如：`firefox`）。查看 [这个 `open` 包](https://github.com/sindresorhus/open#app) 获取更多细节。

##### 6. server.proxy

为开发服务器配置自定义代理规则。期望接收一个 `{ key: options }` 对象。如果 key 值以 `^` 开头，将会被解释为 `RegExp`。`configure` 可用于访问 proxy 实例。

使用 [`http-proxy`](https://github.com/http-party/node-http-proxy)。完整选项详见 [此处](https://github.com/http-party/node-http-proxy#options).

##### 7. server.cors

为开发服务器配置 CORS。默认启用并允许任何源，传递一个 [选项对象](https://github.com/expressjs/cors) 来调整行为或设为 `false` 表示禁用。

##### 8. server.force

设置为 `true` 强制使依赖预构建。

##### 9. server.hmr

禁用或配置 HMR 连接（用于 HMR websocket 必须使用不同的 http 服务器地址的情况）。

##### 10. server.watch

##### 11. server.middlewareMode

##### 12. server.fs.strict

限制为工作区 root 路径以外的文件的访问。

##### 13. server.fs.allow

限制哪些文件可以通过 `/@fs/` 路径提供服务。当 `server.fs.strict` 设置为 true 时，访问这个目录列表外的文件将会返回 403 结果。

##### 14. server.fs.deny

用于限制 Vite 开发服务器提供敏感文件的黑名单。

##### 15. server.origin

用于定义开发调试阶段生成资产的 origin。

#### 构建选项

##### 1. build.target

设置最终构建的浏览器兼容目标。默认值是一个 Vite 特有的值——`'modules'`，这是指 [支持原生 ES 模块的浏览器](https://caniuse.com/es6-module)。

另一个特殊值是 “esnext” —— 即假设有原生动态导入支持，并且将会转译得尽可能小：

- 如果 [`build.minify`](https://vitejs.cn/config/#build-minify) 选项为 `'terser'`， `'esnext'` 将会强制降级为 `'es2019'`。
- 其他情况下将完全不会执行转译。

转换过程将会由 esbuild 执行，

##### 2. build.polyfillModulePreload



##### 3. build.outDir

指定输出路径（相对于 [项目根目录](https://vitejs.cn/guide/#index-html-and-project-root)).

##### 4. build.assetsDir

指定生成静态资源的存放路径（相对于 `build.outDir`）。

##### 5. build.assetsInlineLimit

小于此阈值的导入或引用资源将内联为 base64 编码，以避免额外的 http 请求。设置为 `0` 可以完全禁用此项。

**注意:** 如果你指定了 `build.lib`，那么 `build.assetsInlineLimit` 将被忽略，无论文件大小，资源都会被内联

##### 6. build.cssCodeSplit

启用/禁用 CSS 代码拆分。当启用时，在异步 chunk 中导入的 CSS 将内联到异步 chunk 本身，并在其被加载时插入。

如果禁用，整个项目中的所有 CSS 将被提取到一个 CSS 文件中。

**注意:** 如果指定了 `build.lib`，`build.cssCodeSplit` 会默认为 `false`。

##### 7. build.cssTarget

此选项允许用户为 CSS 的压缩设置一个不同的浏览器 target，此处的 target 并非是用于 JavaScript 转写目标

##### 8. build.rollupOptions

自定义底层的 Rollup 打包配置。这与从 Rollup 配置文件导出的选项相同，并将与 Vite 的内部 Rollup 选项合并。查看 [Rollup 选项文档](https://rollupjs.org/guide/en/#big-list-of-options) 获取更多细节。

##### 9. build.lib

构建为库。`entry` 是必须的因为库不能使用 HTML 作为入口。`name` 则是暴露的全局变量，在 `formats` 包含 `'umd'` 或 `'iife'` 时是必须的。默认 `formats` 是 `['es', 'umd']` 。`fileName` 是输出的包文件名，默认 `fileName` 是 `package.json` 的 `name` 选项，同时，它还可以被定义为参数为 `format` 的函数。

##### 10. build.manifest

##### 11. build.ssrManifest

##### 12. build.ssr

##### 13. build.minify

设置为 `false` 可以禁用最小化混淆，或是用来指定使用哪种混淆器。默认为 [Esbuild](https://github.com/evanw/esbuild)，它比 terser 快 20-40 倍，压缩率只差 1%-2%

##### 14. build.write

设置为 `false` 来禁用将构建后的文件写入磁盘。这常用于 [编程式地调用 `build()`](https://vitejs.cn/guide/api-javascript.html#build) 在写入磁盘之前，需要对构建后的文件进行进一步处理。

##### 15. build.emptyOutDir

默认情况下，若 `outDir` 在 `root` 目录下，则 Vite 会在构建时清空该目录。若 `outDir` 在根目录之外则会抛出一个警告避免意外删除掉重要的文件。可以设置该选项来关闭这个警告。该功能也可以通过命令行参数 `--emptyOutDir` 来使用。

##### 16. build.brotliSize

启用/禁用 brotli 压缩大小报告。压缩大型输出文件可能会很慢，因此禁用该功能可能会提高大型项目的构建性能。

##### 17. build.chunkSizeWarningLimit

chunk 大小警告的限制（以 kbs 为单位）

##### 18. build.watch

设置为 `{}` 则会启用 rollup 的监听器。在涉及只用在构建时的插件时和集成开发流程中很常用。

#### 预览选项

##### 1. preview.host

为开发服务器指定 ip 地址。 设置为 `0.0.0.0` 或 `true` 会监听所有地址，包括局域网和公共地址。

##### 2. preview.port

指定开发服务器端口。注意，如果设置的端口已被使用，Vite 将自动尝试下一个可用端口，所以这可能不是最终监听的服务器端口。

##### 3. preview.strictPort

设置为 `true` 时，如果端口已被使用，则直接退出，而不会再进行后续端口的尝试。

##### 4. preview.https

启用 TLS + HTTP/2。注意，只有在与 [`server.proxy` 选项](https://vitejs.cn/config/#server-proxy) 同时使用时，才会降级为 TLS。

##### 5. preview.open

开发服务器启动时，自动在浏览器中打开应用程序。当该值为字符串时，它将被用作 URL 的路径名。如果你想在你喜欢的某个浏览器打开该开发服务器，你可以设置环境变量 `process.env.BROWSER` （例如 `firefox`）。欲了解更多细节，请参阅 [`open` 包的源码](https://github.com/sindresorhus/open#app)。

##### 6. preview.proxy

为开发服务器配置自定义代理规则。其值的结构为 `{ key: options }` 的对象。如果 key 以 `^` 开头，它将被识别为 `RegExp`，其中 `configure` 选项可用于访问代理实例。

##### 7. preview.cors

为开发服务器配置 CORS。此功能默认启用，支持任何来源。可传递一个 [options 对象](https://github.com/expressjs/cors) 来进行配置，或者传递 `false` 来禁用此行为。

#### 依赖优化选项

##### 1. optimizeDeps.entries

默认情况下，Vite 会抓取你的 index.html 来检测需要预构建的依赖项。如果指定了 `build.rollupOptions.input`，Vite 将转而去抓取这些入口点。

##### 2. optimizeDeps.exclude

在预构建中强制排除的依赖项。

##### 3. optimizeDeps.include

默认情况下，不在 `node_modules` 中的，链接的包不会被预构建。使用此选项可强制预构建链接的包。

##### 4. optimizeDeps.keepNames

打包器有时需要重命名符号以避免冲突。 设置此项为 `true` 可以在函数和类上保留 `name` 属性。 若想获取更多详情，请参阅 [`keepNames`](https://esbuild.github.io/api/#keep-names)

#### SSR选项

##### 1. ssr.external

列出的是要为 SSR 强制外部化的依赖。

##### 2. ssr.noExternal

列出的是防止被 SSR 外部化依赖项。如果设为 `true`，将没有依赖被外部化。

##### 3. ssr.target

SSR 服务器的构建目标。