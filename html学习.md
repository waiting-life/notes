# CSS学习

## 一、起步

### 1. 浏览器



+ 常用浏览器

浏览器是网页显示、运行平台。常用的浏览器有IE、火狐（Firefox）、谷歌（chrome）、Safari和Opera

+ 浏览器内核

浏览器内核（渲染引擎）：负责读取网页内容，整理讯息，计算网页的显示方式并显示页面

### 2. Web标准

#### 2.1 为什么需要web标准

浏览器不同，它们显示的页面或者排版就有些差异。

### 2.2 web标准的构成

主要包括结构，表现，和行为三个方面（HTML,CSS,Javascript）



## 二、标签

### 1.了解

+ 文档类型声明标签

```
<!DOCTYPE html>
```

+ lang语言种类

1. en定义为语言英语
2. zh-CN定义语言为中文

简单来说，定义为en就是英文网页，定义为zh-CN就是中文网页

其实对于文档显示来说，定义为en也可以显示中文，定义成zh-CN的文档也可以显示英文

这个属性对浏览器和搜索引擎还是有用的

```
<html lang="zh-CN">
```

3. 字符集

字符集是多个字符的集合。以便计算机能够识别和存储各种文字

在```<head>```标签内，可以通过```<meta>```标签的charset属性来规定HTML文档应该使用哪种编码

```
<meta charset="UTF-8"/>
```

### 2. HTML常用标签

#### 2.1图形标签

```
<img scr="图像url"/>
```

alt:替换文本，图片显示不出来的时候用文字替换

title：提示文本，鼠标放到图片上提示的文字

width: 设置图像的宽度

height：设置图像的高度

**注意**：一般要么修改宽度要么修改高度，只需要修改一个，会等比缩放，不会有失真，两个都修改会出现压缩等情况

border：设置图像的边框粗细 //一般通过css来设置



+ 路径

1. 相对路径：图片相对于html页面的位置
2. 绝对路径：是指目录下的绝对位置，直接到达目标位置，通常是从盘符开始的路径



#### 2.2 超链接标签

```
<a></a>
```

herf:用于指定目标的url地址，当为标签应用href属性时，它就具有了超链接的功能

target:用于指定链接页面的打开方式，其中_self为默认值，

_blank为在新窗口中打开方式

+ 下载文件

```
//地址链接的是文件.exe 或者zip等压缩包的形式
<a href="xxx.zip">下载文件</a>
```

锚点链接：当我们点击链接，可以快速定位到页面中的某个位置

+ 在链接文本的href属性中，设置属性值为==#名字==的形式如：```<a href="#two">第二集</a>```

+ 找到目标位置标签，里面添加一个id属性 = 刚才的名字，如```<h3 id="two">第二集介绍</h3>```



#### 2.3 特殊字符

空格： &nbsp

大于号> :  &gt

小于号< :  &lt

 #### 2.4 表格标签

1. ```<table></table>```用于定义表格的标签



2. ```<tr></tr>```标签用于定义表格中的行，必须嵌套在```<table></table>```标签中
3. ```<td></td>```用于定义表格中的单元格，必须嵌套在```<tr></tr>```中
4. ```<th></th>```表头单元格里面的文本内容**加粗居中**显示

align:规定表格相对周围的对齐方式

border：1 或""  规定表格单元是否拥有边框，默认为""，表示没有边框

cellpadding:    规定单元边沿与其内容之间的空白，默认为 1像素

cellspacing: 	规定单元格之间的空白，默认为2像素

width

单元格里面可以放任何元素，文字链接图片等都可以

合并单元格方式：

+ 跨行合并： ```rowspan="合并单元格个数"```
+ 跨列合并：```colspan="合并单元格的个数"```

合并单元格三部曲：

1. 先确定是跨行还是跨列

2. 找到目标单元格，

   跨行：最上侧单元格为目标单元格，写合并代码

   跨列：最左侧单元格为目标单元格，写合并代码

3. 删除多余单元格

#### 2.5 列表标签

表格是用来显示数据的

列表是用来布局的

+ 无序列表

```html
<ul></ul>里面只能放<li></li>标签
<li></li>标签里面可以放所有元素
```

+ 有序列表

```html
<ol></ol>也只能放<li></li>标签

```

+ 自定义列表:自定义列表用于对术语和名词进行解释和描述，定义列表的列表项谦没有任何项目符号

```html
<dl>
	<dt>名词1<dt>
	<dd>名词1解释1</dd>
	<dd>名词1解释2</dd>
</dl>
```

#### 2.6 表单标签

使用表单的目的是为了收集用户信息

一个完整的表单通常由表单域（表单区域，把所有表单元素包起来），表单控件，和提示信息（文字信息）三个部分构成

表单域是一个包含表单元素的区域

在HTML标签中，```<form>```标签用于定义表单域，以实现用户信息的收集和传递

==<form>会把它范围内的表单元素信息提交给服务器==

```html
<form action="url地址" method="提交方式" name="表单域名称"></form>
```



常用属性：

+ action      url地址          用于指定接受并处理表单数据的服务器程序的url地址
+ method    get/post       用于设置表单数据的提交方式
+ name        名称               用于指定表单的名称，以区分同一个页面中的多个表单域

1. <input>标签

+ <input>表单元素

属性： 

**type**

+ button
+ checkbox
+ file
+ hidden
+ image
+ password
+ radio
+ reset
+ submit：定义提交按钮。提交按钮会把表单数据发送到服务器。
+ text

**name**:*name是表单元素的名字，后台可以通过这个name属性找到这个表单，页面中的表单很多，name的主要作用就是用于区别不同的表单*

**注意**：1. 单选框和复选框必须有相同的name值

​			2. name和value是每个表单元素都有的属性值，主要给后台人员		使用。

当性别单选按钮的时候，必须有相同的名字才可以实现多选一

```
<input type="radio" name="sex" checked="checked"/>男
<input type="radio" name="sex"/>女
```

**value**

**checked**:规定此input元素首次加载时被选中，主要针对单选按钮和复选按钮，主要用于一打开页面就可以默认选中某个表单元素

**maxlength**

+ label标签

<label>标签为input元素定义标注

<label>标签用于绑定一个表单元素，当点击<label>标签内的文本时，浏览器就会自动将焦点(光标)转到或者选择对应的表单元素上，用来增加用户体验。

**注意**：<label>标签的for属性应当与相关元素的id属性相同

