<p align="center"><img src="http://wx2.sinaimg.cn/mw690/0060lm7Tly1ftpm5b3ihfj3096097aaj.jpg" alt="fre logo" width="180"></p>
<h1 align="center">Fre</h1>
<p align="center">:ghost: Tiny React16 like library with Concurrent and Suspense.</p>
<p align="center">
<a href="https://circleci.com/gh/132yse/fre"><img src="https://img.shields.io/circleci/project/github/132yse/fre.svg?style=flat-square" alt="Build Status"></a>
<a href="https://codecov.io/gh/132yse/fre"><img src="https://img.shields.io/codecov/c/github/132yse/fre.svg?style=flat-square" alt="Code Coverage"></a>
<a href="https://npmjs.com/package/fre"><img src="https://img.shields.io/npm/v/fre.svg?style=flat-square" alt="npm-v"></a>
<a href="https://npmjs.com/package/fre"><img src="https://img.shields.io/npm/dt/fre.svg?style=flat-square" alt="npm-d"></a>
<a href="https://bundlephobia.com/result?p=fre"><img src="https://img.shields.io/bundlephobia/minzip/fre.svg?&style=flat-square" alt="gzip"></a>
<a href="https://github.com/prettier/prettier"><img src="https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square" alt="prettier"></a>
</p>

### Feature

- :tada: Functional Component and hooks API
- :confetti_ball: Concurrent and Suspense
- :telescope: keyed reconcilation algorithm

### Introduction

