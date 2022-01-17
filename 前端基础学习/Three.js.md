

## 起步

### 入门例子

```ts
  // 使用面向对象来构建程序，scene场景，amera相机，renderer渲染器
const scene = new THREE.Scene()
// 第一个属性filed of view视角  
// 第二个属性是aspect ratio 长宽比
// 第三个属性近裁剪面
// 第四个属性远裁剪面
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
// 渲染器
const renderer = new THREE.WebGLRenderer()
// 设置渲染空间的尺寸, 般就使用目标屏幕的宽高（window.innerWidth和window.innerHeight），也可以给定一个小尺寸。
renderer.setSize(window.innerWidth, window.innerHeight)
// 最后，把renderer 元素添加到HTML文档中。这里是一个 <canvas> 元素，渲染器用来显示场景。
document.body.appendChild( renderer.domElement );

// 创建3D立方体
// 为了创建一个立方体，我们需要使用盒子模型BoxGeometry，这是一个包含立方体所有顶点和填充面的对象
const geometry = new THREE.BoxGeometry(1, 1, 1)  
// 除了这个几何模型外(geometry)，我们还需要一个材料来对其着色
const material = new THREE.MeshBasicMaterial({color: 'pink'})
const cube = new THREE.Mesh(geometry, material)
scene.add(cube)

// 当我们调用 scene.add() 时，对象将被添加到原点处，即坐标点(0,0,0)，这将导致相机和立方体发生空间重叠。为了避免这样，我们把相机（camera）的位置移出来一些。
// // 我们需要一个 渲染循环（render loop）。
camera.position.z = 5;

const render = () => {
requestAnimationFrame( render );

  cube.rotation.x += 0.1;
  cube.rotation.y += 0.1;

    renderer.render( scene, camera );
  };
  render();
```

### 矩阵变换

+  Three.js使用materices来编码3D变换——位置平移，旋转，和缩放。
+ 每个3D对象有一个matrix用来保存该对象的位置，旋转和缩放因子。

#### 快捷属性 和 matrixAutoUpdate

有两个方法来完成对象的矩阵变换：

1. 修改对象的位置(position)，四元数，和缩放属性，然后交由three.js来根据这些属性重新计算对象矩阵。

```js
object.position.copy(start_position)
object.question.copy(question)
```

缺省情况下，matrixAutoUpdate 属性被设置为 true，这样矩阵将会被自动重新计算,如果对象是静止的，或者你想手动控制计算过程来获得更好的性能，你可以设置该属性为false：

```js
object.matrixAutoUpdate = false;
```

在修改对象属性之后，手动更新矩阵

```js
object.updateMtrix()
```

2. 直接修改对象的属矩阵，Matrix4类有多个方法用来修改矩阵

```js
object.matrix.setRotationFromQuaternion(quaternion)
object.matrix.setPosition(start_position)
object.matrixAutoUpdate = false
```

**注意**：在这种情况下，matrixAutoUpdate *必须* 被设置为 false，并且你应该要确保 *不去* 调用 updateMatrix。 否则手动修改会被自动计算所覆盖。

#### 对象和世界矩阵

对象的



#### 旋转和四元数

Three.js提供了两种途径来标识3D旋转

