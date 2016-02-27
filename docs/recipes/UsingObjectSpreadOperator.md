# 使用 Object Spread 運算子

因為 Redux 最核心的原則之一就是永不更動 state，你將會常常發現自己使用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 來建立擁有新值或更新其中值的複製 object。例如，在 `todoApp` 內， `Object.assign()` 慣於用來回傳一個新的且有更新 `visibilityFilter` 屬性後的 `state` object：

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```

然而大力使用 `Object.assign()` 會很快地導致簡單的 reducers 因為其相當冗長的語法而難以閱讀。

一個替代方法就是使用為了下一代 JavaScript 提出的 [object spread 語法](https://github.com/sebmarkbage/ecmascript-rest-spread)，這將讓你使用 spread (`...`) 運算子來更有效地從一個 object 複製 enumerable 屬性到另一個。此 object spread 運算子概念上相似於 ES6 的 [array spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)。我們可以經由使用 object spread 運算子來簡化 `todoApp` 範例：

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return { ...state, visibilityFilter: action.filter }
    default:
      return state
  }
}
```

當你正在構成複雜的 objects時，使用 object spread 語法的好處會變的越來越明顯。下面 `getAddedIds` 用 `getProduct` 和 `getQuantity` 的回傳值 map 一個 `id` array 到另一個 object array。

```js
return getAddedIds(state.cart).map(id => Object.assign(
  {},
  getProduct(state.products, id),
  {
    quantity: getQuantity(state.cart, id)
  }
))
```

Object spread 讓我們簡化以上的 `map` 呼叫，成為：

```js
return getAddedIds(state.cart).map(id => ({
  ...getProduct(state.products, id),
  quantity: getQuantity(state.cart, id)
}))
```

因為 object spread 語法仍然是 ECMAScript 的 Stage 2 提案，你將需要使用像是 [Babel](http://babeljs.io/) 的 transpiler 使 production 環境下可以使用它。你可以使用已存在的 `es2015` preset，安裝 [`babel-plugin-transform-object-rest-spread`](http://babeljs.io/docs/plugins/transform-object-rest-spread/) 並逐一將它加入在你 `.babelrc` 中的 `plugins` array。

```js
{
  "presets": ["es2015"],
  "plugins": ["transform-object-rest-spread"]
}
```

注意這仍然個實驗性質的功能提案語法，所以它可能在未來會改變。儘管如此，一些大專案像是 [React Native](https://github.com/facebook/react-native) 已經廣泛使用。所以可以安全的說，如果未來它真的改變了也將會有一個好的自動 migration 方式。
