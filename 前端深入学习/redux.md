## store

## 重点代码

### 递归处理数据

```ts
function* flattenTree(tree: ITree): any {
  yield tree;
  if (tree.children) {
    for (const item of tree.children) {
      yield* flattenTree(item);
    }
  }
}

function flattenTree2(tree: ITree, result: ITree[] = []) {
  result.push(tree);
  if (tree.children) {
    for (const item of tree.children) {
      flattenTree2(item, result);
    }
  }
  return result;
}

function flattenTree3(tree: ITree) {
  let result: ITree[] = [];
  result.push(tree);
  if (tree.children) {
    for (const item of tree.children) {
      result.push(...flattenTree3(item));
    }
  }
  return result;
}
```

### index.ts

```ts
import { createStore, combineReducers } from "redux";
import { userReducer, treeReducer } from "./reducers";

const reducer = combineReducers({ user: userReducer, tree: treeReducer });

const store = createStore(reducer);
export default store;
```

### constant

```ts
export const EDIT = "edit";
export const ADD_HOBBY = "addHobby";

export const EDIT_MONEY = "editMoney";
```

### reducers

### index.ts

```ts
export { userReducer } from "./user";
export * from "./tree";
```

### user.ts

```ts
import { EDIT, ADD_HOBBY } from "../constant";
const defaultUser = {
  name: "cpp",
  age: 2,
  sex: "male",
  hobbies: ["code", "game"],
};

interface EditAction {
  type: typeof EDIT;
  payload: Partial<{
    name: string;
    age: number;
    sex: string;
  }>;
}

interface AddHobbyAction {
  type: typeof ADD_HOBBY;
  payload: string[];
}

export const userReducer = (
  state = defaultUser,
  action: EditAction | AddHobbyAction
) => {
  switch (action.type) {
    case EDIT:
      return { ...state, ...action.payload };
    case ADD_HOBBY:
      return { ...state, hobbies: [...state.hobbies, ...action.payload] };
    default:
      return state;
  }
};
```

### tree.ts

```ts
import { EDIT_MONEY } from "../constant";
export interface ITree {
  id: number;
  name: string;
  props: {
    money: number;
  };
  children?: Array<ITree>;
}

const defaultState: ITree = {
  name: "boss",
  id: 1,
  props: {
    money: 100000,
  },
  children: [
    {
      name: "leader1",
      id: 2,
      props: {
        money: 30000,
      },
      children: [
        {
          name: "wqj",
          id: 4,
          props: {
            money: 8000,
          },
        },
      ],
    },
    {
      name: "leader2",
      id: 3,
      props: {
        money: 20000,
      },
      children: [],
    },
  ],
};
export const treeReducer = (state = defaultState, action: any) => {
  switch (action.type) {
    case EDIT_MONEY:
      return state;
    default:
      return state;
  }
};
```

## 根组件

```tsx
import ReactDOM from "react-dom";
import App from "./App";
import { BrowserRouter } from "react-router-dom";
import { Provider } from "react-redux";
import "@ant-design/pro-table/dist/table.css";
import "@ant-design/pro-layout/dist/layout.css";
import "@ant-design/pro-form/dist/form.css";
import "antd/dist/antd.css";
import store from "./store";

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </Provider>,

  document.getElementById("root")
);
```

## APP.tsx 中使用 redux

```tsx
import { useEffect, useState } from "react";
import { useSelector, useStore, useDispatch } from "react-redux";
import { EDIT, ADD_HOBBY } from "./store/constant";
import { ITree } from "./store/reducers";

function* flattenTree(tree: ITree): any {
  yield tree;
  if (tree.children) {
    for (const item of tree.children) {
      yield* flattenTree(item);
    }
  }
}

function flattenTree2(tree: ITree, result: ITree[] = []) {
  result.push(tree);
  if (tree.children) {
    for (const item of tree.children) {
      flattenTree2(item, result);
    }
  }
  return result;
}

function flattenTree3(tree: ITree) {
  let result: ITree[] = [];
  result.push(tree);
  if (tree.children) {
    for (const item of tree.children) {
      result.push(...flattenTree3(item));
    }
  }
  return result;
}

const App = () => {
  const dispatch = useDispatch();
  const user = useSelector((state: any) => state.user);
  const arr = useSelector((state: any) => Array.from(flattenTree(state.tree)));

  const changeUser = () => {
    dispatch({ type: EDIT, payload: { name: "www" } });
  };

  const addHobby = () => {
    dispatch({ type: ADD_HOBBY, payload: ["study", "music"] });
  };

  return (
    <div className="App">
      <div>
        <h2>用户基本信息</h2>
        <div>
          <div>姓名: {user.name}</div>
          <div>年龄: {user.age}</div>
          <div>性别: {user.sex}</div>
          {user.hobbies.map((item: string) => (
            <div key={item}>{item}</div>
          ))}
        </div>
        <button onClick={changeUser}>修改用户基本信息</button>
        <button onClick={addHobby}>添加爱好</button>
      </div>
      <div>
        <h2>信息展示</h2>
        {arr.map((item: any, index: number) => (
          <div key={index}>{item.name}</div>
        ))}
      </div>
    </div>
  );
};

export default App;
```
