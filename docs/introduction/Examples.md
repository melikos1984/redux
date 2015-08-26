# 範例

Redux 是隨著一些範例在它的[原始碼](https://github.com/rackt/redux/tree/master/examples)中一起發佈的。
>##### 關於複製的提醒
>如果你把 Redux 範例複製到它們的目錄外面，你可以刪除它們的 `webpack.config.js` 尾端的幾行程式碼。 它們在 「You can safely delete these lines in your project.」 註解的後面。

## Counter

執行 [Counter](https://github.com/rackt/redux/tree/master/examples/counter) 範例：

```
git clone https://github.com/rackt/redux.git

cd redux/examples/counter
npm install
npm start

open http://localhost:3000/
```

它涵蓋了：

* 基礎的 Redux 資料流；
* 測試。

## TodoMVC

執行 [TodoMVC](https://github.com/rackt/redux/tree/master/examples/todomvc) 範例：

```
git clone https://github.com/rackt/redux.git

cd redux/examples/todomvc
npm install
npm start

open http://localhost:3000/
```

它涵蓋了：

* 伴隨著兩個 reducers 的 Redux 資料流；
* 更新巢狀資料；
* 測試。

## Async

執行 [Async](https://github.com/rackt/redux/tree/master/examples/async) 範例：

```
git clone https://github.com/rackt/redux.git

cd redux/examples/async
npm install
npm start

open http://localhost:3000/
```

它涵蓋了：

* 使用 [redux-thunk](https://github.com/gaearon/redux-thunk) 處理基礎的 async Redux 資料流；
* 快取回應並在抓取資料時顯示一個 spinner；
* 讓快取的資料失效。

## Real World

執行 [Real World](https://github.com/rackt/redux/tree/master/examples/real-world) 範例：

```
git clone https://github.com/rackt/redux.git

cd redux/examples/real-world
npm install
npm start

open http://localhost:3000/
```

它涵蓋了：

* 真實世界的 async Redux 資料流；
* 保存 entities 在一個正規化的 entity 快取裡；
* 給 API 呼叫用的自訂 middleware;
* 快取回應並在抓取資料時顯示一個 spinner；
* Pagination；
* Routing。

## 更多範例

你可以在  [Awesome Redux](https://github.com/xgrommx/awesome-redux) 找到更多範例。