```html
<label for="username">用户名</label>
  <input type="text" name="username" id="username"><br/>
  <label for="psw">密码</label>
  <input type="password" name="psw" id="psw">

  <input type="radio" id="man" name="sex"><label for="man">男</label>
  <input type="radio" id="woman" name='sex'><label for="woman">女</label>
```

2. select下拉表单列表

```html
<body>
  籍贯:
  <select>
    <option value="">山东</option>
    <option value="">北京</option>
    <option value="">天津</option>
    <option value="" selected="selected">火星</option>
  </select>
</body>
```



+ 注意：<select>中至少包含一对<option>

+ 在<option>中定义selected = “selected”时，当前即为默认选中项

3. textarea文本域元素：<textarea>标签是用于定义多行文本输入的控件

```html
<form action="">
    今日反馈：
    <textarea rows="3" cols="20">啦啦啦啦啦啦啦啦啦啦</textarea>
  </form>
```



#### 2.7 标题标签

标题标签比较特殊，需要指定字体大小。

## 三、 CSS

#### CSS字体属性

+ font-weight: 

normal和400一样

**比如**：去掉标题的默认加粗

```
h3 {
	font-weight: 400;
}
```



bold等同于700

+ font-style

itallic

noramal：让倾斜的字体不倾斜

+ 复合属性font,有顺序

font:font-style font-weight font-size/line-height font-family



#### CSS文本属性

**color**

**text-align:** 设置元素内文本内容的水平对齐方式

text-align:left;  right;  center;

**text-decoration:**

none,underline,overline,line-through

**text-indent**首行缩进

em是一个相对单位，就是当前元素1个文字的大小，如果当前元素没有大小，则会按照父元素的1个文字大小。

2em就是两个文字大小

```
//是当前元素2个文字大小的距离，首行缩进2
text-indent: 2em
```

**line-height**:用于设置行间的距离（行高），可以控制文字与行之间的距离

**当字体大小和行高相等时，行高的上下间距就没有了，当行高设为大于字体大小时，就有上下间距了**

行高：上间距，文本高度，下间距组成

**让文字的行高等于盒子的高度，就可以让文字在当前盒子内垂直居中**

简单理解： 行高由上间距、文本高度、下间距组成。其中，文本就处于上间距和下间距中间。当把盒子高度和行高设为一样时，相当于把行高高度移到盒子里面，此时文本也处于盒子中间。

如果行高小于盒子的高度，偏上。

如果行高大于盒子的高度，偏下。

想要图片居中对齐，则是让他的父亲标签添加水平居中的代码

```
.pic {
	text-align: center
}

<div class="pic">
 <img/>
</div>
```

#### Emmet语法

快速生成html标签和css

```
.demo$*5
.demo*5{$}
```

每个单词第一个字母，快速生成css

快速格式化代码：右键格式化代码



#### 复合选择器

后代选择器：可以选择父元素里面子元素。 空格 ```div p```表示选中div里面的p元素```div p span```

子选择器：子元素选择器，只能作为某元素的最近一级子元素。```div>p```只选亲儿子

并集选择器： 并集选择器可以选择多组标签，同时为他们定义相同的样式，通常用于集体声明，任何选择器都可以作为并集选择器中的一部分div,p

伪类选择器：

1. 链接伪类选择器（顺序）

a:link :（没有点击过的）未访问的链接

a:visited： （点击过的）访问过的

a:hover

a:active

:hover

2. :focus伪类选择器

:focus伪类选择器用于选取获得焦点(光标)的表单元素

一般情况主要针对表单元素<input>

#### CSS元素显示模式

==块元素==

+ 独占一行
+ 高度、宽度、外边距以及内边距都可以控制
+ 宽度默认是容器(父级宽度的)100%
+ 是一个容器及盒子，里面可以放行内或者块级元素

**注意**：

+ 文字类的元素内不能使用块级元素
+ 比如<p>标签主要用于存放文字，因此<p>里面不能放块级元素，特别是不能放<div>
+ 同理，<h1>——<h6>等都是文字类块级标签，里面也不能放其他块级元素

==行内元素==

+ 相邻行内元素在一行上
+ 高、宽直接设置是无效的
+ 默认宽度即使它本身内容的宽度
+ 行内元素只能容纳文本或其他行内元素
+ 行内元素可以设置左右的margin

**注意**

+ 链接里面不能再放链接
+ 特殊情况下<a>里面可以放块级元素，但是给<a>转换一下块级模式最安全。

==行内块元素==

+ 在行内元素中有几个特殊的标签——<img/>、<input/>、<td>，**它们同时具有块元素和行内元素的特点**
+ 有些资料将他们称为行内块元素

行内块元素的特点：

+ 和相邻行内元素（行块内）在一行上，但是他们之间会有空白缝隙，一行可以显示多个（行内元素特点）
+ 默认宽度就是它本身内容的宽度（**行内元素特点**）
+ 高度、行高、外边距以及内边距都可以控制（**块级元素特点**）

==元素显示模式转换==

比如想要增加<a>标签的触发范围

```
//就可以给行内元素设置宽和高了,行边块
display: block;
//把块级元素转换为行内元素
display:inline
//行内块
display: inline-block;（会有大的缝隙）
```

#### 背景

1. backgrpoound-color,一般情况下背景颜色默认值是transparent（透明）

2. background-imgage: url()

3. background-repeat: no-repeat;

4. background-position: x y;参数代表的意思是x坐标和y坐标

+ 参数是方位名词：与顺序无关

很大的图，设置水平居中，可以使图中间核心的部分显示给我们

```css
background-position: center top;
```



```css
background-position: center top;

//下面两个效果一样
//如果只指定了一个方位名词，另一个省略，则第二个默认居中对齐

background-position： center right;

background-position: right center;

```

+ 精确单位

如果参数值是精确坐标，那么第一个肯定是x坐标，第二个肯定是y坐标

```css
background-position: 20px 40px;
```

+ 参数是混合单位

如果指定的两个值是精确单位和方位名词混合使用，第一个是x坐标

第二个值是y坐标

```css
//x 20px ycenter 等价与background-position:20px;
background-position: 20px center;

//水平居中 y 20px
background-position: center 20px;
```

5. background-attachment背景图像固定

设置背景图像事都固定或者随着页面的其余部分滚动

使用场景： 后期可以制作视差滚动的效果。

background-attachment: **scroll** |**fixed**

scroll；背景图像随着对象内容滚动、默认

fixed：背景图像固定

```css
background: #000 url(image/monv.jpg) no-repeat fixed center top;
```

6. 背景色透明

