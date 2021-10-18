# ProComponents学习

## ProTable

### 1. 自带图标密度， 全屏，设置， 刷新

通过options设置图标是否显示， 比如通过在ProTable中添加options属性

```tsx
options = {{
  show: true,
  density: false,	// 这样子密度图标就不会显示
  fullScreen: true,
  setting: true,
}}
```



### 密度

密度在设置了默认size的情况下点击就没有反应

**解决方法：**

```tsx
import { SizeType } from 'antd/lib/config-provider/SizeContext';

const [size, setSize] = useState<SizeType>('small');

// 在ProTable中添加属性
size={size}
onSizeChange={(size) => setSize(size)}
```



### 2. request

通过reques发起请求获取数据,获取 `dataSource` 的方法

```tsx
request={async (params, sorter, filter) => {
  return handleTableRequest(
    params,
    sorter,
    filter,
    queryCorporateCustomer
  );
}}
```



排序功能实现

```tsx
const { reservationDate = 'descend'} = sorter 
const { data, total } = await handleTableRequest(params, {}, filter, getManyOrderInfo);

data?.sort((a: any, b: any) => {
  const prevTime = new Date(a.reservationDate).getTime()
  const nextTime = new Date(b.reservationDate).getTime()
  return reservationDate === 'ascend' ?  prevTime - nextTime : nextTime - prevTime;
}) 
return {
  data,
  total,
} as any
```



#### params

用于 `request` 查询的额外参数，一旦变化会触发重新加载



### 3. postData

对通过 `request` 获取的数据进行处理

```tsx
postData={(dataSource) => {
  return dataSource.map(item => {
    return item.children.length ? item.children.map((item2: any) => ({...item2, isChildOrder: true})) : {...item}
  })
}}

// return dataSource.map(item => {
//   return item.children.length ? item.children.map((item2: any) => ({...item2, isChildOrder: true})) : {...item}
// })
```



### 4. columns列定义

#### 常用属性

##### title： 

##### dataIndex



##### valueType

值的类型,会生成不同的渲染器

```tsx
// 比如常见的
valueType: 'date' // 以日期的形式展示
valueType: 'time' // 以时间的形式展示
valueType: 'dateTime' // 以日期时间的形式展示
valueType: 'dateRange'	// 日期区间
valueType: 'dateTimeRange'	// 日期时间区间
valueType: 'timeRange'	// 时间区间
```



##### valueEnum

值的枚举，会自动转化把值当成 key 来取出要显示的内容

##### render



##### search

配置列的搜索相关，false为隐藏



##### hideInTable

### 5. formRef

可以获取到查询表单的 form 实例，用于一些灵活的配置

```tsx
const formRef = useRef<FormInstance>();
```



### 6. actionRef

Table action 的引用，便于自定义触发

## Modal/Drawer

### 属性

#### visible

设置visible属性，当我们打开Modal的时候就将visible设置为true，关闭Modal的时候就将visible设置为false

### onFinish

点击确定按钮后的操作

#### onVisibleChange

ModalForm的onVisibleChange可以得到visible的值，然后控制visible为true和false

### 新增Modal

新增表单的话就要清空表格，不然就会保存上一次填充过的记录。

```tsx
const formRef = useRef<FormInstance>();
```

1. 输入数据后点击确定后触发`onFinish = handleFinish(values)`
2. values为表单中填入的数据
3. 点开编辑的时候需要填充表单：`formRef.current?.setFieldsValue`
4. 新增的时候要清空表单内容：`formRef.current?.resetFields();`

### 编辑Modal

通过record判断是编辑还是新增，如果是编辑就通过得到的这行数据，即`record`填充表格



## ProForm



