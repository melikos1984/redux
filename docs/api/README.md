# API 參考

Redux 暴露出來的 API 非常少。Redux 定義一系列的介面給你去實作 (例如 [reducers](../Glossary.md#reducer)) 並提供少數的 helper functions 來把這些介面綁在一起。

這個章節把 Redux API 完整的文件化。Keep in mind that Redux is only concerned with managing the state. In a real app, you’ll also want to use UI bindings like [react-redux](https://github.com/gaearon/react-redux).

### 頂層 Exports

* [createStore(reducer, [initialState])](createStore.md)
* [combineReducers(reducers)](combineReducers.md)
* [applyMiddleware(...middlewares)](applyMiddleware.md)
* [bindActionCreators(actionCreators, dispatch)](bindActionCreators.md)
* [compose(...functions)](compose.md)

### Store API

* [Store](Store.md)
  * [getState()](Store.md#getState)
  * [dispatch(action)](Store.md#dispatch)
  * [subscribe(listener)](Store.md#subscribe)
  * [getReducer()](Store.md#getReducer)
  * [replaceReducer(nextReducer)](Store.md#replaceReducer)

### Importing

Every function described above is a top-level export. You can import any of them like this:

#### ES6

```js
import { createStore } from 'redux';
```

#### ES5 (CommonJS)

```js
var createStore = require('redux').createStore;
```

#### ES5 (UMD build)

```js
var createStore = Redux.createStore;
```