**注意**：背景色半透明是指盒子背景半透明，盒子里面内容不受影响

```css
background: rgba(0,0,0,0.3)
```

#### CSS三大特性

1. 层叠行

相同选择器给设置相同样式，此时会有一个样式就会覆盖另一个冲突的样式。层叠性主要解决样式冲突的问题

+ 样式冲突，遵循的是就近原则覆盖，把前面的覆盖。

+ 样式不冲突，不会层叠



2. 继承性

CSS中的继承：子标签会继承父标签某些样式，如文本颜色和字号。

（text-,font-,line-,这些元素开头的可以继承以及color属性）

行高的继承性：

+ 行高可以跟单位，也可以不跟单位
+ 如果子元素没有设置行高，则会继承父元素的行高为1.5
+ 此时子元素的行高是：当前子元素的文字大小*1.5
+ body行高1.5这样写最大的优势就是里面子元素可以根据自己的文字大小自动调整行高

```html
.myDiv{
      font: 12px/1.5 'Microsoft yahei'
      }
      .p1 {
      /* 子元素继承了父元素div的行高 1.5 这个1.5就是当前元素文字大小font-size的1.5倍*/
      /* 14*1.5 */
      font-size: 14px;
      }
     
     
  <div class="myDiv">
    <p class="p1">啦啦啦啦啦</p>
    <p class="p2">啦啦啦啦啦</p>
  </div>
```

3. 优先级

+ 当同一个元素指定多个选择器，就会有优先级的产生

  + 选择器相同，则执行层叠性
  + 选择器不同，则根据选择器权重执行

  !import > 行内样式style="">ID选择器>类选择器，伪类选择器>元素选择器>继承或*

  继承的权重是0

  **注意**：复合选择器会有权重叠加的问题

  ​			权重虽然会叠加，但是永远不会有进位的

  

#### 盒子模型

盒子模型包括：

边框、外边距、内边距、实际内容

**边框：border**

border： border-width ||border -style||border-color

```html
table,td,th {
      border: 1px solid pink;
      border-collapse: collapse;//合并相邻的边框  }
```

边框会影响实际盒子的大小

**内边距：padding**

padding属性用于设置内边距，即边框与内容之间的距离

当给盒子指定padding值之后，发生了两件事情：

1. 内容和边框有了距离，添加了内边距
2. padding影响了盒子实际大小，也就是说，如果盒子已经有了宽度和高度，此时再指定内边距会撑大盒子

解决方案：如果保证盒子和效果图大小一致，则width/height减去多出来的内边距大小

如果盒子本身没有指定width/height属性，则此时padding不会撑开盒子大小。

**外边距margin**

用于设置外边距，即盒子与盒子之间的距离

1. 外边距可以让**块级盒子**水平居中，但是必须满足两个条件

+ 盒子必须制定了宽度
+ 盒子左右的外边距都设置为auto

```css
//margin: 0 auto;只能让块级元素水平居中，不能让行内元素和行内块元素水平居中
width: 400px;
margin: 0 auto;
```

+ 行内元素或者行内块元素给其父元素添加```text-align:center```即可

+ 外边距合并： 使用margin定义块元素的垂直外边距时，可能会出现外边距的合并

2. 嵌套块元素垂直外边距的塌陷

+ 对于两个嵌套关系（父子关系）的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会塌陷较大的外边距值。

```html
//我们想让子元素距离父元素有个100px的边距
//按下面这样，父元素会有个距离上面100px的边距
//不是我们想要的
<style>
    .outer {
      width: 400px;
      height: 400px;
      background-color: purple;
      margin-top: 50px;
    }
    .inner {
      width: 200px;
      height: 200px;
      background-color: pink;
      margin-top: 100px;
    }
  </style>
</head>
<body>
  <div class="outer">
    <div class="inner">

    </div>
  </div>
</body>
```

**解决方法**

+ 可以为父元素定义上边框
+ 可以为父元素定义上内边距
+ **可以为父元素添加overflow:hidden;**

**清除内外边距**

ul有默认40px的内边距样式

body有默认8px的外边距

```css
//清除内外边距
* {
margin: 0;
padding: 0;
}
```

**注意**：行内元素为了照顾兼容性，尽量只设置左右内外边距，不要设置上下内外边距。但是转换为块级和行内块元素就可以了

设置了也没效果

```html
span {
    background-color: lightblue;
    margin: 0 20px;
}
    
<span>行内元素尽量只设置左右的内外边距</span>   
```

**圆角边框**

border-radius

原理：以所给长度画个圆，放在盒子的四角

**盒子阴影**

box-shadow

```box-shadow: h-shadow v-shadow blur spread color inset;```

h-shadow: 必需，水平阴影的位置，允许负值

v-shadow: 必需，垂直阴影的位置。允许负值。

blur;可选，模糊距离

spread： 可选，阴影的尺寸

color： 可选，阴影的颜色

inset：可选，将外部阴影（outset）改为内部阴影

**注意**：默认为outset，但是不能设置为outset，否则就不起效果

文字阴影：text-shadow

#### CSS浮动

传统网页布局的三种方式：

+ 普通流（标准流）
+ 浮动
+ 定位

float: left;

float: right;

网页布局第一准则：

+ 多个块级元素纵向排列找标准流

+ 多个块级元素横向排列找浮动

1. 浮动元素会脱离标准流
2. 浮动元素会一行内显示并且元素顶部对齐
3. 浮动元素会具有行内块元素的特性
4. 浮动的盒子不再保留原先的位置
5. 浮动的盒子中间是没有缝隙的，是紧挨在一起的
6. 行内元素同理

给一个行内元素span加了浮动后，可以给他设置宽高了，有了行内块元素的属性

```html
span {
    width: 200px;
    height: 160px;
    float: left;
    background-color: orangered;
}
<span></span>
```

不管原先是什么模式的元素，添加浮动之后具有行内块元素的特性

+ 如果块级盒子没有设置宽度，默认宽带和父级一样宽，但是添加浮动后，具有了行内块元素的特性，它的大小根据内容来决定。

```
 //因为没有给p设置宽度，不加浮动的话，它的宽度默认为父元素的宽度
 //加了浮动后，它具有行内块元素的特性，宽度由内容撑开
 p {
     height: 100px;
     background-color:blue;
     float: right;
 }
 <p>cpp</p>
```

**list-style: none;去除li标签的样式**

**常见网页布局**

1. 浮动布局和标准流的父盒子搭配

先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置。

