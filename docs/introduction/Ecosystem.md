# 生態系

Redux 是個非常小的 library，但它的介面和 APIs 都是精挑細選來衍生出工具和 extensions 的生態系。

如果需要一個有關 Redux 一切的廣泛清單，我們推薦 [Awesome Redux](https://github.com/xgrommx/awesome-redux)。它包含範例、boilerplates、middleware、utility libraries 和許多其他的東西。

在這個頁面上，我們將只列出它們之中一些 Redux 維護者已經親自審核的。不要因此而打消你嘗試其他項目的念頭！這個生態系成長得太快，我們沒有足夠的時間去查看所有的東西。請把這些當作「員工推薦」，如果你已經使用 Redux 做出了很棒的東西，請不要猶豫馬上提交一個 PR。

## 綁定

* [react-redux](https://github.com/gaearon/react-redux) — React
* [ng-redux](https://github.com/wbuchwalter/ng-redux) — Angular
* [ng2-redux](https://github.com/wbuchwalter/ng2-redux) — Angular 2

## Middleware

* [redux-thunk](http://github.com/gaearon/redux-thunk) — 撰寫 async action creators 的最簡單方式
* [redux-promise](https://github.com/acdlite/redux-promise) — [FSA](https://github.com/acdlite/flux-standard-action)-相容的 promise middleware
* [redux-rx](https://github.com/acdlite/redux-rx) — Redux 的 RxJS utilities，包含一個 Observable 的 middleware
* [redux-batched-updates](https://github.com/acdlite/redux-batched-updates) — 讓 Redux dispatches 的結果批次發生 React 更新
* [redux-logger](https://github.com/fcomb/redux-logger) — 記錄每一個 Redux action 和下一個 state
* [redux-immutable-state-invariant](https://github.com/leoasis/redux-immutable-state-invariant) — 在開發時，警告 state 的變更

## Components

* [redux-form](https://github.com/erikras/redux-form) — 把 React 表單 state 保存在 Redux 裡

## Store enhancers

* [redux-batched-subscribe](https://github.com/tappleby/redux-batched-subscribe) — 客製化 batching 以及 debouncing 的呼叫給 store 的訂閱者

## Utilities

* [reselect](https://github.com/faassen/reselect) — 有效地獲取資料選擇器，靈感來自 NuclearJS
* [normalizr](https://github.com/gaearon/normalizr) — 正規化巢狀的 API 回應讓 reducers 易於處理
* [redux-actions](https://github.com/acdlite/redux-actions) — 減少撰寫 reducers 和 action creators 的 boilerplate
* [redux-transducers](https://github.com/acdlite/redux-transducers) — Redux 的 Transducer utilities
* [redux-immutablejs](https://github.com/indexiatech/redux-immutablejs) - Redux & [Immutable](https://github.com/facebook/immutable-js/) 之間的整合工具
* [redux-undo](https://github.com/omnidan/redux-undo) — 輕鬆的 undo/redo 並在你的 reducers 的歷史間活動
* [redux-tcomb](https://github.com/gcanti/redux-tcomb) — Immutable 與給 Redux 用的 type-checked state 和 actions

## 開發者工具

* [redux-devtools](http://github.com/gaearon/redux-devtools) — 一個 action 記錄器，它伴隨著 time travel UI、hot reloading 和 reducers 的錯誤處理，[在 React Europe 首次展示](https://www.youtube.com/watch?v=xsSnOQynTHs)

## 教學和文章

* [redux-tutorial](https://github.com/happypoulp/redux-tutorial) — 一步一步的學習如何使用 redux
* [What the Flux?! Let’s Redux.](https://blog.andyet.com/2015/08/06/what-the-flux-lets-redux) — 一個 Redux 的介紹
* [Handcrafting an Isomorphic Redux Application (With Love)](https://medium.com/@bananaoomarang/handcrafting-an-isomorphic-redux-application-with-love-40ada4468af4) — 一個隨著資料抓取與 routing 建立 universal 應用程式的教學
* [Full-Stack Redux Tutorial](http://teropa.info/blog/2015/09/10/full-stack-redux-tutorial.html) — 一個使用 Redux、React 與 Immutable 進行測試先行開發的詳盡指南

## 更多

[Awesome Redux](https://github.com/xgrommx/awesome-redux) 是一個廣泛的 Redux 相關 repositories 的清單。
