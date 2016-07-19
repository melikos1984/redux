# 範例

Redux 是隨著一些範例在它的[原始碼](https://github.com/reactjs/redux/tree/master/examples)中一起發佈的。

## Counter Vanilla

執行 [Counter Vanilla](https://github.com/reactjs/redux/tree/master/examples/counter-vanilla) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/counter-vanilla
open index.html
```

它不需要構建系統或是 view 的框架，僅是用來展示採用 ES5 的原生 Redux API。

## Counter

執行 [Counter](https://github.com/reactjs/redux/tree/master/examples/counter) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/counter
npm install
npm start

open http://localhost:3000/
```

這是將 Redux 和 React 一起使用的最基礎範例。為了簡化，它在 store 更新時手動重新 render React component。在真實世界專案中，你會想要使用更高效的 [React Redux](https://github.com/reactjs/react-redux) 來合併。

此範例包含了測試。

## Todos

執行 [Todos](https://github.com/reactjs/redux/tree/master/examples/todos) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/todos
npm install
npm start

open http://localhost:3000/
```

這是深入了解 state 的更新如何在 Redux 中與 component 一起運作的最佳範例。它展示 reducer 能如何指派正在 handle 的 action 給其他 reducer，並且展示你可以如何從你的 presentational component 中使用 [React Redux](https://github.com/reactjs/react-redux) 來產生 container component。

此範例包含了測試。

## Todos with Undo

執行 [Todos with Undo](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/todos-with-undo
npm install
npm start

open http://localhost:3000/
```

這是前一個範例的變形。它大部分相同，並額外展示如何用 [Redux Undo](https://github.com/omnidan/redux-undo) 的幾行程式包裝你的 reducer 來使你增加 Undo/Redo 功能到你的app中。

## TodoMVC

執行 [TodoMVC](https://github.com/reactjs/redux/tree/master/examples/todomvc) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/todomvc
npm install
npm start

open http://localhost:3000/
```

這是經典的 [TodoMVC](http://todomvc.com/) 範例。它放在這不只是為了比較，更涵蓋了與 Todos 範例相同的重點。

此範例包含了測試。

## Shopping Cart

執行 [Shopping Cart](https://github.com/reactjs/redux/tree/master/examples/shopping-cart) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/shopping-cart
npm install
npm start

open http://localhost:3000/
```

這個範例展示了當你的 app 成長時將逐漸重要的慣用 Redux pattern。它尤其展示了如何由 IDs 標準化儲存 entity、如何在不同層面上建構 reducer、如何依靠 reducer 定義 selector 使得 state shape 的含義得以被封裝在內。它也展示如何由 [Redux Logger](https://github.com/fcomb/redux-logger) 來 log 和由 [Redux Thunk](https://github.com/gaearon/redux-thunk) middleware 來達成有條件的 dispatch action。

## Tree View

執行 [Tree View](https://github.com/reactjs/redux/tree/master/examples/tree-view) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/tree-view
npm install
npm start

open http://localhost:3000/
```

這個範例展示如何 render 一個深度巢狀的樹狀 view，並用一個標準化形式代表它的 state，所以它可以簡單的由 reducer 更新。經由 container component 細緻地 subscribe 其 render tree 節點 ，可以達到優良的 render 效能。

此範例包含了測試。

## Async

執行 [Async](https://github.com/reactjs/redux/tree/master/examples/async) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/async
npm install
npm start

open http://localhost:3000/
```

這個範例包含了如何讀取非同步 API、抓取使用者輸入資料、顯示讀取中標記、快取 response、快取無效化。它使用 [Redux Thunk](https://github.com/gaearon/redux-thunk) middleware 來封裝非同步行為。

## Universal

執行 [Universal](https://github.com/reactjs/redux/tree/master/examples/universal) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/universal
npm install
npm start

open http://localhost:3000/
```

這是 Redux 和 React [伺服器端 rendering](../recipes/ServerRendering.md) 的基礎展示。它展示如何在伺服器端準備初始 store state 並傳遞到客戶端，所以客戶端 store 可以由現存的 state 開始啟動。

## Real World

執行 [Real World](https://github.com/reactjs/redux/tree/master/examples/real-world) 範例：

```
git clone https://github.com/reactjs/redux.git

cd redux/examples/real-world
npm install
npm start

open http://localhost:3000/
```

這是最進階的範例。它被高度地設計過。它涵蓋了由標準化快取中取得 entity、為了 API 呼叫實作客製化 middleware、render 部分讀取的資料、分頁化、快取 responses、顯示錯誤訊息、routing。此外，它也包含了 Redux DevTools。

## 更多範例

你可以在 [Awesome Redux](https://github.com/xgrommx/awesome-redux) 找到更多範例。
