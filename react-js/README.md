# React.js

## React (view component) [[doc]](https://reactjs.org/docs/getting-started.html)
- `npm install react react-dom`

### Context [[doc]](https://reactjs.org/docs/context.html)
- Provide a way to access data cross components instead of pass them down manually at every level.
- <s>Using redux instead... </s>



### <a name="react-refs"></a>Refs [[doc]](https://reactjs.org/docs/refs-and-the-dom.html)
Provide a way to access DOM nodes or React elements in the render method.

#### When to use
- Managing focus, text selection, or media playback.
- Integrating with third-party DOM libraries. (Usually because they're imperative)
- Avoid using refs for anything that can be done declaratively. (React is a declarative view library)

#### Accessing Refs
The reference of `ref.current` is based on what type of element the `ref` attribute is used on.

- HTML element: Reference to the underlying DOM element
- Class Component: Reference to the class instance.
- __Function Component: Can't use `ref` because they don't have instances.__

#### Passing a Ref through a component to its children [[doc]](https://reactjs.org/docs/forwarding-refs.html)

#### Callback Refs
Pass a function to `ref`, the function will receive reference as its argument. So you can store and access it elsewhere.

- React will call the `ref` callback with the DOM element when the component mounts, and call it with null when it unmounts. 
- If `ref` callback is an inline function, it will get called twice during updates. [(explaination)](https://github.com/facebook/react/issues/9328)



### react-router [[doc]](https://reacttraining.com/react-router/core/guides/philosophy)
- `npm install react-router react-router-dom`
- Since v4 it's not static routing anymore. It's dynamically composed with React Components.
- [Work with redux](https://reacttraining.com/react-router/web/guides/redux-integration)
- To keep redux and react-router in sync
	- (deprecated) `npm install react-router-redux` [[doc]](https://github.com/reactjs/react-router-redux)
	- `npm install connected-react-router` [[doc]](https://github.com/supasate/connected-react-router)
- Session history management (for browser it wraps history API)
	- `npm install history` [[doc]](https://github.com/ReactTraining/history)
	- It's already a dependency of react-router



### i18n
- `npm install i18next react-i18next` [[doc]](https://react.i18next.com/)



### Higher Order Component (HOC) [[doc]](https://reactjs.org/docs/higher-order-components.html)
Basically it's a wrapper of another component.

#### Convention & Caveats
- Don't mutate the original component. Use composition.
- Pass unrelated props (that HOC doesn't use) through to the wrapped component. (To keep the interface of both HOC and wrapped component the same)
- Maximizing Composability.
- Wrap the display name for debugging.
- Don't use HOCs inside the render method. (It'll return new instance every render)
- Static methods of component must be copied over. (There's no inheritance by wrapping)
- `Refs` aren't passed through (It's not a prop, like `key`). Use [React.forwardRef](https://reactjs.org/docs/forwarding-refs.html) API to solve this.



### Redner Props [[doc]](https://reactjs.org/docs/render-props.html)
A technique for sharing code between React components using a prop whose value is a function. Its concept is kinda like IoC (Inverse of Control), because it lets component to determine how to render by given render prop.

#### Convention & Caveats
- You don't have to use `render` as prop name.
- Be careful when using render props with `React.PureComponent`.
	- It will return a new function at every render if you write the following code:

	```js
	<Component render={data => (
		<AnotherComponent data={data} />
	)}/>
	```



### Error Boundaries [[doc]](https://reactjs.org/docs/error-boundaries.html)
Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed.

Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them. But that do not catch errors for:

- Event handlers
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
- Server side rendering
- Errors thrown in the error boundary itself (rather than its children)

Currently, to implement error boundaries can only be achieved by class component which defines either (or both) of the lifecycle methods [`static getDerivedStateFromError()`](https://reactjs.org/docs/react-component.html#static-getderivedstatefromerror) or [`componentDidCatch()`](https://reactjs.org/docs/react-component.html#componentdidcatch).



### Component Lifecycle (After 16.3)
[Lifeycycle methods interactive diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
![Lifecycle methods](../images/life-cycle-methods.jpeg)

- Render phase: React DOM calculates the changes that needs to be committed to DOM.
- Commit phase: React DOM actually commits those changes. 

One of the major difference between render phase and commit phase is that render phase can be called multiple times, but commit phase is called only once for a change.



### JSX [[doc]](https://reactjs.org/docs/jsx-in-depth.html)
#### Convention & Caveats
- `React` and custom components must be in scope, since JSX compiles into calls to `	React.createElement`.
- User-defined components must be capitalized, or it will be considered as html tag.
- Props default to "true".

```js
<Component customProp />
// These two are equivalent
<Component customProp={true} />
```
- Spread attributes

```js
function App1() {
	return <Greeting firstName="Ben" lastName="Hector" />;
}
// These two components are equivalent
function App2() {
	const props = {firstName: 'Ben', lastName: 'Hector'};
	return <Greeting {...props} />;
}
```

- Content between component tags will be passed as a special prop: `prop.children`.
- Normally, JavaScript expressions inserted in JSX will evaluate to a string, a React element, or a list of those things.  However, `props.children` works just like any other prop in that it can pass any sort of data (ex: functions).

```js
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
	let items = [];
	for (let i = 0; i < props.numTimes; i++) {
		items.push(props.children(i));
	}
	return <div>{items}</div>;
}

function ListOfTenThings() {
	return (
		<Repeat numTimes={10}>
			{(index) => <div key={index}>This is item {index} in the list</div>}
		</Repeat>
	);
}
```

- Booleans, `null`, and `undefined` are ignored. But some false values, such as `0` (number), are still rendered by React.



### Reconciliation [[doc]](https://reactjs.org/docs/reconciliation.html)
The process of how react update the tree of react elements.

#### The Diffing Algorithm
- __Elements of different types__: Rebuild the whole tree, old state will be lost.
- __DOM elements of the same type__: Update attributes only, and recurse on children.
- __Component elements of the same type__: The instance stays the same, and update props and state if needed. Then, call render() and recurse the diff algorithm on returning results between render().

#### Recursing On Children
By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there’s a difference.

```html
<ul>
	<li>Duke</li>
	<li>Villanova</li>
</ul>

/* Every child will be render because they don't match */
<ul>
	<li>Connecticut</li>
	<li>Duke</li>
	<li>Villanova</li>
</ul>
```
 Solution to this is to add `key` attribute.

```html
<ul>
	<li key="2015">Duke</li>
	<li key="2016">Villanova</li>
</ul>

<ul>
	<li key="2014">Connecticut</li>
	<li key="2015">Duke</li>
	<li key="2016">Villanova</li>
</ul>
```

- Keys only need to be unique among sibling elements, not globally unique.
- Keys should be a stable identity across re-renders. For example, hash some parts of data to get a key.



### Controlled vs Uncontrolled Component
#### Controlled
A controlled componen's state is maintained outside the component. (usually in the parent component, ex: `form`) And this also means the states inside /outside the component are in sync.

```js
const Form = () => {
	const [name, setName] = React.useState('');
	return (
		<div>
			{/* State gives value to the input*/}
			<input type="text" value={name}
				{/* And the input inform state of the changes */}
				onChange={e => setName(e.target.value)} />
		</div>
	);
};
```

#### Uncontrolled
On the contrary, an uncontrolled component's state is maintained inside the component. We only access the state while we need it through [`ref`](#react-refs) (or [`React.forwardRef()`](https://reactjs.org/docs/forwarding-refs.html)).

And because we don't have to sync up states, it's easier (lesser code) to integrate with non-React.js code.

##### Caveats
- the `value` attributes will override the value in DOM, if we want an initial value and keep it uncontrolled, use `defaultValue` instead. (or `defaultChecked` for `<input type="radio">` and `<input type="checkbox">`)
- `<input type="file">` is always uncontrolled due to its value can't be set programmatically, it can only be set by user.

#### Which to Use
Controlled comopnent gives you more flexibility to access state (and also require more code), so you can achive something like:
- instant validation
- dynamic input (change values depends on the others)
- enforcing input format
- ...

But if you don't need features like above, using uncontrolled components is just fine. And you can always migrate them to controlled components.


---

## Flux (design pattern / architecture) [[doc]](https://github.com/facebook/flux)
 
- `npm install flux`
- [Concept: Unidirectional data flow](https://github.com/facebook/flux/tree/master/examples/flux-concepts) 
   + (Actions) > Dispatcher > Store > View > (Actions) > ...
          ![Flow](../images/flow.png)
- View implementation > React
- Store + Dispatcher alternative > Redux

---

## React Server Side Rendering

-  [Isomorphic Javascript to React SSR](https://blog.techbridge.cc/2016/08/27/react-redux-immutablejs-node-server-isomorphic-tutorial/)

---

## PWA Related

- [Workbox (service worker cache library)](https://developers.google.com/web/tools/workbox/)