2. 一个元素浮动了，理论上其他的兄弟元素也要浮动

**为什么需要清楚浮动**

+ 由于父盒子很多情况下，不方便给高度，但是子盒子浮动又不占有位置，最后父级盒子高度为0时，就会影响下面的标准流盒子。

+ 清楚浮动的本质是清除浮动元素造成的影响
+ 清楚浮动后，父元素会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流。
+ 子元素浮动脱离标准流后，不给父元素设置高度，父元素高度就为0，所以其他元素就上去了，会影响其他元素的布局

**清楚浮动的方法**

1. 额外标签法
2. 父级添加overflow属性
3. 父级添加after伪元素
4. 父级添加双伪元素

+ 额外标签法

多添加一个类名为clear的元素，这个新增的元素必须为块级元素不能为行内元素。给它设置样式clear: both;

```html
.box1-item1,.box1-item2 {
  width: 200px;
  height: 100px;
  float: left;
}
.box1-item1 {
  background-color:pink;
}
.box1-item2 {
  background-color:green;
}
.box2 {
  background-color: purple;
  height: 120px;
}

/*清除浮动*/
.clear {
  clear: both;
}

<div class="box1">
  <div class="box1-item1"></div>
  <div class="box1-item2"></div>
  <div class="clear"></div>
  </div>
<div class="box2"></div>
```

+ 父级添加overflow

```
.box1 {
  overflow: hidden;
}
```





清楚浮动后：父亲就有了高度，孩子有多高，父亲就有多高。

+ 父级添加after伪元素，给父元素添加clearfix类名

```html
.box1-item1,.box1-item2 {
  width: 200px;
  height: 100px;
  float: left;
}
.box1-item1 {
  background-color:pink;
}
.box1-item2 {
  background-color:green;
}
.box2 {
  background-color: purple;
  height: 120px;
}

.clearfix:after {
  content: "";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
.clearfix {
  *zoom: 1;
}


<div class="box1 clearfix">
  <div class="box1-item1"></div>
  <div class="box1-item2"></div>
</div>
<div class="box2"></div>
```

+ 双伪元素清楚浮动，也是给父元素添加

```html
.clearfix:before,.clearfix:after {
  content: "";
  display: table;
}
.clearfix:after {
  clear: both;
}
.clearfix {
  *zoom: 1;
}

<div class="box1 clearfix">
  <div class="box1-item1"></div>
  <div class="box1-item2"></div>
</div>
<div class="box2"></div>
```

为什么要清除浮动

1. 父级没高度
2. 子盒子浮动了
3. 影响下面布局了

#### 定位

定位=定位模式+边偏移

定位模式:static，relative，absolute，fixed

边偏移：top，right，left，bottom

1. 相对定位



2. 绝对定位

+ 如果**没有祖先元素或者祖先元素没有定位**，则以浏览器为准定位（document文档）
+ 绝对定位脱离标准流，不在占有原先的位置

3. 固定定位

固定定位贴着版心定位

固定定位小技巧：固定在版心右侧位置

小算法：

- 让固定定位的盒子left:50%;走到浏览器可视区（也可以看版心）的一半位置。
- 让固定定位的盒子margin-left:版心宽度的一半距离。夺走版心宽度的一半位置。

#### Css高级

**精灵图**



**三角**

```html
 .box3 {
     width: 0;
     height: 0;
     /* 四个边先设成透明 */
     border: 50px solid transparent;
     border-top-color:pink;
     margin-top: 20px;
 }

<div class="box3"></div>
```

**vertical-align**

CSS的`vertical-align`属性使用场景：经常用于设置图片或者表单（行内块元素）和文字垂直对齐。

官方解释：用于设置一个元素的垂直对齐方式，但是它只针对行内元素或者行内块元素

#### 学成在线案例

**导航栏注意点：**

+ 实际开发中，我们不会直接用连接而是用li包含连接的(li+a)的做法
+ 如果直接用a，搜索引擎容易辨别为有堆砌关键字嫌疑（故意堆砌关键字容易被搜索引擎有降权的风险，从而影响网站排名）



**注意：**外边距合并



清除浮动：

+ 浮动元素的父元素给特定的高度，底下的元素不会去上面。
+ 还有overflow：hidden
+ 一般用这个因为一般高度会变化，给浮动元素的父元素给个`clearfix`类名

添加样式：

```css
.clearfix:before,.clearfix:after {
  content:'';
  display: table;
}
.clearfix:after {
  clear: both;
}
.clearfix {
  *zoom: 1;
}
```

**定位特殊特性**x_x

1. 行内元素添加绝对定位或者固定定位，可以直接设置高度和宽度。
2. 块级元素添加绝对或者固定定位，如果不给宽度或者高度，默认是内容的大小。
3. 脱标的盒子不会触发外边距塌陷。
   + 浮动元素、绝对定位（固定定位）元素都不会触发外边距合并的问题

+ display属性用于设置一个元素如何显示

  + display隐藏元素后，不再占有原来的位置
+ visibility属性用于设置一个元素应该可见还是隐藏

  + visibility:visible; 元素可视
  + visibility:hidden；元素隐藏
    + visibility隐藏元素后，继续占有原来的位置 
+ overflow：属性指定了一个元素的框（超过指定高度及宽度）时，会发生什么

  + overflow: visible;默认
+ overflow:hidden;
  
  + overflow: scroll;  溢出的时候显示滚动条，不溢出的时候不显示滚动条
+ overflow: auto;  溢出的时候显示滚动条， 不溢出的时候不显示滚动条
+ 有·定位的时候慎用overflow:hidden;因为他会隐藏多余的部分

**img**

+ 只有行内元素和行内块元素才有 `vertical-align`

+ `vertical-align` 和行内块元素搭配，图片本身是个行内块元素，如果是块级元素，先转换成行内块元素。

+ `textarea` 也是行内块元素

+ `vertical:middle; ` 

+ 图片默认和文字的基线对齐，所以底下会有空隙

  + 解决方法：给图片添加 `vertical-align：middle|top|bottom` 改成其他这三种形式(提倡)
  + 把图片转换为块级原宿： `display: block`
  

**溢出的文字省略号代替**
+ 单行文本溢出显示省略号
  + 先强制一行内显示文本 white-space: nowrap; (默认 `normal`  自动换行)
  + 超出的部分隐藏 `overflow: hidden;`
  + 文字用省略号代替超出的部分：text-overflow: ellipsis;

