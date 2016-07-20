# 分隔 Redux 子應用程式

試想一個嵌入較小的「子應用程式」(被裝在 `<SubApp>` component) 的「大型」應用程式 (被裝在一個 `<BigApp>` component) 案例：

```js
import React, { Component } from 'react'
import SubApp from './subapp'

class BigApp extends Component {
  render() {
    return (
      <div>
        <SubApp />
        <SubApp />
        <SubApp />
      </div>
    )
  }
}
```

這些 `<SubApp>` 會是完全地獨立的。它們不會共享資料或是 action，並不會看見或與彼此溝通。

最好不要把這個方式跟標準的 Redux reducer composition 混在一起。
針對典型的 web 應用程式，請繼續使用 reducer composition。針對
「product hub」、「dashboard」或是把不同的工具包進一個統一包裝的企業軟體，則可以試試子應用程式方式。

子應用程式方式也對以產品或功能劃分的大型團隊很有用。這些團隊可以獨立的發布子應用程式或是與一個附帶的 「應用程式 shell」做結合。

以下是一個子應用程式的 root connected component。
跟一般一樣，它可以 render 更多的 component 作為 children，不管有沒有被 connect 的都可以。
通常我們會就這樣在 `<Provider>` 中 render 它。

```js
class App extends Component { ... }
export default connect(mapStateToProps)(App)
```

不過，如果我們對隱藏這個子應用程式 component 是一個 Redux 應用程式的事實有興趣，我們不需要呼叫 `ReactDOM.render(<Provider><App /></Provider>)`。

或許我們想要能夠在同個「較大的」應用程式執行數個它的實體並讓它維持是一個完全的黑箱，把 Redux 變成實作細節。

要把 Redux 隱藏在 React API 的背後，我們可以把它包在一個特別的 component 中，並在 constructor 初始化 store：

```js
import React, { Component } from 'react'
import { Provider } from 'react-redux'
import reducer from './reducers'
import App from './App'

class SubApp extends Component {
  constructor(props) {
    super(props)
    this.store = createStore(reducer)
  }

  render() {
    return (
      <Provider store={this.store}>
        <App />
      </Provider>
    )
  }
}
```

用這個方法每個實體都會是獨立的。

這個模式*不*建議用在相同但是會共享資料的應用程式部件上。
不過，它在較大的應用程式完全不存取較小的應用程式內部時很有用，
而且我們想要把它們是用 Redux 實作的事實作為實作細節。
每個 component 實體會有它自己的 store，所以它們不會「了解」彼此。