Fre (pronounced `/fri:/`, like free) is a tiny and perfect js library, It means [Free!](https://www.clicli.us/search/free) ~

#### Contributors

Fre has wonderful code, we need anyone to join us improve together.

<table><tbody><tr>
<td><a target="_blank" href="https://github.com/132yse"><img width="70px" src="https://avatars0.githubusercontent.com/u/12951461?s=70&v=4"></a></td>
<td><a target="_blank" href="https://github.com/mindplay-dk"><img width="70px" src="https://avatars3.githubusercontent.com/u/103348?s=70&v=4"></a></td>
<td><a target="_blank" href="https://github.com/hkc452"><img width="70px" src="https://avatars2.githubusercontent.com/u/3286658?s=70&v=4"></a></td>
<td><a target="_blank" href="https://github.com/wu-yu-xuan"><img width="70px" src="https://avatars3.githubusercontent.com/u/35450080?s=70&v=4"></a></td>
<td><a target="_blank" href="https://github.com/yiliang114"><img width="70px" src="https://avatars1.githubusercontent.com/u/11473889?s=70&v=4"></a></td>
</tr></table></tbody>

#### Users or sponsors

Thanks for the following websites and sponsors, If you do the same, please tell us with issue~

<table><tbody><tr>
<td><a target="_blank" href="https://ke.qq.com/course/368629?flowToken=1015240"><img height="60px" src="https://ws1.sinaimg.cn/large/0065Zy9ely1g983zobxqzj30ka03y0v6.jpg"></a></td>
<td><a target="_blank" href="https://www.clicli.me"><img height="60px" src="https://ws1.sinaimg.cn/large/0065Zy9ely1g983rcrcyuj30a305sgm2.jpg"></a></td>
</tr></table></tbody>

### Demo

- [async rendering demo](https://codesandbox.io/s/suspicious-rosalind-i06mv)

- [key-based reconcilation demo](https://codesandbox.io/s/fre-demo-d7vm7)

- [suspense demo](https://codesandbox.io/s/fre-suspense-demo-h5nfy)

Thanks for [Rasmus Schultz](https://github.com/mindplay-dk)

### Use

```shell
yarn add fre
```

```js
import { h, render, useState } from 'fre'

function App() {
  const [count, setCount] = useState(0)
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

### Hooks API

- [useState](https://github.com/132yse/fre#usestate)

- [useEffect](https://github.com/132yse/fre#useeffect)

- [useReducer](https://github.com/132yse/fre#usereducer)

- [useCallback](https://github.com/132yse/fre#usecallback)

- [useMemo](https://github.com/132yse/fre#usememo)

- [useRef](https://github.com/132yse/fre#useref)

#### useState

`useState` is a base API, It will receive initial state and return a Array

You can use it many times, new state is available when component is rerender

```js
function App() {
  const [up, setUp] = useState(0)
  const [down, setDown] = useState(0)
  return (
    <div>
      <h1>{up}</h1>
      <button onClick={() => setUp(up + 1)}>+</button>
      <h1>{down}</h1>
      <button onClick={() => setDown(down - 1)}>-</button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

#### useReducer

`useReducer` and `useState` are almost the same，but `useReducer` needs a global reducer

```js
function reducer(state, action) {
  switch (action.type) {
    case 'up':
      return { count: state.count + 1 }
    case 'down':
      return { count: state.count - 1 }
  }
}

function App() {
  const [state, dispatch] = useReducer(reducer, { count: 1 })
  return (
    <div>
      {state.count}
      <button onClick={() => dispatch({ type: 'up' })}>+</button>
      <button onClick={() => dispatch({ type: 'down' })}>+</button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

#### useEffect

`useEffect` takes two parameters, the first is a effect callback and the second is an array

if the array changed, the callback will execute after commitWork, such as `pureComponentDidUpdate`

if the array is empty, it means execute once, such as `componentDidMount`

if no array, it means execute every time , such as `componentDidUpdate`

if useEffect returns a function, the function will execute before next commitWork, such as `componentWillUnmount`

```js
function App({ flag }) {
  const [count, setCount] = useState(0)
  useEffect(() => {
    document.title = 'count is ' + count
  }, [flag])
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

#### useCallback

`useCallback` has the same parameters as `useEffect`, but `useCallback` will return a cached function.

```js
const set = new Set()

function App() {
  const [count, setCount] = useState(0)
  const cb = useCallback(() => {
    console.log('cb was cached')
  }, [count])
  set.add(cb)

  return (
    <div>
      <h1>{set.size}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  )
}
```

#### useMemo

`useMemo` has the same parameters as `useEffect`, but `useMemo` will return a cached value.

```js
function App() {
  const [count, setCount] = useState(0)
  const val = useMemo(() => {
    return new Date()
  }, [count])
  return (
    <div>
      <h1>
        {count} - {val}
      </h1>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

#### useRef

`useRef` will return a function or an object.

```js
function App() {
  useEffect(() => {
    console.log(t) // { current:<div>t</div> }
  })
  const t = useRef(null)
  return <div ref={t}>t</div>
}
```

If it use a function, It can return a cleanup and exectes when removed.

```js
function App() {
  const t = useRef(dom => {
    if (dom) {
      doSomething()
    } else {
      cleanUp()
    }
  })
  return flag && <span ref={t}>I will removed</span>
}
```

### Awesome API

There are some awesome APIs, It used outside of component, Usually a `with` prefix is used

#### withContext

Simplify context implement for hooks, no need Provider or useContext, share state easier.

```js
const useTheme = withContext('light')

function App() {
  const [theme, setTheme] = useTheme()
  return (
    <div>
      {theme}
      <A />
      <button onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}>
        change
      </button>
    </div>
  )
}

function A() {
  const [theme] = useTheme()
  return <div>{theme}</div>
}
```

#### withSuspense

One API to suspense not only fetch data but also dynamic component.

```js
const useUser = withSuspense(pageSize =>
  fetch(`https://api.clicli.us/users?level=4&page=1&pageSize=${pageSize}`)
    .then(res => res.json())
    .then(next => next.users)
)
const OtherComponent = withSuspense(() => import('./other-component'))

function App() {
  const users = useUser(pageSize)
  return <OtherComponent users={users} />
}
```

### render props / children

```js
const HelloBox = () => <Box render={value => <h1>{value}</h1>} />

const Box = props => <div>{props.render('hello world!')}</div>
```

```js
const HelloBox = () => (
  <Box>
    {value => {
      return <h1>{value}</h1>
    }}
  </Box>
)

const Box = props => <div>{props.children('hello world!')}</div>
```

#### Concurrent

Fre implements a tiny priority scheduler, which called [Concurrent Mode](https://reactjs.org/docs/concurrent-mode-intro.html).

It uses the `linked list` data struct to iterate a tree, which can better break, continue, and fallback.

At the same time, it uses double buffering to separate reading and writing.

Of course, the new data struct brings different algorithms and many possibilities.

#### time slicing

Time slicing is the scheduling of reconcilation, synchronous tasks, sacrifice CPU and reduce blocking time

#### Suspense

Suspense is the scheduling of promise, asynchronous tasks, break current tasks, and continue tasks after promise resolve

#### key-based reconcilation

Fre implements a compact reconcilation algorithm support keyed, which also called diff.

It uses hash to mark locations to reduce much size.

#### License

_MIT_ ©132yse inspired by [react](https://github.com/facebook/react) [anu](https://github.com/RubyLouvre/anu)
