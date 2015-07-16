# 入門

不要被所有有關 reducers、middleware、higher-order stores 的花俏談話給騙了 — Redux 令人難以置信的簡單。如果你曾經建置過一個 Flux 應用程式，你會感覺像是回到家了。(如果你剛開始接觸 Flux，它也很簡單！)

在這份指南中，我們將帶過建立一個簡單 Todo 應用程式的過程。

## Actions

首先，讓我們來定義一些 actions。

**Actions** 是從你的應用程式傳遞資料到你的 store 的資訊 payloads。它們是一個 store *唯一的* 資訊來源。你藉由 `store.dispatch()` 傳遞它們到 store。

這是一個 action 的例子，它代表添加一個新的 todo 項目：

```js
{
  type: ADD_TODO,
  payload: {
    text: 'Build my first Redux app'
  }
}
```

Actions 是一般的 JavaScript 物件。慣例上，actions 應該有一個 `type` 屬性，它代表被執行的 action 的類型。Types 通常應該被定義成常數並從其他 module import。

```js
import { ADD_TODO, REMOVE_TODO } from '../actionTypes';
```

除了 `type` 以外，一個 action 物件的結構完全取決於你。如果你有興趣，請查看 [Flux Standard Action](https://github.com/acdlite/flux-standard-action) 上有關應該如何建構 actions 的建議。

我們需要另一個 action type，用來移除代辦事項：

```js
{
  type: REMOVE_TODO,
  payload: 123
}
```

`payload` 在這個例子中代表我們想要移除的 todo 的 id。

## Action creators

**Action creators** 就是 — 產生 actions 的 functions。「action」和「action creator」 這兩個詞非常容易混為一談，所以請盡你所能使用正確的術語。

在*其他* Flux 實作中，action creators 被呼叫時，通常會像這樣觸發一個 dispatch：

```js
function addTodoWithDispatch(text) {
  const action = {
    type: ADD_TODO,
    payload: {
      text
    }
  };
  dispatch(action);
}
```

相比之下，在 Redux 裡 action creators 是沒有副作用的 **pure functions**。它們簡單地回傳一個 action：

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    payload: {
      text
    }
  };
}
```

這使它們更具有可攜性並更容易測試。要實際地啟動一個 dispatch，必須傳遞回傳值給 `dispatch()` function：

```js
dispatch(addTodo(text));
dispatch(removeTodo(id));
```

或是建立會自動地 dispatches 的 **bound action creator**：

```js
const boundAddTodo = text => dispatch(addTodo(text));
const boundRemoveTodo = id => dispatch(addTodo(id));
```

這是一個 **higher-order function** 的例子。當使用 Redux 時，你將會發現有很多這種模式。但是不要害怕 — higher-order function 就只是一個回傳其他 function 的 function。就像你所看到的，ES6 arrow functions 讓使用 higher-order functions 變得輕而易舉。

`dispatch()` function 可以像 `store.dispatch()` 直接地從 store 取用，不過你將更可能會藉由 helper 來取用它，例如 react-redux 的 `connect()`。

## Reducers

現在讓我們來建立我們的 store 來回應我們前面定義的 action。

跟其他的 Flux 實作不同，Redux 只有一個 store，而不是在你的應用程式中每個 domain 的資料有不同的 store。代替建立多個 stores 來手動地管理它們自己內部 state 的方式，我們建立 **reducers** 來指定要如何計算 state 來回應新的 actions。

一個 reducer 看起來像這樣：

```js
(previousState, action) => newState
```

這就是你會傳遞給 `Array.prototype.reduce(reducer, ?initialValue)` 的 function 類型。

這或許看起來很激進，但事實證明這個 function signature 足夠表達完整的傳統 Flux store 模型 — 用一個純粹 functional 的方式。**這是 Redux 的本質**。這就是使像是 hot reloading、time travel 和 universal rendering 等所有酷炫功能成真的東西。然而撇除這一些，它只是個能更好用於表達 state 變更的模型。

[**觀看 Dan 在 React Europe 的談話了解更多有關這個主題的內容**](https://www.youtube.com/watch?v=xsSnOQynTHs).

讓我們來建立一個 reducer 給我們的 Todo 應用程式：

```js
const initialState = { todos: [], idCounter: 0 };