```css
div {
  width: 150px;
  height: 80px;
  background-color: pink;
  margin: 100px auto;
  /* 1.如果文字显示不开也必须强制一行内显示 */
  white-space: nowrap;
  /* 2.溢出的部分隐藏起来 */
  overflow: hidden;
  /* 3.溢出的时候用省略号代替 */
  text-overflow: ellipsis;
}
```

```html
<div>
  老公程皮皮程皮皮吹泡泡臭屁屁
</div>
```



+ 多行文本溢出显示省略号
  + 多行文本溢出显示省略号，有较大兼容性问题，适合于 webKit 浏览器或移动端（移动端大部分是 webkit 内核） 

```css
div {
  width: 150px;
  height: 65px;
  background-color: pink;
  margin: 100px auto;
  /* 1.如果文字显示不开也必须强制一行内显示 */
  white-space: nowrap;
  /* 2.溢出的部分隐藏起来 */
  overflow: hidden;
  /* 3.溢出的时候用省略号代替 */
  text-overflow: ellipsis;
  /* 弹性伸缩盒子模型显示 */
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
```

```html
<div>
  老公程皮皮程皮皮吹泡泡臭屁屁lallalallalallal
</div>
```

```html
<video 
src="assets/mi11.mp4" 
autoplay="autoplay" 
muted="muted"
controls="controls"
loop="loop"
poster="assets/mi11.png"
>
</video>
```

http://music\.163\.com/song/media/outer/url?id=2534006.mp3

```css
/* 给placeholder修改颜色 */
input::placeholder {
  color: pink;
}
```

```html
<form action="">
<!-- required -->
<!-- autofocus自动获得焦点 -->
<input type="search" 
   required="required" 
   placeholder="今天吃什么" 
   autofocus="autofocus">
<input type="submit" value="提交">
</form>
```

+ 类选择器，伪类选择器，属性选择器权重都是10



+ `nth-child()`和`nth-of-type()`

```css
 /* nth-child 会把所有的盒子都排列序号 */
/* 执行的时候 首先看 :nth-child(1)* 之后回去看 前面div */
section div:nth-child(1) {
  background-color: pink;
}
```

```css
/* 执行的时候 首先看 前面指定的元素，比如这里div 之后回去看 :nth-child(1) */
section div:nth-of-type(1) {
  background-color: skyblue;
}
```

+ 新增伪元素选择器

```css
div {
  width: 200px;
  height: 200px;
  background-color: pink;
}
div::before {
  /* content是必须要写的 */
  /* 行内元素，不能直接设置大小 */
  display: inline-block;
  content: '我';
  width: 30px;
  height: 40px;
  background-color: purple;
}
div::after {
  content: '小猪佩奇'
}
```

```html
<!-- 伪元素选择器可以帮助我们利用css创建标签元素，而不需要HTML标签，
从而简化HTML 结构 -->
<div>
  是
</div>

```

```css
background-size
background-url
background-origin
```



## 四  CSS3新特性

### 4.1 transform

+ `translate`对于行内元素是无效的

### 4.2 动画的使用

+ 动画的基本使用

```css
<style>
    /* 我们想要页面一打开，一个盒子就从左边走到右边 */
    /* 1. 定义动画 */
    @keyframes move {
      0% {
        transform: translateX(0px)
      }
      100% {
        transform: translateX(1000px)
      }
    }
    div {
      width: 200px;
      height: 200px;
      background-color: pink;
      /* 2. 调用动画 */
      /* animation-name: 动画名称 */
      animation-name: move;
      /* 持续时间 */
      /* animation-duration: 时间 */
      animation-duration: 2s;
    }
</style>
```

+ `from`和`to`

```css
<style>
    @keyframes move {
      /* 这里from相当于0%,to等价于0%,和100% */
      from {
        transform: translate(0, 0);
      }
      to {
        transform: translate(1000px, 0);
      }
    }
    /* 动画序列 */
    div {
      width: 100px;
      height: 100px;
      background-color: pink;
      animation-name: move;
      animation-duration: 2s;
    }
</style>
```

+ 动画属性

```css
<style>
    /* @keyframes 规定动画*/
    @keyframes move {
      0% {
        transform: translate(0, 0)
      }
      100% {
        transform: translate(1000px, 0)
      }
    }
    div {
      width: 100px;
      height: 100px;
      background-color: pink;
      /* 动画名称 */
      animation-name: move;
      /* 动画时间 */
      animation-duration: 4s;
      /* 运动曲线 */
      animation-timing-function: ease;
      /* 何时开始，延迟 */
      animation-delay: 1s;
      /* 规定动画被播放的次数(重复次数)，默认是1 */
      /* animation-iteration-count: infinite; */
      /* 是否反方向播放  默认的是normal 如果想要反方向播放 设为alternate*/
      /* animation-direction: alternate; */
      /* animation-fill-mode默认是backwards，返回原来的位置， 我们可以让它停留在结束状态forwards*/
      animation-fill-mode: forwards;
    }
    div:hover {
      animation-play-state: paused;
    }
</style>
```

```css
/* 动画简写属性 */
/* 动画名称和持续时间不能省略 */
animation: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束的状态;
/* 顺序 */
animation: myfirst 5s linear 2s infinite alternate;
```





### 4.3 图片模糊处理

+ `filter:` 函数;  例如: `filter: blur(5px)`;    `blur`模糊处理,数值越大越模糊.
+ transition: 要过渡的属性 运动曲线 何时开始

1. 属性: 想要变化的 css 属性, 宽度高度 背景颜色 内外边距都可以,如果想要所有的属性都变化过渡,写一个all就可以
2. 花费时间: 单位是 s
3. 运动曲线: 默认是 ease
4. 何使开始:单位是 秒 可以设置延迟触发时间 默认是 0s
   + 过渡给谁加: 谁来过渡给谁加



### 4.4 3D转换

+ 3D移动

```css
div {
  width: 200px;
  height: 200px;
  background-color: pink;
  /* transform: translateX(100px); */
  /* 分开写下面的覆盖上面的 */
  /* transform: translateY(100px); */

  /* transform: translateX(100px) translateY(100px) translateZ(100px); */
  /* 同时应用多个可以这样写在一起 */
  /* 1. translateZ, 沿着Z轴移动 */
  /* 2. translateZ后面的单位我们一般跟px */
  /* 3. translateZ(100px)向外移动100px */
  /* 4. 3D移动有简写的方法 */
  /* transform: translate3d(x, y, z) */
  /* 5. x，y，z是不能省略的，如果没有就写0 */
  transform: translate3d(100px, 100px, 100px)
}
```



