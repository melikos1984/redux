# 計算衍生資料

[Reselect](https://github.com/faassen/reselect.git) 是一個簡單的 library 可以用來建立會自動記憶、可組合的 **selector** functions。可以用 Reselect 的 selectors 來有效率的從 Redux store 計算衍生資料。

### 使用 Memoized Selectors 的動機

讓我們再來看看 [Todos 清單的範例](../basics/UsageWithReact.md)：

#### `containers/VisibleTodoList.js`

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'

const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

在上面的範例中，`mapStateToProps` 呼叫 `getVisibleTodos` 以計算 `todos`。這運作得很好，不過有一個缺點：在每次 component 更新的時候都會計算 `todos`。如果 state tree 很大，或是計算的代價很大，在每次更新的時候重複計算可能會造成效能問題。Reselect 可以幫助我們避免這些不需要的重複計算。

### 建立一個 Memoized Selector

我們會想要把 `getVisibleTodos` 換成一個 memoized selector，它會在 `state.todos` 或是 `state.visibilityFilter` 的值變更的時後重新計算 `todos`，但當變更發生在 state tree 其他 (不相關的) 的部分的時候則不會。

Reselect 提供一個 `createSelector` function 來建立 memoized selectors。`createSelector` 接收一個 input-selectors 陣列和一個轉換 function 當作它的參數。如果用一個會造成 input-selector 的值改變的方式去變動 Redux 的 state tree，selector 將會以 input-selectors 的值作為參數呼叫它的轉換 function 並回傳結果。如果 input-selectors 的值跟先前呼叫 selector 的時候一樣，它會回傳先前的計算結果而不會呼叫轉換 function。

讓我們來定義一個叫做 `getVisibleTodos` 的 memoized selector 來取代上面的 non-memoized版本：

#### `selectors/index.js`

```js
import { createSelector } from 'reselect'

const getVisibilityFilter = (state) => state.visibilityFilter
const getTodos = (state) => state.todos

export const getVisibleTodos = createSelector(
  [ getVisibilityFilter, getTodos ],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

在上面的範例中，`getVisibilityFilter` 和 `getTodos` 就是 input-selectors。因為它們沒有轉換它們選擇的資料，所以被建立成普通的非 memoized selector functions。而另一方面，`getVisibleTodos` 是一個 memoized selector。它接收 `getVisibilityFilter` 和 `getTodos` 作為 input-selectors，以及一個計算過濾後的 todos 清單的轉換 function。

### 組合 Selectors

一個 memoized selector 可以是另一個 memoized selector 的 input-selector。這裡 `getVisibleTodos` 被用來當作 selector 的 input-selector，以藉由 keyword 進一步的過濾 todos：

```js
const getKeyword = (state) => state.keyword

const getVisibleTodosFilteredByKeyword = createSelector(
  [ getVisibleTodos, getKeyword ],
  (visibleTodos, keyword) => visibleTodos.filter(
    todo => todo.text.indexOf(keyword) > -1
  )
)
```

### 把 Selector 連結到 Redux Store

如果你是使用 [React Redux](https://github.com/reactjs/react-redux)，你可以在 `mapStateToProps()`內呼叫 selectors 當作正規的 functions使用：

#### `containers/VisibleTodoList.js`

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state)
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

## 接下來

查看 Reselect [官方文件](https://github.com/rackt/reselect) 和它的 [FAQ](https://github.com/rackt/reselect#faq)。 大部分 Redux 專案遇到因太多衍生運算和浪費地重複 render 而造成效能問題時，都已開始使用 Reselect 。所以在你開始建構某些大專案前確保你已熟悉它。讀 [它的原始碼](https://github.com/rackt/reselect/blob/master/src/index.js) 也很有幫助，這讓你不會覺得它很神奇。
