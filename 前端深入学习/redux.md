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

```ts
function editMoneyById(
  state: ITree,
  payload: {
    id: number;
    props: Partial<ITree["props"]>;
  }
): ITree {
  if (state.id === payload.id) {
    return { ...state, props: { ...state.props, ...payload.props } };
  } else {
    if (state.children) {
      for (let item of state.children) {
        const newItem = editMoneyById(item, payload);
        if (newItem !== item) {
          return {
            ...state,
            children: state.children.map((i) => (i === item ? newItem : i)),
          };
        }
      }
      return state;
    }
  }
  return state;
}

function replaceNode(node: ITree, map: (node: ITree) => ITree): ITree {
  const newNode = map(node);
  if (newNode !== node) return newNode;
  else {
    if (node.children) {
      for (const item of node.children) {
        const newItem = replaceNode(item, map);
        if (newItem !== item) {
          return {
            ...node,
            children: node.children?.map((i) =>
              i.id === item.id ? newItem : i
            ),
          };
        }
      }
      return node;
    }
  }
  return node;
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

### node.ts

```ts
import { EDIT_NODE, ADD_NODE, DELETE_NODE } from "../constant";

export interface INode {
  id: number;
  name: string;
  props: {
    money: number;
    car: number;
  };
  children?: Array<INode>;
  root?: boolean;
}
interface EditNodeAction {
  type: typeof EDIT_NODE;
  payload: {
    id: number;
    props: Partial<INode["props"]>;
  };
}
interface AddNodeAction {
  type: typeof ADD_NODE;
  payload: {
    id: number;
    data: any;
  };
}
interface DeleteNodeAction {
  type: typeof DELETE_NODE;
  payload: {
    id: number;
  };
}

type NodeAction = EditNodeAction | AddNodeAction | DeleteNodeAction;

export const createEditNodeAction: (
  payload: EditNodeAction["payload"]
) => EditNodeAction = (payload) => ({
  type: EDIT_NODE,
  payload,
});
export const createAddNodeAction: (
  payload: AddNodeAction["payload"]
) => AddNodeAction = (payload) => ({
  type: ADD_NODE,
  payload,
});
export const createDeleteNodeAction: (
  payload: DeleteNodeAction["payload"]
) => DeleteNodeAction = (payload) => ({
  type: DELETE_NODE,
  payload,
});

const defaultState: INode = {
  root: true,
  name: "boss",
  id: 1,
  props: {
    money: 100000,
    car: 5,
  },
  children: [
    {
      name: "leader1",
      id: 2,
      props: {
        money: 30000,
        car: 2,
      },
      children: [
        {
          name: "cpp",
          id: 4,
          props: {
            money: 10000,
            car: 0,
          },
          children: [
            {
              name: "wqj",
              id: 5,
              props: {
                money: 8000,
                car: 0,
              },
            },
          ],
        },
      ],
    },
    {
      name: "leader2",
      id: 3,
      props: {
        money: 20000,
        car: 2,
      },
      children: [],
    },
  ],
};

function replaceNode(node: INode, map: (node: INode) => INode): INode {
  const newNode = map(node);
  if (newNode !== node) return newNode;
  else {
    if (node.children) {
      for (const item of node.children) {
        const newItem = replaceNode(item, map);
        if (newItem !== item) {
          return {
            ...node,
            children: node.children?.map((i) =>
              i.id === item.id ? newItem : i
            ),
          };
        }
      }
      return node;
    }
  }
  return node;
}

function editById(
  state: INode,
  payload: {
    id: number;
    props: Partial<INode["props"]>;
  }
): INode {
  if (state.id === payload.id) {
    return { ...state, props: { ...state.props, ...payload.props } };
  } else {
    if (state.children) {
      for (let item of state.children) {
        const newItem = editById(item, payload);
        if (newItem !== item) {
          return {
            ...state,
            children: state.children.map((i) => (i === item ? newItem : i)),
          };
        }
      }
      return state;
    }
    return state;
  }
}