+ 透视`perspective`

1. 如果想要在网页产生3D效果需要透视（理解成3D物体投影在2D平面内）
2. 透视我们也称为视距：视距就是人的眼睛到屏幕的距离。
3. 距离视觉点越近的电脑平面成像越大，越远成像越小。
4. 透视的单位是像素

+ 透视写在被观察元素的父盒子上面

```css
<style>
    body {
      /* 透视写到被观察元素的父盒子上面 */
      /* perspective: 500px; */
      /* 近大远小 */
      perspective: 200px;
    }
    div {
      width: 200px;
      height: 200px;
      background-color: pink;
      /* transform: translateX(100px); */
      /* 分开写下面的覆盖上面的 */
      /* transform: translateY(100px); */

      /* transform: translateX(100px) translateY(100px) translateZ(100px); */
      /* 同时应用多个可以这样写在一起 */
      /* 1. translateZ, 沿着Z轴移动 */
      /* 2. translateZ后面的单位我们一般跟px */
      /* 3. translateZ(100px)向外移动100px */
      /* 4. 3D移动有简写的方法 */
      /* transform: translate3d(x, y, z) */
      /* 5. x，y，z是不能省略的，如果没有就写0 */
      transform: translate3d(400px, 100px, 100px);
    }
</style>

```

```html
<body>
  <div></div>
</body>

```
+ **注意**

  + `translateZ`物体距离屏幕的距离。z轴越大，我们看到的物体就越大

  + `perspective`眼睛距离屏幕的距离。

+ 3D旋转`rotate3d`
  + 3D旋转可以让元素在三维平面内沿着x轴，y轴，z轴或者自定义轴进行旋转。

**语法**

+ transform: rotateX(45deg):沿着x轴正方向旋转45度。
+ transform: rotateY(45deg):沿着y轴正方向旋转45度。
+ transform: rotateZ(45deg):沿着z轴正方向旋转45deg。
+ transform: rotate3d(x, y, z, deg): 沿着自定义轴旋转deg为角度(了解即可)

**补充**

***margin:0 auto；***在不同场景下生效条件如下：

+ **块级元素**：给定要居中的块级元素的宽度。

+ **行内元素**：①设置display:block；②给定要居中的行内元素的宽度。（行内元素设置成块级元素后可以对其宽高进行设置）

+ **行内块元素**：设置display:block。（如input、button、img等元素，自带宽度可以不用设置其宽度）

```css
/* 旋转的正值方向，左手法则 
   大拇指指向轴的正方向，四指弯曲的方向即为旋转角度为正值的方向
*/ 
<style>
    div {
      perspective: 300px;
    }
    img {
      display: block;
      width: 300px;
      height: 300px;
      /* 加了display:block;  margin: 100px auto;才生效 */
      margin: 100px auto;
      transition: all 2s;
    }
    img:hover {
      /* transform: rotateX(180deg); */
      /* 注意正值和负值 */
      /* 左手准则 */
      /* 1. 大拇指指向x轴正方向，四指弯曲的方向为旋转时为正值的方向。 */
      /* transform: rotateX(360deg); */
      /* transform: rotateY(360deg); */
      /* transform: rotateZ(360deg); */
    }
</style>
```

**rotate3d**

```css
<style>
    div {
      perspective: 300px;
    }
    img {
      display: block;
      margin: 200px auto;
      width: 300px;
      height: 300px;
      transition: all 2s;
      /* transition: transform 2s; */
    }
    img:hover {
      /* 这里的旋转轴为矢量 */
      transform: rotate3d(1, 1, 0, 45deg);
      /* transform: rotate3d(1, 1, 1, 45deg); */
    }
</style>
```

+ `transform-style`

1. 控制子元素是否开启三维立体环境。
2. tranform-style:preserve-3d;子元素开启立体空间。
3. 代码写给父级，但是影响的是子盒子。

```css
<style>
    .box {
      position: relative;
      width: 300px;
      height: 300px;
      margin: 100px auto;
      /* 让后面的紫色盒子保留立体空间 */
      transform-style: preserve-3d;
      transition: all 2s;
      font-size: 24px;
    }
    .box:hover {
      transform: rotateY(180deg);
    }
    .front, .back {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
    }
    .front {
      background-color: pink;
      z-index: 1;
    }
    .back {
      background-color: purple;
      transform: rotateY(180deg);
    }
</style>
```

```html
<div class="box">
    <div class="front">前 面</div>
    <div class="back">后 面</div>
</div>
```

**1. 浏览器私有前缀**：浏览器私有前缀是为了兼容老版本的写法，比较新版本的浏览器无需添加。

+ -moz-: 代表 firefox 浏览器私有属性
+ -ms-: 代表 ie 浏览器私有属性
+ -webkit-:代表safari、chrome私有属性
+ -o-:代表Opera私有属性

**2. 提倡的写法**

```css
-moz-border-radius: 10px;
-webkit-border-radius: 10px;
-o-border-radius: 10px;
border-radius: 10px;
```





## 五. 响应式

+ viewport浏览器的可视区域



+ 媒体查询

```css
// 关键词 媒体类型 逻辑操作符 媒体条件 媒体属性
// and就是screen还有min-width max-width同时存在的时候
@media screen and (min-width: 768px) and (max-width: 1024px) {
    .demo {
        color: red;
    }
}
```

+ landscape 表示横屏，portrait 表示竖屏

### 5.1 媒体查询MediaQuery

是css3的新语法

+ 概念: 为不同尺寸的屏幕设定不同的CSS样式



**@media常用属性**

>width,height: 浏览器可视宽度,高度
>
>device-width: 设备屏幕的宽度
>
>device-height: 设备屏幕的高度

### 5.2 响应式图片

```html
<img src="img/small.png"
     /* w是宽度描述符 */
     srcet="img/large.png 960w, img/medium.png 640w, img/small.png 320w"
     sizes="(max-width: 414px) 100vw, 640px"
     alt="响应式图片"/>
需要的图片大小（相当于响应式查询）
```

### 5.3 引入资源

+ 当样式比较繁多的时候，我们可以针对不同的媒体使用不同的stylesheets(样式表)
+ 原理，就是直接在link中判断设备的尺寸，然后引用不同的css文件。

