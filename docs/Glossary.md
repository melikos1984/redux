# 術語表

這是 Redux 中核心詞彙的術語表，以及它們的 type signatures。這些 types 是使用 [Flow notation](http://flowtype.org/docs/quick-reference.html) 來記錄。

## State

```js
type State = any;
```

*State* (也稱作 *state tree*) 是個廣義的詞彙，不過在 Redux API 中，它通常是指被 store 管理的單一狀態值並藉由 [`getState()`](api/Store.md#getState) 回傳。它代表 Redux 應用程式的完整狀態，通常是個多層的巢狀 object。

習慣上，top-level state 是個 object 或一些其他 key-value collection 像是 Map，but technically it can be any type。不過，你應該盡你所能讓 state serializable。不要放任何你不能輕易轉成 JSON 的東西在裡面。

## Action

```js
type Action = Object;
```

An *action* is a plain object that represents an intention to change the state. Actions are the only way to get data into the store. Any data, whether from UI events, network callbacks, or other sources such as WebSockets needs to eventually be dispatched as actions.

習慣上，actions should have a `type` field that indicates the type of action being performed。 Types can be defined as constants and imported from another module。 It’s better to use strings for `type` than [Symbols](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol) because strings are serializable。

Other than `type`, the structure of an action object is really up to you。 If you’re interested, check out [Flux Standard Action](https://github.com/acdlite/flux-standard-action) for recommendations on how actions should be constructed。

See also [async action](#async-action) below。

## Reducer

```js
type Reducer<S, A> = (state: S, action: A) => S;
```

A *reducer* (也稱作 *reducing function*) is a function that accepts an accumulation and a value and returns a new accumulation. They are used to reduce a collection of values down to a single value.

Reducers are not unique to Redux—they are a fundamental concept in functional programming.  Even most non-functional languages, like JavaScript, have a built-in API for reducing. In JavaScript, it's [`Array.prototype.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce).

In Redux, the accumulated value is the state object, and the values being accumulated are actions. Reducers calculate a new state given the previous state and an action. They must be *pure functions*—functions that return the exact same output for given inputs. They should also be free of side-effects. This is what enables exciting features like hot reloading and time travel.

Reducers 是 Redux 中最重要的概念。

*不要在 reducers 裡面呼叫 API*

## Dispatching Function

```js
type BaseDispatch = (a: Action) => Action;
type Dispatch = (a: Action | AsyncAction) => any;
```

A *dispatching function* (or simply *dispatch function*) is a function that accepts an action or an [async action](#async-action); it then may or may not dispatch one or more actions to the store.

We must distinguish between dispatching functions in general and the base [`dispatch`](api/Store.md#dispatch) function provided by the store instance without any middleware.

The base dispatch function *always* synchronously sends an action to the store’s reducer, along with the previous state returned by the store, to calculate a new state. It expects actions to be plain objects ready to be consumed by the reducer.

[Middleware](#middleware) wraps the base dispatch function. It allows the dispatch function to handle [async actions](#async-action) in addition to actions. Middleware may transform, delay, ignore, or otherwise interpret actions or async actions before passing them to the next middleware. See below for more information.

## Action Creator

```js
type ActionCreator = (...args: any) => Action | AsyncAction;
```

An *action creator* is, quite simply, a function that creates an action. Do not confuse the two terms—again, an action is a payload of information, and an action creator is a factory that creates an action.

Calling an action creator only produces an action, but does not dispatch it. You need to call the store’s [`dispatch`](api/Store.md#dispatch) function to actually cause the mutation. Sometimes we say *bound action creators* to mean functions that call an action creator and immediately dispatch its result to a specific store instance.

If an action creator needs to read the current state, perform an API call, or cause a side effect, like a routing transition, it should return an [async action](#async-action) instead of an action.

## Async Action

```js
type AsyncAction = any;
```

An *async action* is a value that is sent to a dispatching function, but is not yet ready for consumption by the reducer. It will be transformed by [middleware](#middleware) into an action (or a series of actions) before being sent to the base [`dispatch()`](api/Store.md#dispatch) function. Async actions may have different types, depending on the middleware you use. They are often asynchronous primitives, like a Promise or a thunk, which are not passed to the reducer immediately, but trigger action dispatches once an operation has completed.

## Middleware

```js
type MiddlewareAPI = { dispatch: Dispatch, getState: () => State };
type Middleware = (api: MiddlewareAPI) => (next: Dispatch) => Dispatch;
```

A middleware is a higher-order function that composes a [dispatch function](#dispatching-function) to return a new dispatch function. It often turns [async actions](#async-action) into actions.

Middleware is composable using function composition. It is useful for logging actions, performing side effects like routing, or turning an asynchronous API call into a series of synchronous actions.

See [`applyMiddleware(...middlewares)`](./api/applyMiddleware.md) for a detailed look at middleware.

## Store

```js
type Store = {
  dispatch: Dispatch;
  getState: () => State;
  subscribe: (listener: () => void) => () => void;
  getReducer: () => Reducer;
  replaceReducer: (reducer: Reducer) => void;
};
```

A store is an object that holds the application’s state tree.
There should only be a single store in a Redux app, as the composition happens on the reducer level.

- [`dispatch(action)`](api/Store.md#dispatch) is the base dispatch function described above.
- [`getState()`](api/Store.md#getState) returns the current state of the store.
- [`subscribe(listener)`](api/Store.md#subscribe) registers a function to be called on state changes.
- [`getReducer()`](api/Store.md#getReducer) and [`replaceReducer(nextReducer)`](api/Store.md#replaceReducer) can be used to implement hot reloading and code splitting. Most likely you won’t use them.

See the complete [store API reference](api/Store.md#dispatch) for more details.

## Store creator

```js
type StoreCreator = (reducer: Reducer, initialState: ?State) => Store;
```

A store creator is a function that creates a Redux store. Like with dispatching function, we must distinguish the base store creator, [`createStore(reducer, initialState)`](api/createStore.md) exported from the Redux package, from store creators that are returned from the store enhancers.

## Store enhancer

```js
type StoreEnhancer = (next: StoreCreator) => StoreCreator;
```

A store enhancer is a higher-order function that composes a store creator to return a new, enhanced store creator. This is similar to middleware in that it allows you to alter the store interface in a composable way.

Store enhancers are much the same concept as higher-order components in React, which are also occasionally called “component enhancers”.

Because a store is not an instance, but rather a plain-object collection of functions, copies can be easily created and modified without mutating the original store. There is an example in [`compose`](api/compose.md) documentation demonstrating that.

Most likely you’ll never write a store enhancer, but you may use the one provided by the [developer tools](https://github.com/gaearon/redux-devtools). It is what makes time travel possible without the app being aware it is happening. Amusingly, the [Redux middleware implementation](api/applyMiddleware.md) is itself a store enhancer.
