## antd使用

### 1. Affix固钉



### 2. AutoComplete自动完成



### 3. Cascader级联选择

+ 省市联动

### 4. Steps



### 5. DatePicker



### 6. Mentions提及

+ 用于在输入中提及某人或某事，常用于发布、聊天或评论功能。



### 7. Rate评分组件



### 8. Slider滑动输入条



### 9. Switch开关



### 10. TimePicker



### 11. TreeSelect树选择

+ 类似 Select 的选择控件，可选择的数据结构是一个树形结构时，可以使用 TreeSelect，例如公司层级、学科系统、分类目录等等。

### 12. Upload



### 13. Badge徽标数

+ 图标右上角的圆形徽标数字





### 14. Calendar日历



### 15. Carousel走马灯



### 16. Collapse折叠面板



### 17. Comment评论



### 18. Descriptions描述列表

+ 成组展示多个只读字段。





### 19. Empty空状态

+ 空状态时的展示占位图





### 20. Popover气泡卡片

+ 点击/鼠标移入元素，弹出气泡式的卡片浮层。





### 21. Statistic统计数值





### 22. Timeline时间轴

+ 垂直展示的时间流信息



### 23. Tooltip文字提示

+ 简单的文字提示气泡框。



### 24. Tree树形控件

+ 多层次的结构列表。





### 25. Drawer抽屉





### 26. Notification通知提醒框





### 27. Popconfirm气泡确认框

+ 点击元素，弹出气泡式的确认框。



### 28. Alert警告提示



### 29. Progress进度条

+ 展示操作的当前进度





### 30. Result 结果



### 31. Skeleton骨架屏

+ 在需要等待加载内容的位置提供一个占位图形组合。



### 32. Spin加载中

+ 用于页面和区块的加载中状态。



### 33. Anchor锚点

+ 用于跳转到页面指定位置。



### 34. BackTop回到顶部

+ 返回页面顶部的操作按钮。





### 35. ConfigProvider全局化配置

+ 为组件提供统一的全局化配置





## and-design-pro

### 权限管理

#### 页面内的权限控制

**在项目中经常有的场景是不同的用户的权限不同**

1. 不同的用户在页面中可以看到的元素和操作不同
2. 不同的用户对页面的访问权限不同

**首先新建 `src/access.ts` ，在该文件中 `export default` 一个函数，定义用户拥有的权限**

+ 该文件需要返回一个 function，返回的 function 会在应用初始化阶段被执行，执行后返回的对象将会被作为用户所有权限的定义。对象的每个 key 对应一个 boolean 值，只有 true 和 false，代表用户是否有该权限。

```tsx
const super_admin = 'admin';
const all_permission = '*:*:*';

/**
 * @see https://umijs.org/zh-CN/plugins/plugin-access
 * */
export default function access(initialState: { permissions?: string[]; roles?: string[] }) {
  const { permissions = [], roles = [] } = initialState || {};

  return {
    checkPerms: (value: string[] = []) => {
      if (value && value instanceof Array && value.length > 0) {
        const permissionDatas = value;

        const hasPermission = permissions.some(
          (permission: string) =>
            all_permission === permission || permissionDatas.includes(permission),
        );
        return !!hasPermission;
      }
      console.error(`need perms! Like perms="['system:user:add','system:user:edit']"`);
      return false;
    },
    checkRoles: (value: string[] = []) => {
      if (value && value instanceof Array && value.length > 0) {
        const permissionRoles = value;

        const hasRole = roles.some(
          (role: string) => super_admin === role || permissionRoles.includes(role),
        );
        return !!hasRole;
      }
      console.error(`need roles! Like roles="['admin','editor']"`);
      return false;
    },
  };
}

```

#### 路由和菜单的权限控制

+ 一种轻量级的全局数据共享的方案

+ useModel 可以接受一个可选的第二个参数，可以用于性能优化。当组件只需要消费 model 中的部分参数，而对其他参数的变化并不关心时，可以传入一个函数用于过滤。函数的返回值将取代 model 的返回值，成为 useModel 的最终返回值。