```html
<style>
    /* 当我们屏幕大于640px以上的，我们让div一行显示2个 */
    /* 当我们屏幕小于640px 我们让div一行显示一个 */
    /* 一个建议：我们媒体查询1最好的方法是从小到大 */
    /* 引入资源就是针对不同的屏幕尺寸 调用不同的css文件 */
  </style>
  <link rel="stylesheet" href="css/style320.css" media="screen and (min-width: 320px)">
  <link rel="stylesheet" href="css/style640.css" media="screen and (min-width: 640px)">

<body>
  <div>1</div>
  <div>2</div>
</body>
```

+ style640.css

```css
* {
  margin: 0;
  padding: 0;
}
body {
  display: flex;
}
div {
  width: 50%;
  height: 100px;
}
div:nth-child(1) {
  background-color: pink;
}
div:nth-child(2) {
  background-color: purple;
}
```

+ style320.css

```css
* {
  margin: 0;
  padding: 0;
}
div {
  width: 100%;
  height: 100px;
}

div:nth-child(1) {
  background-color: pink;
}

div:nth-child(2) {
  background-color: purple;
}
```

### 5.4响应式布局容器

响应式需要一个父级作为布局容器，来配合子元素来实现变化效果。

原理就是在不同屏幕下，通过媒体查询来改变这个布局容器的大小，再改变里面子元素的排列方式和大小，从而实现不同屏幕下看到不同的页面布局和样式变化。

**平时我们的响应式尺寸划分**

+ 超小屏幕（手机，小于768px）：设置宽度为100%
+ 小屏幕（平板，大于等于768px）：设置宽度为750pxpx
+ 中等屏幕（桌面显示其，大于等于992px）：宽度设置为970px
+ 大屏幕（大桌面显示器，大于等于1200px）：宽度设置为1170px





## 六 移动端

### 1. 移动端基础

**总结**： 

+ 兼容移动端主流浏览器，处理`Webkit`内核浏览器即可,移动端浏览器我们主要对webkit内核进行兼容。
+ 我们现在开发的移动端主要针对手机端开发。
+ 现在移动端碎化比较严重，分辨率和屏幕尺寸大小不一。
+ 学会用谷歌浏览器模拟手机界面以及调试。

### 2. 视口

**视口(viewport)**就是浏览器显示页面内容的屏幕区域。视口可以分为布局视口、视觉视口和理想视口。

+ 布局视口
+ 视觉视口
+ 理想视口
  + 需要手动添加meta标签通知浏览器操作。
  + meta视口标签的主要目的：布局视口的宽度应该与理想视口的宽度一致，简单理解就是设备多宽，我们布局的视口就多宽。

```html
<meta name="viewport" content="width=device-width, user-scalable=no, intitial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

+ `content="width=device-width`宽度为设备的宽度。
+ `user-scalable`不希望用手指捏大,用户是否可以缩放，yes或no(1或0)。
+ ` maximum-scale`最大缩放比，大于0的数字。
+ `minimum-scale`最小缩放比，大于0的数字。
+ `initial-scale`初始缩放比，大于0的数字。

**注意**：标准的viewport设置

+ 视口宽度和设备保持一致。
+ 视口的默认缩放比例为1.0。
+ 不允许用户自行缩放。
+ 最大允许的缩放比例1.0。
+ 最小允许的缩放比例1.0。

### 3. 二倍图

#### 3.1 物理像素&物理像素比

+ 物理像素点指的是屏幕显示的最小颗粒，是物理真实存在的，屏幕分辨率，比如iphone6/7/8的是750*1334(横向750个像素点竖向1334个像素点)
+ 我们开发时候的1px不是一定等于1个物理像素的。
+ pc端页面，1个px等于1个物理像素的，但是移动端就不尽相同。（在iphone8里面，1px对应两个物理像素点。）
+ 一个px的能显示的物理像素点的个数，称为物理像素比或屏幕像素比。
  + iphone里面物理像素比为2.

1.  物理像素 就是我们说的分辨率 iPhone的物理像素是 750*1334 

2. 在iPhone里面 1px开发像素 = 2个物理像素 



+ PC端和早前的手机屏幕/普通手机屏幕：1CSS像素 = 1 物理像素的
+ Retina（视网膜屏幕）是一种显示技术，可以把更多的物理像素点压缩至一块屏幕里，从而达到更高的分辨率，并提高屏幕显示的细腻程度。

#### 3.2 多倍图

```css
<style>
    /* 我们需要一个50*50像素（css像素）的图片 直接放到我们iphone8里面会放大2倍（宽高各2倍）， 100*100， 就会模糊 */
    /* 我们采取的是 放一个100*100图片，然后手动把这个图片缩小为50*50（css像素） */
    /* 我们准备的图片 比我们实际需要的大小大2倍，这种方式就是2倍图 */
    img:nth-child(2) {
      width: 50px;
      height: 50px;
    }
 </style>
```

```html
<!-- 模糊的 -->
<img src="images/apple50.jpg" alt="">
<!-- 我们采取二倍图 -->
<img src="images/apple100.jpg" alt="">
```



#### 3.3 背景缩放

+ `backgroound-size`属性规定背景图像的尺寸。

```css
background-size: 背景图片宽度 背景图片高度;
```



+ 单位：长度|百分比|cover|contain；
+ cover把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。
+ contain把图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。

```css
.div1 {
  width: 500px;
  height: 500px;
  margin: 100px auto;
  border: 2px solid red;
  background: url(../image/唯.jpg) no-repeat;
  /* background-size: 图片的宽度 图片的高度; */
  /* background-size: 500px 400px; */
  /* 1.只写一个，肯定是宽度 高度省略了 会等比缩放 */
  /* background-size: 500px; */

  /* 2. 里面的单位课可以跟%，相对于父盒子来说的 */
  /* background-size: 50%; */

  /* 3. cover */
  /* background-size: cover; */

  /* 4. contain 高度和宽度等比例拉伸 当宽度 或者高度 铺满div盒子就不再进行拉伸了*/
  /* background-size: contain; */
}
```

```html
<div class="div1"></div>
```

### 4. 移动端开发选择

#### 4.1 移动端主流方案

##### 	1. 单独制作==移动端==页面(主流)

通常情况下，网址域名前面加m（mobile）可以打开移动端。通过判断设备，如果移动设备打开，则跳到移动端页面。

+ 京东手机版

+ 淘宝触屏版

+ 苏宁易购手机版

  ##### 2. ==响应式==页面兼容移动端（其次）

+ 三星手机官网

通过判断屏幕宽度来改变样式，以适应不同终端。

缺点:制作麻烦，需要花费很大精力去调兼容性问题。

### 5. 移动端技术解决方案

#### 5.2 CSS初始化 `normalize.css`