四元数，一般表示为(x,y,z,w)，是一个4维的向量，前3个表征旋转轴向，第4个数值用来表征旋转角度，也就是你把对象围绕指定轴旋转一个指定角度。我们可以把四元数理解成一个复数，其实数部分就是w，虚数部分为(x,y,z)，这样就可以通过复平面向量旋转来理解这个概念。更多说明请阅读[四元数章节](https://www.techbrood.com/threejs/docs/#参考手册/数学工具库(Math)/四元数(Quaternion))。

### 如何创建VR内容

1. `WEBVR.createButton()`做了两件事，它创建了一个指示虚拟现实兼容性的按钮，

```js
WEBVR.createButton()
```

2. 此外，如果用户激活按钮，它将启动一个虚拟现实会话。要做的就是将下面的代码行添加到你的应用程序中。

```js
document.body.appendChild( WEBVR.createButton( renderer ) );
```

3. 接下来，告诉渲染器 WebGLRenderer 实例来启用VR渲染。

```js
renderer.vr.enabled = true
```

4. 由于没法使用众所周知的`window.requestAnimationFrame() `函数，得调整一下动画循环。对于vr项目，使用 [setAnimationLoop](javascript:window.parent.goTo('WebGLRenderer.setAnimationLoop')). 最少代码如下所示:

```js
renderer.setAnimationLoop(function() {
  renderer.render(scene, camera)
})
```

### 自定义混合方程

#### 自定义混合方程常量

+ 所谓混合就是在绘制时，不是直接把新的颜色覆盖在原来旧的颜色上，而是将新的颜色与旧的颜色经过一定的运算，获得最终的混合颜色。
  其中新的颜色被称为源颜色，旧的颜色称为目标颜色。传统意义上的混合，是将源颜色乘以源因子，目标颜色乘以目标因子，然后相加。
  源因子和目标因子设置的不同直接导致混合结果的不同。

+ 下面用数学公式来表达一下这个运算方式。假设源颜色的四个分量（指红色，绿色，蓝色，alpha值）是(Rs, Gs, Bs, As)，目标颜色的四个分量是(Rd, Gd, Bd, Ad)，
  又设源因子为(Sr, Sg, Sb, Sa)，目标因子为(Dr, Dg, Db, Da)。则混合产生的新颜色可以表示为：

```js
(RsSr+RdDr, GsSg+GdDg, BsSb+BdDb, AsSa+AdDa)
```

**如果颜色的某一分量超过了1.0，则它会被自动截取为1.0，不应出现越界。**

- Cf表示混合后显示的颜色
- Cd混合前颜色缓冲中已经有的颜色值
- Cs将要绘制的颜色
- S为glBlendFunc函数设置时的第一个参数,源颜色因子
- D为glBlendFunc函数设置时的第二个参数,目标颜色因子

#### 方程



#### 目标因子



#### 源因子



+ 源因子和目标因子是可以通过`glBlendFunc`函数来进行设置的。`glBlendFunc`有两个参数，前者表示源因子，后者表示目标因子。

```js
// 如果设置了下面这样，则表示完全使用源颜色，完全不使用目标颜色，因此画面效果和不使用混合的时候一致。
glBlendFunc(GL_ONE, GL_ZERO);

// 如果设置了下面这样，则表示源颜色乘以自身的alpha 值，目标颜色乘以1.0减去源颜色的alpha值，
// 源颜色的alpha值越大，则产生的新颜色中源颜色所占比例就越大，而目标颜色所占比例则减小。
// 这种情况下，我们可以简单的将源颜色的alpha值理解为“不透明度”。这也是混合时最常用的方式。
glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
```



### GL状态常量

这里定义的常量用来指定多边形正反面是否可以被剔除。背面一般是不可见的，进行剔除后，可以提高渲染效率。

#### 剔除面(Cull Face)

THREE.CullFaceNone: 多边形面不剔除。

THREE..CullFaceBack: 多边形的反面进行剔除。

THREE.CullFaceFront：多边形的正面进行剔除。

THREE.CullFaceFrontBack：多边形的正面和反面都进行剔除。

#### 正面方向(Front Face Direction)

THREE.FrontFaceDirectionCW：定义多边形正面的方向：顺时针为0（CW=Clockwise）。
THREE.FrontFaceDirectionCCW：定义多边形正面的方向：逆时针为1（CCW=Count Clockwise）

### 材料常量(Material Constants)

#### 面(side)

+ 这些常量用来定义几何体哪些面需要应用材料。注意3D空间一个表面有内外两边。
+ 默认情况下是THREE.FrontSide（前外面）

THREE.FrontSide：前面
THREE.BackSide：后面
THREE.DoubleSide：双面（即内外两面都应用该材料）。

#### 着色方式(Shading）

+ 着色是给骨架（线框轮廓）添加皮肤（色彩）的过程。

**THREE.FlatShading**：

1. 平面着色，也称之为“恒量着色”.

2. 平面着色是最简单也是最快速的着色方法，每个多边形都会被指定一个单一且没有变化的颜色。

3. 这种方法虽然会产生出不真实的效果，不过它非常适用于快速成像及其它速度要求高于细致度的场合，如：生成预览动画。

**THREE.SmoothShading**：

1. 平滑着色，用多种颜色进行绘制，每个顶点都是单独进行处理的，各顶点和各图元之间采用均匀插值。

#### 颜色(Colors)

+ 在将Geometry对象转换为BufferGeometry对象时，用来设置转换后的对象顶点从哪里继承颜色。

THREE.NoColors：顶点没有颜色
THREE.FaceColors：顶点使用面的颜色
THREE.VertexColors：顶点使用顶点的颜色。

#### 混合模式(Blending Mode)

+ 这里定义材料混合模式常量

THREE.NoBlending：无混合
THREE.NormalBlending：普通混合
THREE.AdditiveBlending：加法混合
THREE.SubtractiveBlending：减法混合
THREE.MultiplyBlending：乘法混合
THREE.CustomBlending：自定义混合。参见[自定义混合方程(CustomBlendingEquation)](javascript:window.parent.goTo('自定义混合方程(CustomBlendingEquation)'))。



活动奖品列表：0.5

新增奖品: 1