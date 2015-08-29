# Reducers

[Actions](./Actions.md) 描述實際*發生一些事*，不過並不指定應用程式的 state 要如何去應對改變。這是 reducer 的工作。

## 設計 State 形狀

在 Redux 中，所有的應用程式 state 被儲存為一個單一物件。在撰寫任何程式碼之前先思考它的形狀是個好主意。如何把你的應用程式的 state 描述成一個最簡單形式的物件？

以我們的 todo 應用程式來說，我們想要儲存兩個不同的東西：

* 現在被選擇的可見度過濾條件；
* 實際的 todos 清單。

你常常會發現需要儲存一些資料，以及一些 UI 狀態在 state tree 中。這沒問題，不過僅盡量讓資料和 UI 狀態分離。

```js
{
  visibilityFilter: 'SHOW_ALL',
  todos: [{
    text: 'Consider using Redux',
    completed: true,
  }, {
    text: 'Keep all state in a single tree',
    completed: false
  }]
}
```

>##### 關於 Relationships 的附註

>在更複雜的應用程式中，你會希望不同的實體可以呼香參考到其他實體。我們建議你盡量保持 state 正規化，不要有任何的巢狀。讓儲存在物件的每個實體都用一個 ID 作為 key，並且使用 IDs 來從其他實體或清單參考它。把應用程式的 state 想成是一個資料庫。這個方法詳細的描述在 [normalizr 的](https://github.com/gaearon/normalizr) 文件中。例如，在一個真實的應用程式中，同時把 `todosById: { id -> todo }` 和 `todos: array<id>` 保存在 state 裡面會是比較好的方式，不過我們會盡量讓範例簡單。

## 處理 Actions

現在我們已經決定 state 物件要長什麼樣子，我們已經準備好幫它撰寫一個 reducer。reducer 是一個 pure function，它接收先前的 state and 和一個 action，然後回傳下一個 state。

```js
(previousState, action) => newState
```

It’s called a reducer because it’s the type of function you would pass to [`Array.prototype.reduce(reducer, ?initialValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce). It’s very important that the reducer stays pure. Things you should **never** do inside a reducer:

* 改變它的參數；
* Perform side effects like API calls and routing transitions.

我們將會探索 how to perform side effects in the [advanced walkthrough](../advanced/README.md). For now, just remember that the reducer must be pure. **Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API calls. No mutations. Just a calculation.**

With this out of the way, let’s start writing our reducer by gradually teaching it to understand the [actions](Actions.md) we defined earlier.

We’ll start by specifying the initial state. Redux will call our reducer with an `undefined` state for the first time. This is our chance to return the initial state of our app:

```js
import { VisibilityFilters } from './actions';

const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
};

function todoApp(state, action) {
  if (typeof state === 'undefined') {
    return initialState;
  }

  // 在現在，不要處理俵處理任何 actions
  // 而只是回傳給我們的 state。
  return state;
}
```

One neat trick is to use the [ES6 default arguments syntax](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/default_parameters) to write this in a more compact way:

```js
function todoApp(state = initialState, action) {
  // For now, don’t handle any actions
  // and just return the state given to us.
  return state;
}
```

現在讓我們來處理 `SET_VISIBILITY_FILTER`。All it needs to do is to change `visibilityFilter` on the state。簡單的：

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return Object.assign({}, state, {
      visibilityFilter: action.filter
    });
  default:
    return state;
  }
}
```

注意這幾點：

1. **我們不改變 `state`.** 我們用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 複製一份。`Object.assign(state, { visibilityFilter: action.filter })` 也是錯的：它會改變第一個參數。你**必須**提供一個空物件作為第一個參數。你也可以啟用實驗性的 ES7 [object spread syntax](https://github.com/sebmarkbage/ecmascript-rest-spread) 提案，就可以寫 `{ ...state, ...newState }` 作為取代。

2. **我們在 `default` case 回傳先前的 `state`。**針對任何未知的 action 回傳先前的 `state` 非常重要。

>##### 關於 `Object.assign` 的附註

>[`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 是 ES6 的一部分，不過大部份瀏覽器尚未實作。你會需要使用一個 polyfill，[Babel plugin](https://github.com/babel-plugins/babel-plugin-object-assign) 或是從其他 library 來的 helper 像是 [`_.assign()`](https://lodash.com/docs#assign)。

>##### 關於 `switch` 和 Boilerplate 的附註

>The `switch` statement is *not* the real boilerplate. The real boilerplate of Flux is conceptual: the need to emit an update, the need to register the Store with a Dispatcher, the need for the Store to be an object (and the complications that arise when you want a universal app). Redux solves these problems by using pure reducers instead of event emitters.

>It’s unfortunate that many still choose a framework based on whether it uses `switch` statements in the documentation. If you don’t like `switch`, you can use a custom `createReducer` function that accepts a handler map, as shown in [「減少 boilerplate」](../recipes/ReducingBoilerplate.md#reducers).

## 處理更多 Actions

我們有兩個以上的 actions 要處理！讓我們來擴充 reducer 以處理 `ADD_TODO`。

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return Object.assign({}, state, {
      visibilityFilter: action.filter
    });
  case ADD_TODO:
    return Object.assign({}, state, {
      todos: [...state.todos, {
        text: action.text,
        completed: false
      }]
    });
  default:
    return state;
  }
}
```

就像之前一樣，we never write directly to `state` or its fields, and instead we return new objects. The new `todos` is equal to the old `todos` concatenated with a single new item at the end. The fresh todo was constructed using the data from the action.

Finally, the implementation of the `COMPLETE_TODO` handler shouldn’t come as a complete surprise:

```js
case COMPLETE_TODO:
  return Object.assign({}, state, {
    todos: [
      ...state.todos.slice(0, action.index),
      Object.assign({}, state.todos[action.index], {
        completed: true
      }),
      ...state.todos.slice(action.index + 1)
    ]
  });
```

Because we want to update a specific item in the array without resorting to mutations, we have to slice it before and after the item. If you find yourself often writing such operations, it’s a good idea to use a helper like [React.addons.update](https://facebook.github.io/react/docs/update.html), [updeep](https://github.com/substantial/updeep), or even a library like [Immutable](http://facebook.github.io/immutable-js/) that has native support for deep updates. Just remember to never assign to anything inside the `state` unless you clone it first.

## 拆分 Reducers

這是我們到目前的程式碼。這相當的詳細：

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return Object.assign({}, state, {
      visibilityFilter: action.filter
    });
  case ADD_TODO:
    return Object.assign({}, state, {
      todos: [...state.todos, {
        text: action.text,
        completed: false
      }]
    });
  case COMPLETE_TODO:
    return Object.assign({}, state, {
      todos: [
        ...state.todos.slice(0, action.index),
        Object.assign({}, state.todos[action.index], {
          completed: true
        }),
        ...state.todos.slice(action.index + 1)
      ]
    });
  default:
    return state;
  }
}
```

有方法讓它更容易理解嗎？It seems like `todos` and `visibilityFilter` are updated completely independently. Sometimes state fields depend on one another and more consideration is required, but in our case we can easily split updating `todos` into a separate function:

```js
function todos(state = [], action) {
  switch (action.type) {
  case ADD_TODO:
    return [...state, {
      text: action.text,
      completed: false
    }];
  case COMPLETE_TODO:
    return [
      ...state.slice(0, action.index),
      Object.assign({}, state[action.index], {
        completed: true
      }),
      ...state.slice(action.index + 1)
    ];
  default:
    return state;
  }
}

function todoApp(state = initialState, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return Object.assign({}, state, {
      visibilityFilter: action.filter
    });
  case ADD_TODO:
  case COMPLETE_TODO:
    return Object.assign({}, state, {
      todos: todos(state.todos, action)
    });
  default:
    return state;
  }
}
```

Note that `todos` also accepts `state`—but it’s an array! Now `todoApp` just gives it the slice of the state to manage, and `todos` knows how to update just that slice. **This is called *reducer composition*, and it’s the fundamental pattern of building Redux apps.**

Let’s explore reducer composition more. Can we also extract a reducer managing just `visibilityFilter`? We can:

```js
function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return action.filter;
  default:
    return state;
  }
}
```

Now we can rewrite the main reducer as a function that calls the reducers managing parts of the state, and combines them into a single object. It also doesn’t need to know the complete initial state anymore. It’s enough that the child reducers return their initial state when given `undefined` at first.

```js
function todos(state = [], action) {
  switch (action.type) {
  case ADD_TODO:
    return [...state, {
      text: action.text,
      completed: false
    }];
  case COMPLETE_TODO:
    return [
      ...state.slice(0, action.index),
      Object.assign({}, state[action.index], {
        completed: true
      }),
      ...state.slice(action.index + 1)
    ];
  default:
    return state;
  }
}

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return action.filter;
  default:
    return state;
  }
}