[](https://necolas.github.io/normalize.css/)

#### 5.3 CSS3盒子模型 box-sizing

+ 传统模式宽度计算：盒子的宽度 = CSS中设置的width+border+padding
+ CSS3盒子模型：盒子的宽度width里面包含了border和padding，也就是说，我们CSS3中的盒子模型，padding和border不会撑大盒子了。

```css
<style>
    div:nth-child(1) {
      width: 200px;
      height: 200px;
      background-color: pink;
      padding: 50px;
      border: 10px solid purple;
    }
    div:nth-child(2) {
      width: 200px;
      height: 200px;
      background-color: skyblue;
      box-sizing: border-box;
      padding: 50px;
      border: 10px solid green;
    }
</style>
```

```html
<div></div>
<div></div>
```

```css
/*CSS3盒子模型*/
box-sizing: border-box;
/*传统盒子模型*/
box-sizing: content-box;
```

+ 传统orCSS3盒子模型
  + 移动端可以全部CSS3盒子模型。
  + PC端如果完全需要兼容，我们就用传统模式，如果不考虑兼容性，我们就选择CSS3盒子模型。

#### 5.4 特殊样式



```css
<style>
    a {
      /* 点击高亮我们需要清除 设置为transparent完全透明 */
      /* 不加这个移动端点击会有高亮 */
      -webkit-tap-highlight-color: transparent;
    }
    input {
      /* 在移动端浏览器默认的外观在ios上加上这个属性才能给按钮和输入框自定义样式 */
      -webkit-appearance: none;
    }
    /* 禁用长按页面时的弹出菜单 */
    img, a {
      -webkit-touch-callout: none;
    }
</style>
```

```html
<a href="#">lg</a>
<input type="button" value="按钮">
```

### 6. 移动端布局

#### 6.1 流式布局(百分比布局)

+ 流式布局，就是百分比布局，也称非固定像素布局。
+ 通过盒子的宽度设置成百分比来根据屏幕的宽度来进行伸缩，不受固定像素的限制，内容向两侧填充。
+ 流式布局方式是移动端web开发使用的比较常见的布局方式。

### 7. 案例：京东移动端首页

#### 7.1 二倍精灵图做法

+ 在firework里面把精灵图等比缩放为原来的一半。
+ 之后根据大小测量坐标。
+ 注意代码里面的background-size也要写：精灵图原来宽度的一半。

### 8. flex弹性布局

+ `align-items` 设置侧轴上的子元素排列方式（单行）,在子项为单行的时候使用

+ `align-content` 设置侧轴上的子元素的排列方式（多行）

```css
div {
  display: flex;
  width: 800px;
  height: 400px;
  background-color: pink;
  /* 因为有了换行，此时我们侧轴上控制子元素的对齐方式我们用 align-content*/
  flex-wrap: wrap;
  /* align-content: flex-start; */
  /* align-content: center; */
  /* align-content: flex-end; */
  /* align-content: space-between; */
  align-content: space-around;
  /* flex-direction: column;
  flex-wrap: wrap; */
  /* 把设置主轴方向和是否换行（换列）简写 */
  flex-flow: column wrap;
}
div span {
  width: 150px;
  height: 100px;
  background-color: purple;
  margin: 0 10px;
}
```

```html
<div>
    <span>1</span>
    <span>2</span>
    <span>3</span>
    <span>4</span>
    <span>5</span>
    <span>6</span>
</div>
```

### 9. rem适配方案

#### 9.1 rem实际开发适配方案

+ 按照设计稿与设备宽度的比例，动态计算并设置html标签的font-size大小；（媒体查询）
+ CSS中，设计稿元素的宽、高、相对位置等取值，按照同等比例换算为单位为rem的值

**技术方案1**

+ less
+ 媒体查询
+ rem

**技术方案2(推荐)**

+ flexble.js
  + 手机淘宝团队出的简洁高效 移动端适配库
  + 他的原理是把当前设备划分为10等份，但是在不同设备下，比例还是一致的。
  + 我们要做的就是确定好当前设备的html文字大小就可以了。

+ rem

**在index.less中导入common.less文件**

```
@import "common"
```



## 七 SASS

1. 嵌套



2. 变量

`$black=#000;`

3. mixin

```sass
@mixin box-shadow($shadow...) {
	-moz-box-shadow: $shadow;
	-webkit-box-shadow: $shadow;
	box-shadow: $shadow;
}
.box {
	@include box-shadow(2px 4px #ccc);
}

.header {
	@include box-shadow(3px 4px #ccc))
}
```



4. 循环

```sass
@for $i from 1 to 4 {
	.item-#{$i} {
	background-image: url(xxx/#{$i}.png)
	}
}

/* 对应css **/

```

## 八 Less基础

#### 8.1

**Less使用**

我们首先新建一个后缀名为less的文件，在这个less文件里面书写less语句。

**Less变量**

```
@变量名: 值;
```

1. 变量命名规范

+ 必须有@为前缀。
+ 不能包含特殊字符。
+ 不能以数字开头。
+ 大小写敏感。

**Less编译**

​	**vcode Less插件**：easy less

+ 可以把less编译为css

**Less嵌套**

**css**

```css
#header .logo {
    width: 300px;
}

a:hover {
    color: pink;
}

a::before {
    content: ''';
}
```

**Less嵌套写法**

```less
#header {
    .logo {
        width: 300px;
    }
}

a {
    &:hover {
        pink;
    }
}

a {
    &::before {
        content: '';
    } 
}
```



**Less运算**

Less提供了加(+)、减(-)、乘(*)、除(/)算术运算。

1. 我们运算符的左右两侧必须敲一个空格隔开

2. 两个数参与运算 如果只有一个数有单位，则最后的结果就以这个单位为准

3. 两个数参与运算，如果两个数都有单位，而且不一样的单位 以第一个单位为准

4. 除法要加括号

```less
@baseFont: 50px;
@border: 5px + 5;
html {
  font-size: @baseFont;
}
div {
  width: 200px - 50;
  // height: 200px * 2;
  height: (150px + 50)*2;
  border: @border solid pink;
  // background-color: skyblue;
  background-color: #666 - #444;
}
img {
  width: (82rem / @baseFont);
  height: (82rem / @baseFont);
}
```

**css**

```css
html {
  font-size: 50px;
}
div {
  width: 150px;
  height: 400px;
  border: 10px solid pink;
  background-color: #222222;
}
img {
  width: 1.64rem;
  height: 1.64rem;
}

```

