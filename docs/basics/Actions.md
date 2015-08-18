# Actions

首先，讓我們來定義一些 actions。

**Actions** 是從你的應用程式傳遞資料到你的 store 的資訊 payloads。它們是一個 store *唯一的*資訊來源。你藉由 [`store.dispatch()`](../api/Store.md#dispatch) 傳遞它們到 store。

這是一個 action 的例子，它代表添加一個新的 todo 項目：

```js
{
  type: 'ADD_TODO',
  text: 'Build my first Redux app'
}
```

Actions 是一般的 JavaScript 物件。慣例上，actions 應該有一個字串 `type` 屬性，它代表被執行的 action 的類型。Types 通常應該被定義成字串常數。一旦你的應用程式夠大了，你能會想把它們移進一個 separate module.

```js
import { ADD_TODO, REMOVE_TODO } from '../actionTypes';
```

>##### 關於 Boilerplate 的提醒

>You don’t have to define action type constants in a separate file, or even to define them at all. For a small project, it might be easier to just use string literals for action types. However, there are some benefits to explicitly declaring constants in larger codebases. Read [Reducing Boilerplate](../recipes/ReducingBoilerplate.md) for more practical tips on keeping your codebase clean.

除了 `type` 以外，一個 action 物件的結構完全取決於你。如果你有興趣，請查看 [Flux Standard Action](https://github.com/acdlite/flux-standard-action) 上有關可以如何建構 actions 的建議。

We’ll add one more action type to describe a user ticking off a todo as completed. We refer to a particular todo by `index` because we store them in an array. In a real app it is wiser to generate a unique ID every time something new is created.

```js
{
  type: COMPLETE_TODO,
  index: 5
}
```

It’s a good idea to pass as little data in action as possible. For example, it’s better to pass `index` than the whole todo object.

Finally, we’ll add one more action type for changing the currently visible todos.

```js
{
  type: SET_VISIBILITY_FILTER,
  filter: SHOW_COMPLETED
}
```

## Action Creators

**Action creators** 就是—產生 actions 的 functions。「action」和「action creator」 這兩個詞非常容易混為一談，所以請盡你所能使用正確的術語。

在[傳統的 Flux](http://facebook.github.io/flux) 實作中，action creators 被呼叫時，通常會像這樣觸發一個 dispatch：

```js
function addTodoWithDispatch(text) {
  const action = {
    type: ADD_TODO,
    text
  };
  dispatch(action);
}
```

相比之下，在 Redux 裡 action creators 是沒有副作用的 **pure functions**。它們簡單地回傳一個 action：

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  };
}
```

這使它們更具有可攜性並更容易測試。要實際地啟動一個 dispatch，必須傳遞回傳值給 `dispatch()` function：

```js
dispatch(addTodo(text));
dispatch(completeTodo(index));
```

或是建立會自動地 dispatches 的 **bound action creator**：

```js
const boundAddTodo = (text) => dispatch(addTodo(text));
const boundCompleteTodo = (index) => dispatch(CompleteTodo(index));
```

你將可以直接去呼叫它們：

```
boundAddTodo(text);
boundCompleteTodo(index);
```

`dispatch()` function 可以藉由 [`store.dispatch()`](../api/Store.md#dispatch) 直接地從 store 取用，不過你將更可能會藉由 helper 來取用它，例如 [react-redux](http://github.com/gaearon/react-redux) 的 `connect()`。 You can use [`bindActionCreators()`](../api/bindActionCreators.md) to automatically bind many action creators to a `dispatch()` function.

## 原始碼

### `actions.js`

```js
/*
 * action types
 */

export const ADD_TODO = 'ADD_TODO';
export const COMPLETE_TODO = 'COMPLETE_TODO';
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';

/*
 * 其他常數
 */

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
};

/*
 * action creators
 */

export function addTodo(text) {
  return { type: ADD_TODO, text };
}

export function completeTodo(index) {
  return { type: COMPLETE_TODO, index };
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter };
}
```

## 下一步

現在讓我們來[定義一些 reducers](Reducers.md) 以指定當你 dispatch 這些 actions 時，state 要如何更新！

>##### 給進階使用者的提醒
>如果你已經熟悉基本的概念而且之前已經看完了這份教學，請不要忘記查看在[進階教學](../advanced/README.md)中的 [async actions](../advanced/AsyncActions.md)，以學習如何處理 AJAX 回應和組合 action creators 到非同步控制流程中。
