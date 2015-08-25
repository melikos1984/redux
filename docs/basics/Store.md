# Store

在前面的章節，我們定義了代表實際上「發生了什麼」的 [actions](Action.md)，和依據這些 actions 更新 state 的 [reducers](Reducers.md)。

**Store** 是 brings them together 的物件。store 有以下的責任：

* 掌管應用程式狀態；
* 允許藉由 [`getState()`](../api/Store.md#getState) 獲取 state；
* 允許藉由 [`dispatch(action)`](../api/Store.md#dispatch) 來更新 state；
* 藉由 [`subscribe(listener)`](../api/Store.md#subscribe) 註冊 listeners。

It’s important to note that you’ll only have a single store in a Redux application. When you want to split your data handling logic, you’ll use [reducer composition](Reducers.md#splitting-reducers) instead of many stores.

如果你已經有一個 reducer，要建立 store 非常簡單。 在[前面的章節](Reducers.md)，我們使用 [`combineReducers()`](../api/combineReducers.md) 來把一些 reducers 合併成一個。我們現在要 import 它，並把它傳進 [`createStore()`](../api/createStore.md)。

```js
import { createStore } from 'redux';
import todoApp from './reducers';

let store = createStore(todoApp);
```

You may optionally specify the initial state as the second argument to [`createStore()`](../api/createStore.md). This is useful for hydrating the state of the client to match the state of a Redux application running on the server.

```js
let store = createStore(todoApp, window.STATE_FROM_SERVER);
```

## Dispatching Actions

現在我們已經建立了一個 store，let’s verify our program works! Even without any UI, we can already test the update logic.

```js
import { addTodo, completeTodo, setVisibilityFilter, VisibilityFilters } from './actions';

// 記錄初始 state
console.log(store.getState());

// 每次 state 變更，就記錄它
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

// Dispatch 一些 actions
store.dispatch(addTodo('Learn about actions'));
store.dispatch(addTodo('Learn about reducers'));
store.dispatch(addTodo('Learn about store'));
store.dispatch(completeTodo(0));
store.dispatch(completeTodo(1));
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED));

// 停止監聽 state 的更新
unsubscribe();
```

你可以看到 how this causes the state held by the store to change:

<img src='http://i.imgur.com/zMMtoMz.png' width='70%'>

We specified the behavior of our app before we even started writing the UI. We won’t do this in this tutorial, but at this point you can write tests for your reducers and action creators. You won’t need to mock anything because they are just functions. Call them, and make assertions on what they return.

## 原始碼

#### `index.js`

```js
import { createStore } from 'redux';
import todoApp from './reducers';

let store = createStore(todoApp);
```

## 下一步

在為我們的 todo 應用程式建立 UI 之前，we will take a detour to see [how the data flows in a Redux application](DataFlow.md).
