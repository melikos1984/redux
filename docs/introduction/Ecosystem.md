# 生態系

Redux 是個非常小的 library，但它的介面和 APIs 都是精挑細選來衍生出工具和 extensions 的生態系。

如果需要一個有關 Redux 一切的廣泛清單，我們推薦 [Awesome Redux](https://github.com/xgrommx/awesome-redux)。它包含範例、boilerplates、middleware、utility libraries 和許多其他的東西。

在這個頁面上，我們將只列出它們之中一些 Redux 維護者已經親自審核的。不要因此而打消你嘗試其他項目的念頭！這個生態系成長得太快，我們沒有足夠的時間去查看所有的東西。請把這些當作「員工推薦」，如果你已經使用 Redux 做出了很棒的東西，請不要猶豫馬上提交一個 PR。

## 學習 Redux

### Screencasts

* **[Getting Started with Redux](https://egghead.io/series/getting-started-with-redux)** — 直接從 Redux 的作者學習它的基礎 (30 部免費影片)

### 應用程式範例

* [SoundRedux](https://github.com/andrewngu/sound-redux) — 一個用 Redux 打造的 SoundCloud 客戶端
* [Shopping Cart (Flux Comparison)](https://github.com/voronianski/flux-comparison/tree/master/redux) — 一個從 Flux Comparison 來的購物車範例

### 教學和文章

* [Redux Tutorial](https://github.com/happypoulp/redux-tutorial) — 一步一步的學習如何使用 redux
* [Redux Egghead Course Notes](https://github.com/tayiorbeii/egghead.io_redux_course_notes) — Redux [Egghead 影片課程](https://egghead.io/series/getting-started-with-redux) 的筆記
* [What the Flux?! Let’s Redux.](https://blog.andyet.com/2015/08/06/what-the-flux-lets-redux) — 一個 Redux 的介紹
* [A cartoon intro to Redux](https://code-cartoons.com/a-cartoon-intro-to-redux-3afb775501a6) — 一個視覺化的 Redux 資料流說明
* [Understanding Redux](http://www.youhavetolearncomputers.com/blog/2015/9/15/a-conceptual-overview-of-redux-or-how-i-fell-in-love-with-a-javascript-state-container) — 學習 Redux 的基礎概念
* [Handcrafting an Isomorphic Redux Application (With Love)](https://medium.com/@bananaoomarang/handcrafting-an-isomorphic-redux-application-with-love-40ada4468af4) — 一個藉由資料抓取與 routing 建立 universal 應用程式的教學
* [Full-Stack Redux Tutorial](http://teropa.info/blog/2015/09/10/full-stack-redux-tutorial.html) — 一個使用 Redux、React 與 Immutable 進行測試先行開發的詳盡指南
* [Understanding Redux Middleware](https://medium.com/@meagle/understanding-87566abcfb7a#.l033pyr02) — 實作 Redux middleware 的深入指南
* [A Simple Way to Route with Redux](http://jlongster.com/A-Simple-Way-to-Route-with-Redux) — 一個 Redux Simple Router 的介紹

### 演講

* [Live React: Hot Reloading and Time Travel](http://youtube.com/watch?v=xsSnOQynTHs) — 看看 Redux 強制的限制如何讓隨著時間旅行 hot reload 變得簡單
* [Cleaning the Tar: Using React within the Firefox Developer Tools](https://www.youtube.com/watch?v=qUlRpybs7_c) — 學習如何漸漸的把既存的 MVC 應用程式遷移到 Redux
* [Redux: Simplifying Application State](https://www.youtube.com/watch?v=okdC5gcD-dM) — Redux 架構的介紹

## 使用 Redux

### 綁定

* [react-redux](https://github.com/gaearon/react-redux) — React
* [ng-redux](https://github.com/wbuchwalter/ng-redux) — Angular
* [ng2-redux](https://github.com/wbuchwalter/ng2-redux) — Angular 2
* [backbone-redux](https://github.com/redbooth/backbone-redux) — Backbone
* [redux-falcor](https://github.com/ekosz/redux-falcor) — Falcor
* [deku-redux](https://github.com/troch/deku-redux) — Deku

### Middleware

* [redux-thunk](http://github.com/gaearon/redux-thunk) — 撰寫 async action creators 的最簡單方式
* [redux-promise](https://github.com/acdlite/redux-promise) — [FSA](https://github.com/acdlite/flux-standard-action)-相容的 promise middleware
* [redux-rx](https://github.com/acdlite/redux-rx) — Redux 的 RxJS utilities，包含一個 Observable 的 middleware
* [redux-batched-updates](https://github.com/acdlite/redux-batched-updates) — 讓 Redux dispatches 的結果批次發生 React 更新
* [redux-logger](https://github.com/fcomb/redux-logger) — 記錄每一個 Redux action 和下一個 state
* [redux-immutable-state-invariant](https://github.com/leoasis/redux-immutable-state-invariant) — 在開發時，警告 state 的變更
* [redux-analytics](https://github.com/markdalgleish/redux-analytics) — Redux 的分析用 middleware
* [redux-gen](https://github.com/weo-edu/redux-gen) — Redux 的 Generator middleware
* [redux-saga](https://github.com/yelouafi/redux-saga) — 一個 Redux 應用程式的 side effect model 方案

### Routing

* [react-router-redux](https://github.com/rackt/react-router-redux) — 用非常簡單的綁定來保持 React Router 和 Redux 同步
* [redux-router](https://github.com/acdlite/redux-router) — React Router 的 Redux 綁定

### Components

* [redux-form](https://github.com/erikras/redux-form) — 把 React 表單 state 保存在 Redux 裡

### Enhancers

<<<<<<< HEAD
* [redux-batched-subscribe](https://github.com/tappleby/redux-batched-subscribe) — 客製化 batching 以及 debouncing 的呼叫給 store 的訂閱者
* [redux-history-transitions](https://github.com/johanneslumpe/redux-history-transitions) — 基於任意的 actions 來處理 History transitions
* [redux-optimist](https://github.com/ForbesLindesay/redux-optimist) — 樂觀地使用可以在之後 commit 或 revert 的 actions
* [redux-undo](https://github.com/omnidan/redux-undo) — 輕鬆的 undo/redo 並在你的 reducers 的歷史間活動
* [redux-ignore](https://github.com/omnidan/redux-ignore) — 藉由陣列或是 filter function 來忽略 redux actions
* [redux-recycle](https://github.com/omnidan/redux-recycle) — 在收到特定的 actions 時重置 redux state
* [redux-batched-actions](https://github.com/tshelburne/redux-batched-actions) — 用只通知 subscriber 一次的方式來 Dispatch 多個 actions
* [redux-search](https://github.com/treasure-data/redux-search) — 自動地幫資源在 web worker 中建立索引並透過非阻塞的方式搜尋它們
* [redux-electron-store](https://github.com/samiskin/redux-electron-store) — 跨越 Electron processes 同步 Redux stores 的 Store enhancers
* [redux-loop](https://github.com/raisemarketplace/redux-loop) — 透過從 Reducer 返回它自然和純粹序列的影響

### Utilities

* [reselect](https://github.com/faassen/reselect) — 有效地獲取資料選擇器，靈感來自 NuclearJS
* [normalizr](https://github.com/gaearon/normalizr) — 正規化巢狀的 API 回應讓 reducers 易於處理
* [redux-actions](https://github.com/acdlite/redux-actions) — 減少撰寫 reducers 和 action creators 的 boilerplate
* [redux-act](https://github.com/pauldijou/redux-act) — 一個自己用來建立 reducers 和 action creators 的 library
* [redux-transducers](https://github.com/acdlite/redux-transducers) — Redux 的 Transducer utilities
* [redux-immutablejs](https://github.com/indexiatech/redux-immutablejs) - Redux & [Immutable](https://github.com/facebook/immutable-js/) 之間的整合工具
* [redux-undo](https://github.com/omnidan/redux-undo) — 輕鬆的 undo/redo 並在你的 reducers 的歷史間活動
* [redux-tcomb](https://github.com/gcanti/redux-tcomb) — Immutable 與給 Redux 用的 type-checked state 和 actions
* [redux-mock-store](https://github.com/arnaudbenard/redux-mock-store) — Mock redux store 以測試你的應用程式
=======
* [redux-batched-subscribe](https://github.com/tappleby/redux-batched-subscribe) — Customize batching and debouncing calls to the store subscribers
* [redux-history-transitions](https://github.com/johanneslumpe/redux-history-transitions) — History transitions based on arbitrary actions
* [redux-optimist](https://github.com/ForbesLindesay/redux-optimist) — Optimistically apply actions that can be later committed or reverted
* [redux-undo](https://github.com/omnidan/redux-undo) — Effortless undo/redo and action history for your reducers
* [redux-ignore](https://github.com/omnidan/redux-ignore) — Ignore redux actions by array or filter function
* [redux-recycle](https://github.com/omnidan/redux-recycle) — Reset the redux state on certain actions
* [redux-batched-actions](https://github.com/tshelburne/redux-batched-actions) — Dispatch several actions with a single subscriber notification
* [redux-search](https://github.com/treasure-data/redux-search) — Automatically index resources in a web worker and search them without blocking
* [redux-electron-store](https://github.com/samiskin/redux-electron-store) — Store enhancers that synchronize Redux stores across Electron processes
* [redux-loop](https://github.com/raisemarketplace/redux-loop) — Sequence effects purely and naturally by returning them from your reducers

### Utilities

* [reselect](https://github.com/faassen/reselect) — Efficient derived data selectors inspired by NuclearJS
* [normalizr](https://github.com/gaearon/normalizr) — Normalize nested API responses for easier consumption by the reducers
* [redux-actions](https://github.com/acdlite/redux-actions) — Reduces the boilerplate in writing reducers and action creators
* [redux-act](https://github.com/pauldijou/redux-act) — An opinionated library for making reducers and action creators
* [redux-transducers](https://github.com/acdlite/redux-transducers) — Transducer utilities for Redux
* [redux-immutable](https://github.com/gajus/redux-immutable) — Used to create an equivalent function of Redux `combineReducers` that works with [Immutable.js](https://facebook.github.io/immutable-js/) state.
* [redux-tcomb](https://github.com/gcanti/redux-tcomb) — Immutable and type-checked state and actions for Redux
* [redux-mock-store](https://github.com/arnaudbenard/redux-mock-store) — Mock redux store for testing your app
>>>>>>> 9b88b3d4622cd6e0d778e4698f7df7d4d6490c9d

### 開發者工具

* [Redux DevTools](http://github.com/gaearon/redux-devtools) — 一個 action 記錄器，它伴隨著 time travel UI、hot reload 和 reducers 的錯誤處理，[在 React Europe 首次展示](https://www.youtube.com/watch?v=xsSnOQynTHs)
* [Redux DevTools Extension](https://github.com/zalmoxisus/redux-devtools-extension) — 一個包裝了 Redux DevTools 並提供額外功能的 Chrome 擴充套件

### 社群慣例

* [Flux Standard Action](https://github.com/acdlite/flux-standard-action) — 一個 Flux action 物件 的人性化標準
* [Canonical Reducer Composition](https://github.com/gajus/canonical-reducer-composition) — 一個巢狀 reducer composition 的自訂標準
* [Ducks: Redux Reducer Bundles](https://github.com/erikras/ducks-modular-redux) — 一個 關於包裝 reducers、action types 和 actions 的建議

### 翻譯

* [中文文档](http://camsong.github.io/redux-in-chinese/) — Chinese
* [繁體中文文件](https://github.com/chentsulin/redux) — Traditional Chinese
* [Redux in Russian](https://github.com/rajdee/redux-in-russian) — Russian

## 更多

[Awesome Redux](https://github.com/xgrommx/awesome-redux) 是一個廣泛的 Redux 相關 repositories 的清單。
