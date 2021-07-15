# Dispatch Resolve
## useDispatchResolve

useDispatchResolve is a custom hook helping to synchronously execute the statement after dispatching a redux side effect action when using saga or thunk middleware.
##### Usage
useDispatchResolve() return a dispatch function like useDispatch():
```js
const dispatchResolve = useDispatchResolve();
```
dispatchResolve(action) will return a promise:
```js
await dispatchResolve(action); // OR dispatchResolve(action).then(...) as well
// others statements
```
###### Importance Note
When using dispatchResolve, your action will be add a new property "resolver" that contains resolve and reject function of a new Promise (check source code to get to know more). Use that "resolver" to decide that whenever resolve the promise then execute next statement OR reject the promise.

Example:
```js
function* abcSaga(action) {
  const { resolver = {} } = action; // Be careful, resolver will be undefined if using useDispatch of react-redux
  try {
    const result = yield axios.get(...);
    typeof resolver.resolve === 'function' && resolver.resolve();
  } catch () {
    typeof resolver.reject === 'function' && resolver.reject();
  }
}
```