function todoReducer(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [
          ...state.todos,
          { text: action.payload, id: state.idCounter + 1 }
        ],
        idCounter: state.idCounter + 1
      };
    case REMOVE_TODO:
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload)
      };
    default:
      return state;
  }
}
```

哇，這怎麼一回事？有幾件事情需要注意：

- `state` 是 store 先前的 state。在你建立你的 store 之後，Redux 會立即 dispatch 一個假 action，在那個時候 `state` 會是 undefined。在那之後。`state` 會是前一次 reducer 回傳的值。
- 我們藉由一個預設參數來指定 store 的 initial state。
- 我們藉由一個 switch statement 來分辨 action type。
- **我們不改變先前的 state** — 我們基於**先前的** state 物件回傳一個**新的** state 物件。

最後一點尤其重要：永遠不要去改變先前的 state 物件。每次都必須回傳一個新的 state。請記住，reducers 是 pure functions，不應該進行改變或是有副作用的行為。這裡我們藉由 ES7 spread operator 來深層複製舊 state 的值到一個新物件。你可以使用像 Immutable.js 之類的 library 來追求夠好的 API 與更佳的效能，因為它使用 [persistent data structures](http://en.wikipedia.org/wiki/Persistent_data_structure)。下面就是同樣的 store 使用 immutable 的值將會看起來的樣子：

```js
const initialState = Immutable.Map({ todos: [], idCounter: 0 });

function todoReducer(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return state
        .update('todos',
          todos => todos.push(Immutable.Map({
            text: action.payload,
            id: state.get('idCounter')
          })
        ))
        .set('idCounter', state.get('idCounter') + 1);
    case REMOVE_TODO:
      return state
        .update('todos',
          todos => todos.filter(todo => todo.id !== action.payload )
        );
    default:
      return state;
  }
}
```

如果你在想說「呸，switch statements」，請記住 reducers 只是個 functions — 你可以藉由 helpers 把這些細節抽象化。請查看 [redux-actions](https://github.com/acdlite/redux-actions) 作為範例來了解如何使用 higher-order functions 來建立 reducers。

## 建立一個 store

現在我們有一些 action creators 和一個處理它們的 reducer。下一步是建立我們的 store。

要建立一個 store，必須傳遞一個 reducer 給 `createStore()`：

```js
import { createStore } from 'redux';
import todoReducer from '../reducers/todos';
const store = createStore(todoReducer);
```

通常你會有許多針對不同 domains 的資料的 reducers 在你的應用程式裡面。你可以使用 `combineReducers()` helper 來把多個 reducers 結合成為一個：

```js
import { createStore, combineReducers } from 'redux';
import * as reducers from '../reducers';
const reducer = combineReducers(reducers);
const store = createStore(reducer);
```

例如，如果傳遞給 `combineReducers()` 的物件看起來像這樣：

```js
const reducers = {
  todos: todoReducer,
  counter: counterReducer
};
```

它會建立一個 reducer，而它會產生像這樣的 state 物件：

```js
const state = {
  todos: todoState,
  counter: counterState
};
```

## Middleware

Middleware 是 Redux 一個選擇性的功能，它可以讓你客製化要如何處理 dispatches。把 middleware 想成是 Redux 某種類型的 plugin 或 extension。

middleware 的一個常見用途是用來啟用非同步的 dispatches。例如，promise middleware 增加了對 dispatching promises 的支援：

```js
dispatch(Promise.resolve({ type: ADD_TODO, payload: { text: 'Use middleware!' } }));
```
promise middleware 會偵測有 promise 被 dispatched，攔截它並取而代之在未來的時間點以 resolved 的值來 dispatch。

可以非常簡單地藉由 function composition 來建立 Middleware。在這份文件中，我們並不打算聚焦在 middleware 如何運作，不過以下是如何在你建立的 store 時候啟用它：

```js
import { createStore, applyMiddleware } from 'redux';
// where promise, thunk, and observable are examples of middleware
const store = applyMiddleware(promise, thunk, observable)(createStore)(reducer);
```

是的，你沒看錯。[在這裡閱讀更多有關 middleware 如何運作的內容。](middleware.md)

## 連結到你的 views

你很少會直接與 store 物件互動。大多數情況下，你會把它用某種方式綁定到你偏好的 view library。

Flux 在 React 社群裡最流行，不過 Redux 可以與任何種類的 view layer 一起運作。React 跟 Redux 的綁定也是可以的，例如 react-redux — 請查看專案來了解如何與 React 整合的細節。

然而，如果你發現自己需要直接地取用 store，要這樣做的 API 也非常簡單：

- `store.dispatch()` dispatches 一個 action。
- `store.getState()` 取得現在的 state。
- `store.subscribe()` 註冊一個會在每一次 dispatch 之後被呼叫的 listener，並回傳一個你可以用來 unsubscribe 的 function。


## 開始讓一些東西更好

就是這樣！正如你所看到的，儘管 Redux 提供強大的功能，Redux 的核心真的非常簡單。

如果你有這份指南可以如何改善的建議，請讓我們知道。

原文連結: [https://github.com/gaearon/redux/blob/improve-docs/docs/getting-started.md](https://github.com/gaearon/redux/blob/improve-docs/docs/getting-started.md)
