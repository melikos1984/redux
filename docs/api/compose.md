# `compose(...functions)`

把 function 從右到左組合起來。

這是一個 functional programming 的 utility，並為了方便而直接被放在 Redux 裡。
你可能會想要使用它來在一行中使用幾個 [store enhancer](../Glossary.md#store-enhancer)。

#### 參數

1. (*arguments*)：要組合的 function。每個 function 都預期會接收一個參數。它的回傳值將會作為在它左邊的 function 的變數，以此類推。有個例外是當為最後被組合的 function 提供 signature 時， 最右邊的變數可接受多個參數。

#### 回傳

(*Function*)：藉由從右到左組合給定的 function 而獲得的最終 function。

#### 範例

這個範例展示了要如何使用 `compose` 藉由 [`applyMiddleware`](applyMiddleware.md) 與幾個來自 [redux-devtools](https://github.com/gaearon/redux-devtools) 套件的開發工具來增強一個 [store](Store.md)。

```js
import { createStore, combineReducers, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import DevTools from './containers/DevTools'
import reducer from '../reducers/index'

const store = createStore(
  reducer,
  compose(
    applyMiddleware(thunk),
    DevTools.instrument()
  )
)
```

#### 提示

* `compose` 做的只是讓你不需要把程式碼往右縮進，就能撰寫深度巢狀的 function 轉換。不要把它想得太複雜！
