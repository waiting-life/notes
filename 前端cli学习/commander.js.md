## commander.js 学习

### 声明 program 变量

```js
const { program } = require("commander");
program.version("0.0.1");
```

要在 ECMAScript 模块中使用命名导入，可从`commander/esm.mjs`中导入。

```js
// index.mjs
import { Command } from 'commander/esm.mjs';
const program = new Command();
```

### 选项

1. Commander使用`.option()`方法来定义选项。每个选项可以定义一个短选项名称(-后面接单个字符)和一个长选项名称(--后面接一个或多个单词)，使用逗号，空格或`|`分隔。

2. 解析后的选项可以通过`Commander`对象上的`.opts()`方法获取，同事会被传递给命令处理函数。可以使用`.getOptionValue()`和`.setOptionValue()`操作单个选项的值。
3. 对于多个单词的长选项，选项名会转为驼峰命名法，例如`--template-engine`选项会通过`program.opts().templateEngine`获取
4. 多个短选项可以合并简写，其中最后一个选项可以附加参数。 例如，`-a -b -p 80`也可以写为`-ab -p80`，甚至进一步简化为`-abp80`。`--`可以标记选项的结束，后续的参数均不会被命令解释，可以正常使用。

5. 通过`program.parse(arguments)`方法处理参数，没有被使用的选项会存放在`program.args`数组中。该方法的参数是可选的，默认值为`process.argv`