function edit(
  state: INode,
  payload: {
    id: number;
    props: Partial<INode["props"]>;
  }
): INode {
  if (state.id === payload.id) {
    return { ...state, props: { ...state.props, ...payload.props } };
  } else {
    if (state.children) {
      for (let item of state.children) {
        const newItem = edit(item, payload);
        if (newItem !== item) {
          return {
            ...state,
            children: state.children.map((i) => (i === item ? newItem : i)),
          };
        }
      }
      return state;
    }
  }
  return state;
}
function deleteNodeById(node: INode, id: number): INode | undefined {
  if (node.id === id) {
    if (node.root) return node;
    else {
      return undefined;
    }
  } else {
    if (node.children) {
      for (const item of node.children) {
        const newItem = deleteNodeById(item, id);
        if (!newItem) {
          return {
            ...node,
            children: node.children.filter((i) => i !== item),
          };
        } else if (newItem !== item) {
          return {
            ...node,
            children: node.children.map((i) => (i === item ? newItem : i)),
          };
        }
      }
      return node;
    }
    return node;
  }
}
function addNodeByParentId(node: INode, id: number, data: any): INode {
  if (node.id === id) {
    return {
      ...node,
      children: [
        ...(node.children || []),
        { id: new Date().getTime(), ...data },
      ],
    };
  } else {
    if (node.children) {
      for (const item of node.children) {
        const newItem = addNodeByParentId(item, id, data);
        if (newItem !== item) {
          return {
            ...node,
            children: node.children.map((i) => (i === item ? newItem : i)),
          };
        }
      }
      return node;
    }
  }
  return node;
}

export const treeReducer = (state = defaultState, action: NodeAction) => {
  switch (action.type) {
    case EDIT_NODE:
      console.log("action", action);
      // return replaceNode(state, (node) => {
      //   if (node.id === action.payload.id) {
      //     return {
      //       ...node,
      //       props: {
      //         ...node.props,
      //         ...action.payload.props,
      //       },
      //     };
      //   }
      //   return node;
      // });
      return editById(state, action.payload);
    case ADD_NODE:
      return addNodeByParentId(state, action.payload.id, action.payload.data);
    case DELETE_NODE:
      return deleteNodeById(state, action.payload.id);
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
import { useState } from "react";
import Other from "./pages/Other";
import Test from "./pages/Test";
import Home from "./pages/Home";
import Login from "./pages/Login";
import { Switch, Route } from "react-router-dom";
import { useSelector, useStore, useDispatch } from "react-redux";
import { INode } from "./store/reducers";
import { InputNumber } from "antd";
import { useFlattenedTree, useNode } from "./store/hooks";
import {
  createEditAction,
  createAddHobbyAction,
  createEditNodeAction,
  createDeleteNodeAction,
  createAddNodeAction,
} from "./store/reducers";

// function* flattenTree(tree: INode): any {
//   yield tree;
//   if (tree.children) {
//     for (const item of tree.children) {
//       yield* flattenTree(item);
//     }
//   }
// }

// function flattenTree2(tree: INode, result: INode[] = []) {
//   result.push(tree)
//   if(tree.children) {
//     for(const item of tree.children) {
//       flattenTree2(item, result)
//     }
//   }
//   return result
// }

export function* flattenTree(tree: INode): any {
  yield tree;
  if (tree.children) {
    for (const item of tree.children) {
      yield* flattenTree(item);
    }
  }
}

function flattenTree2(tree: INode, result: INode[] = []) {
  result.push(tree);
  if (tree.children) {
    for (const item of tree.children) {
      flattenTree2(item, result);
    }
  }
  return result;
}

function flattenTree3(tree: INode) {
  let result: INode[] = [];
  result.push(tree);
  if (tree.children) {
    for (const item of tree.children) {
      result.push(...flattenTree3(item));
    }
  }
  return result;
}

const App = () => {
  const [money, setMoney] = useState<number>(0);
  const dispatch = useDispatch();
  const user = useSelector((state: any) => state.user);
  const arr = useFlattenedTree();

  // user
  const changeUser = () => {
    dispatch(createEditAction({ name: "www" }));
  };

  const addHobby = () => {
    dispatch(createAddHobbyAction(["study", "music"]));
  };

  // node
  const node = useNode(2);
  console.log(node, "node======");

  const changeMoney = (id: number, money: number) => {
    dispatch(
      createEditNodeAction({
        id,
        props: {
          money,
        },
      })
    );
  };

  const deleteNodeById = (id: number) => {
    dispatch(createDeleteNodeAction({ id }));
  };

  const addNode = (id: number) => {
    dispatch(
      createAddNodeAction({
        id,
        // data: { name: "leader3", props: { money: 30000, car: 1 } },
        data: { name: "www", props: { money: 8000, car: 0 } },
        // data: { name: "leader4", props: { money: 30000, car: 3 } },
      })
    );
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
        <button onClick={() => addNode(3)}>添加node</button>
        {arr.map((item: any) => (
          <div key={item.id}>
            姓名：{item.name}
            薪资：{item.props.money}
            <div style={{ display: "flex" }}>
              <InputNumber
                value={money}
                onChange={(value: any) => setMoney(value)}
              />
              <button onClick={() => changeMoney(item.id, money)}>
                修改薪资
              </button>
            </div>
            <button onClick={() => deleteNodeById(item.id)}>删除node</button>
          </div>
        ))}
      </div>
    </div>
  );
};

export default App;
```