function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  };
}
```

**Note that each of these reducers is managing its own part of the global state. The `state` parameter is different for every reducer, and corresponds to the part of the state it manages.**

This is already looking good! When the app is larger, we can split the reducers into separate files and keep them completely independent and managing different data domains.

Finally, Redux provides a utility called [`combineReducers()`](../api/combineReducers.md) that does the same boilerplate logic that the `todoApp` above currently does. With its help, we can rewrite `todoApp` like this:

```js
import { combineReducers } from 'redux';

const todoApp = combineReducers({
  visibilityFilter,
  todos
});

export default todoApp;
```

Note that this is completely equivalent to:

```js
export default function todoApp(state, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  };
}
```

You could also give them different keys, or call functions differently. These two ways to write a combined reducer are completely equivalent:

```js
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
});
```

```js
function reducer(state, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  };
}
```

All [`combineReducers()`](../api/combineReducers.md) does is generate a function that calls your reducers **with the slices of state selected according to their keys**, and combining their results into a single object again. [它不是魔法。](https://github.com/rackt/redux/issues/428#issuecomment-129223274)

>##### 給理解 ES6 的使用者們的附註

>Because `combineReducers` expects an object, we can put all top-level reducers into a separate file, `export` each reducer function, and use `import * as reducers` to get them as an object with their names as the keys:

>```js
>import { combineReducers } from 'redux';
>import * as reducers from './reducers';
>
>const todoApp = combineReducers(reducers);
>```
>
>因為 `import *` 還是一個新的語法，我們今後不會在文件中使用它以避免[困惑](https://github.com/rackt/redux/issues/428#issuecomment-129223274)，不過你可能會在一些社群的範例中遇到它。

## 原始碼

#### `reducers.js`

```js
import { combineReducers } from 'redux';
import { ADD_TODO, COMPLETE_TODO, SET_VISIBILITY_FILTER, VisibilityFilters } from './actions';
const { SHOW_ALL } = VisibilityFilters;

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
  case SET_VISIBILITY_FILTER:
    return action.filter;
  default:
    return state;
  }
}

function todos(state = [], action) {
  switch (action.type) {
  case ADD_TODO:
    return [...state, {
      text: action.text,
      completed: false
    }];
  case COMPLETE_TODO:
    return [
      ...state.slice(0, action.index),
      Object.assign({}, state[action.index], {
        completed: true
      }),
      ...state.slice(action.index + 1)
    ];
  default:
    return state;
  }
}

const todoApp = combineReducers({
  visibilityFilter,
  todos
});

export default todoApp;
```

## 下一步

接下來，我們將探索如何[建立 Redux store](Store.md)，它會掌管 state 並在你 dispatch 一個 action 時幫忙呼叫 reducer。
