# Redux
A predictable state container. [[doc]](https://redux.js.org/introduction)
- [Translated Document](https://chentsulin.github.io/redux/index.html)
- `npm install redux`
- `npm install -D redux-devtools`  [[doc]](https://github.com/reduxjs/redux-devtools)
- Principles
    - Single source of truth (Store)
    - States are ready-only
    - Changes (by Actions) of state are made through pure functions (Reducer)

## Actions

- [Flux Standard Action](https://github.com/acdlite/flux-standard-action)
- Utils
	- `npm install redux-actions` [[doc]](https://github.com/redux-utilities/redux-actions)
		- [`createAction(s)`](https://redux-actions.js.org/api/createaction): Actions creating helper
		- [`handleAction(s)`](https://redux-actions.js.org/api/handleaction): Reducer creating helper
		- [`combineAction`](https://redux-actions.js.org/api/combineactions)
		- [`bindActionCreators`](https://redux.js.org/api/bindactioncreators): Binding dispatch to action creators.

## Middleware (by using [`applyMiddleware()`](https://redux.js.org/api/applymiddleware))

Middleware is basically an intercepter between `store.dispatch()` to actually dispatch the action. The signature is `({ getState, dispatch }) => next => action`, which should return a function calling `next(action)` to pass action to next middleware until the last one. For the last middleware, calling `next(action)` is to actually dispatch the action.


- Async actions solution
	- `npm install redux-thunk` [[doc]](https://github.com/reduxjs/redux-thunk)
	- `npm install redux-promise` [[doc]](https://github.com/redux-utilities/redux-promise) 
- Logging
	- `npm install redux-logger` [[doc]](https://github.com/LogRocket/redux-logger)
- Flattening store data
	- `npm install normalizr` [[doc]](https://github.com/paularmstrong/normalizr)
	- `npm install redux-normalizr-middleware` [[doc]](https://github.com/wbinnssmith/redux-normalizr-middleware)

## Reducer

- Why it's called Reducer: Similar to [`Array.prototype.reduce()`](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce).
	- `Array.prototype.reduce()` reduce array items into one
	- Reducer reduce a series of state into one
- It's a [pure function](https://jigsawye.gitbooks.io/mostly-adequate-guide/content/ch3.html)
- [Split reducer](https://redux.js.org/basics/reducers#splitting-reducers) into smaller ones if you need to handle partial state individually. And use [`combineReducers()`](https://redux.js.org/api/combinereducers) to combine reducers into one

## Store

- [`store.getState()`](https://redux.js.org/api/store#getState), [`store.dispatch(action)`](https://redux.js.org/api/store#dispatch), [`store.subscribe(listener)`](https://redux.js.org/api/store#subscribe)  
- Simplify [`shouldComponentUpdate()`](https://reactjs.org/docs/react-component.html#shouldcomponentupdate) logic
	- `npm install immutable` [[doc]](https://facebook.github.io/immutable-js/)

### Memorization
- Simple solution to memorize data, based on argument values. (cache size: 1, only last result)
	- `npm install memoize-one` [[doc]](https://github.com/alexreardon/memoize-one)
- Memorize data based on returned value of input selectors (could be another selector), and that makes selector reusable, composable.  Usually used in `connect()` to memorize derived state or props. (cache size: 1, only last result)
	- `npm install reselect` [[doc]](https://github.com/reduxjs/reselect)
- A wrapper of reselect to enhance selector.
	- `npm install reselect` (dependency)
	- `npm install re-reselect` [[doc]](https://github.com/toomuchdesign/re-reselect)

## Usage With React

- `npm install react-redux` [[doc]](https://github.com/reduxjs/react-redux)
- Presentational Components: How things look (markup, styles)
- Container Components: How things work (data fetching, state updates)
    - [`connect()`](https://github.com/reduxjs/react-redux/blob/master/docs/api.md#connect): Connect store to components
