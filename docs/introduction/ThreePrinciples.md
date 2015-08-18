# 三大原則

Redux 可以用三個基本的原則來描述：

### 唯一真相來源

**你整個應用程式的 [state](../Glossary.md#state)，被儲存在一個樹狀物件放在唯一的 [store](../Glossary.md#store) 裡面。**

這讓建立 universal 應用程式變得更簡單。從 server 來的 state 可以被 serialized 並 hydrated 進去客戶端，而不需要撰寫其他額外的程式碼。當只有一個 state tree 時，比較容易去 debug 一個應用程式。你也可以為了加快開發週期，在開發期間保存應用程式的 state。伴隨著單一的 state tree，你還可以輕易得到在以前很困難實現的功能，例如：復原/重做。

```js
console.log(store.getState());

{
  visibilityFilter: 'SHOW_ALL',
  todos: [{
    text: 'Consider using Redux',
    completed: true,
  }, {
    text: 'Keep all state in a single tree',
    completed: false
  }]
}
```

### State 是唯讀的

**改變 state 的唯一的方式是發出一個 [action](../Glossary.md#action)，也就是一個描述發生什麼事的物件。**

這能確保 views 或網路 callbacks 永遠不會直接寫入 state，而是表達了變更的意圖。因為所有的變更都是集中的，並依照嚴格的順序一個接一個的發生，沒有需要特別注意的微妙 race conditions。Actions 只是普通物件，所以它們可以被記錄、serialized、儲存、並在之後為了 debugging 或測試目的而重播。

```js
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
});

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
});
```

### 變更被寫成 pure functions

**要指定 state tree 如何藉由 actions 來轉變，你必須撰寫 pure [reducers](../Glossary.md#reducer)。**

Reducers 只是 pure functions，它取得先前的 state 和一個 action，並回傳下一個 state。請記得要回傳一個新的 state 物件，而不要去改變先前的 state。你可以從單一一個 reducer 開始，但是隨著你的應用程式成長，你可以把拆分成比較小的 reducers 來管理 state tree 中的特定部分。因為 reducers 只是 functions，你可以控制它們被呼叫的順序、傳遞額外的資料、或甚至建立可重用的 reducers 來做一些常見的任務，例如 pagination。

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
  case 'SET_VISIBILITY_FILTER':
    return action.filter;
  default:
    return state;
  }
}

function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return [...state, {
      text: action.text,
      completed: false
    }];
  case 'COMPLETE_TODO':
    return [
      ...state.slice(0, action.index),
      Object.assign({}, state[action.index], {
        completed: true
      }),
      ...state.slice(action.index + 1)
    ];
  default:
    return state;
  }
}

import { combineReducers, createStore } from 'redux';
let reducer = combineReducers({ visibilityFilter, todos });
let store = createStore(reducer);
```

就這樣！現在你已經知道 Redux 都是些什麼了。
