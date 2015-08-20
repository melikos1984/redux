# [Redux](http://rackt.github.io/redux)

Redux 是個給 JavaScript 應用程式所使用的可預測 state 容器。

他幫助你撰寫行為一致的應用程式，可以在不同的環境下執行 (客戶端、伺服器、原生應用程式)，並且易於測試。在這之上，它提供一個很棒的開發體驗，例如 [live code editing combined with a time traveling debugger](https://github.com/gaearon/redux-devtools)。

你可以使用 Redux 結合 [React](https://facebook.github.io/react/)，或結合其他任何的 view library。
它非常小 (2kB) 並且沒有任何依賴套件。

[![build status](https://img.shields.io/travis/rackt/redux/master.svg?style=flat-square)](https://travis-ci.org/rackt/redux)
[![npm version](https://img.shields.io/npm/v/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![npm downloads](https://img.shields.io/npm/dm/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![redux channel on slack](https://img.shields.io/badge/slack-redux@reactiflux-61DAFB.svg?style=flat-square)](http://www.reactiflux.com)


### 推薦

>[「我愛那些你在 Redux 做的東西」](https://twitter.com/jingc/status/616608251463909376)
>Jing Chen，Flux 作者

>[「我在 FB 的內部 JS 討論群組尋求對 Redux 的評論，並獲得了普遍的好評。真的做得非常棒。」](https://twitter.com/fisherwebdev/status/616286955693682688)
>Bill Fisher，Flux 作者

>[「這很酷，你藉由完全不做 Flux 來發明了一個更好的 Flux。」](https://twitter.com/andrestaltz/status/616271392930201604)
>André Staltz，Cycle 作者

### 開發經驗

我在準備我的 React Europe 演講 [“Hot Reloading with Time Travel”](https://www.youtube.com/watch?v=xsSnOQynTHs) 的時候撰寫了 Redux。我那時的目標是建立一個 state 管理 library，它只有最少的 API，但卻擁有完全可預測的行為，所以它可以實現 logging、hot reloading、time travel、universal apps、record 和 replay，而不需要開發者任何其他的代價。

### 受到的影響

Redux 從 [Flux](https://facebook.github.io/flux) 的概念發展而來，不過藉由從 [Elm](http://elm-lang.org/guide/architecture) 獲取線索來避免它的複雜度。
不管你以前有沒有用過它們，只需要花幾分鐘就能入門 Redux。

### 安裝

安裝穩定版本：

```
npm install --save redux
```

大多數情況，你也會需要 [React 的綁定](http://github.com/gaearon/react-redux)和[開發者工具](http://github.com/gaearon/redux-devtools)。

```
npm install --save react-redux
npm install --save-dev redux-devtools
```

### 程式碼片段

The whole state of your app is stored in an object tree inside a single *store*.
The only way to change the state tree is to emit an *action*, an object describing what happened.
To specify how the actions transform the state tree, you write pure *reducers*.

就這樣！

```js
import { createStore } from 'redux';

/**
 * 這是一個 reducer，a pure function with (state, action) => state signature.
 * It describes how an action transforms the state into the next state.
 *
 * The shape of the state is up to you: it can be a primitive, an array, an object,
 * or even an Immutable.js data structure. The only important part is that you should
 * not mutate the state object, but return a new object if the state changes.
 *
 * In this example, we use a `switch` statement and strings, but you can use a helper that
 * follows a different convention (such as function maps) if it makes sense for your project.
 */
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}

// 建立一個 Redux store 來掌管你的應用程式的 state。
// 它的 API 是 { subscribe, dispatch, getState }。
let store = createStore(counter);

// You can subscribe to the updates manually, or use bindings to your view layer.
store.subscribe(() =>
  console.log(store.getState())
);

// The only way to mutate the internal state is to dispatch an action.
// The actions can be serialized, logged or stored and later replayed.
store.dispatch({ type: 'INCREMENT' });
// 1
store.dispatch({ type: 'INCREMENT' });
// 2
store.dispatch({ type: 'DECREMENT' });
// 1
```

Instead of mutating the state directly, you specify the mutations you want to happen with plain objects called *actions*. Then you write a special function called a *reducer* to decide how every action transforms the entire application’s state.

If you’re coming from Flux, there is a single important difference you need to understand. Redux doesn’t have a Dispatcher or support many stores. Instead, there is just a single store with a single root reducing function. As your app grows, instead of adding stores, you split the root reducer into smaller reducers independently operating on the different parts of the state tree. This is exactly like there is just one root component in a React app, but it is composed out of many small components.

This architecture might seem like an overkill for a counter app, but the beauty of this pattern is how well it scales to large and complex apps. It also enables very powerful developer tools, because it is possible to trace every mutation to the action that caused it. You can record user sessions and reproduce them just by replaying every action.

### 文件

* [介紹](http://rackt.github.io/redux/docs/introduction/index.html)
* [基礎](http://rackt.github.io/redux/docs/basics/index.html)
* [進階](http://rackt.github.io/redux/docs/advanced/index.html)
* [Recipes](http://rackt.github.io/redux/docs/recipes/index.html)
* [疑難排解](http://rackt.github.io/redux/docs/Troubleshooting.html)
* [術語表](http://rackt.github.io/redux/docs/Glossary.html)
* [API 參考](http://rackt.github.io/redux/docs/api/index.html)

### 範例

* [Counter](http://rackt.github.io/redux/docs/introduction/Examples.html#counter) ([source](https://github.com/rackt/redux/tree/master/examples/counter))
* [TodoMVC](http://rackt.github.io/redux/docs/introduction/Examples.html#todomvc) ([source](https://github.com/rackt/redux/tree/master/examples/todomvc))
* [Async](http://rackt.github.io/redux/docs/introduction/Examples.html#async) ([source](https://github.com/rackt/redux/tree/master/examples/async))
* [Real World](http://rackt.github.io/redux/docs/introduction/Examples.html#real-world) ([source](https://github.com/rackt/redux/tree/master/examples/real-world))

如果你不熟悉 NPM 生態系並在讓 project 運作起來時遇到了困難，或是你不確定要在哪裡貼上上面的程式碼片段，請查看 [simplest-redux-example](https://github.com/jackielii/simplest-redux-example)，它把 Redux 和 React、Browserify 結合在一起。

### 討論

加入 [Reactiflux](http://reactiflux.com) Slack 社群的 **#redux** 頻道。

### 致謝

* [Elm 架構](https://github.com/evancz/elm-architecture-tutorial) for a great intro to modeling state updates with reducers;
* [Turning the database inside-out](http://blog.confluent.io/2015/03/04/turning-the-database-inside-out-with-apache-samza/) for blowing my mind;
* [Developing ClojureScript with Figwheel](http://www.youtube.com/watch?v=j-kj2qwJa_E) for convincing me that re-evaluation should “just work”;
* [Webpack](https://github.com/webpack/docs/wiki/hot-module-replacement-with-webpack) for Hot Module Replacement;
* [Flummox](https://github.com/acdlite/flummox) for teaching me to approach Flux without boilerplate or singletons;
* [disto](https://github.com/threepointone/disto) for a proof of concept of hot reloadable Stores;
* [NuclearJS](https://github.com/optimizely/nuclear-js) for proving this architecture can be performant;
* [Om](https://github.com/omcljs/om) for popularizing the idea of a single state atom;
* [Cycle](https://github.com/staltz/cycle) for showing how often a function is the best tool;
* [React](https://github.com/facebook/react) for the pragmatic innovation.

特別感謝 [Jamie Paton](http://jdpaton.github.io) 它移交了 `redux` NPM 套件名稱給我們。

### 贊助者

在 Redux 的工作是[由社群出資](https://www.patreon.com/reactdx)。
遇到一些卓越的公司使這可以成真：

* [Webflow](http://webflow.com/)
* [Chess iX](http://www.chess-ix.com/)

[查看完整的 Redux 贊助者清單。](PATRONS.md)

### License

MIT
