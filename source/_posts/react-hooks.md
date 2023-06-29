---
title: React Hooks
toc: true
date: 2023-03-16 16:45:17
tags:
---

Reactjs是一个用于构建用户界面的JavaScript库，它提供了一种称为Hooks的特性，让你可以在函数组件中使用状态和其他React特性，而不需要写类组件。

## Reactjs提供了以下几种Hooks

### `useState`

- `useState`: 用于在函数组件中添加状态。它返回一个状态变量和一个更新该变量的函数。你可以在组件中多次使用`useState`来定义多个状态变量。

### `useEffect`

- `useEffect`: 用于在函数组件中执行副作用，如数据获取、订阅、定时器等。它接受一个函数作为参数，该函数会在每次渲染后执行。你可以在组件中多次使用`useEffect`来定义多个副作用。

### `useContext`

- `useContext`：用于在函数组件中访问React的上下文。它接受一个上下文对象作为参数，并返回该上下文的当前值。你可以使用`useContext`来避免通过多层组件传递props。

<!--more-->

### `useReducer`

- `useReducer`：用于在函数组件中管理复杂的状态逻辑。它接受一个reducer函数和一个初始状态作为参数，并返回一个当前状态和一个分发action的函数。你可以使用`useReducer`来替代`useState`，当你的状态逻辑包含多个子值或者依赖于之前的状态时。

### `useCallback`

- `useCallback`：用于在函数组件中缓存函数。它接受一个内联函数和一个依赖数组作为参数，并返回该函数的缓存版本。你可以使用`useCallback`来避免不必要的重新渲染，当你把函数作为props传递给子组件时。

### `useMemo`

- `useMemo`：用于在函数组件中缓存计算结果。它接受一个创建值的函数和一个依赖数组作为参数，并返回该值的缓存版本。你可以使用`useMemo`来避免重复计算，当你的计算结果依赖于一些变化较少的值时。

### `useRef`

- `useRef`: 用于在函数组件中创建可变的引用对象。它接受一个初始值作为参数，并返回一个包含当前值属性（`.current`) 的对象。你可以使用
  `useRef`
  来保存任何可变值，而不会触发重新渲染，例如DOM元素、定时器ID等。

### `useImperativeHandle`

- `useImperativeHandle`: 用于在自定义Hook中自定义暴露给父组件的ref对象。它接受一个ref对象和一个创建ref对象当前值属性（`.current`) 的回调函数作为参数，并将回调函数返回
  的结果分配给ref对象当前值属性（`.current`) 。你可以使用
  `useImperativeHandle`
  来控制父组件通过ref访问子组件实例时能够访问到什么内容。

### `useLayoutEffect`

- `useLayoutEffect`: 与
  `useEffect`
  类似，但是会在所有DOM更改之后同步执行副作用回调，在浏览器绘制之前执行回调，在大多数情况下应该优先使用
  `useEffect`
  ，除非需要从DOM读取布局并同步触发更新，在这种情况下应该使用
  `useLayoutEffect`

### `useDebugValue`

- `useDebugValue`: 可以被自定义Hook开发者使用，在 React DevTools 中显示自定义hook标签。

好的，我可以给你一些使用Hooks的示例代码，但是请注意，这些代码只是为了演示Hooks的用法，并不一定代表最佳实践。如果你想要更深入地学习Hooks，请参考React官方文档¹²或者其他可靠的资源。

## 以下是一些使用Hooks的示例代码

### `useState`

```js
import React, { useState } from "react";

function Counter() {
  // 定义一个名为count的状态变量，初始值为0
  const [count, setCount] = useState(0);

  // 定义一个函数，用于增加count的值
  function increment() {
    setCount(count + 1);
  }

  // 定义一个函数，用于减少count的值
  function decrement() {
    setCount(count - 1);
  }

  // 返回一个JSX元素，显示count的值和两个按钮
  return (
    <div>
      <p>当前计数：{count}</p>
      <button onClick={increment}>+1</button>
      <button onClick={decrement}>-1</button>
    </div>
  );
}
```

### `useEffect`

```js
import React, { useEffect, useState } from "react";

function Clock() {
  // 定义一个名为time的状态变量，初始值为当前时间
  const [time, setTime] = useState(new Date());

  // 使用useEffect定义一个副作用，每隔一秒更新time的值
  useEffect(() => {
    // 创建一个定时器ID
    const timerID = setInterval(() => {
      setTime(new Date());
    }, 1000);

    // 返回一个清理函数，在组件卸载时取消定时器
    return () => {
      clearInterval(timerID);
    };
  }, []); // 空数组表示只在组件挂载时执行一次

  // 返回一个JSX元素，显示当前时间
  return <p>现在是：{time.toLocaleTimeString()}</p>;
}
```

### `useReducer`

```js
import React, { useReducer } from "react";

// 定义一个reducer函数，根据action的类型更新state
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  // 使用useReducer定义一个名为state的状态变量和一个名为dispatch的函数，初始值为{count:0}
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  // 返回一个JSX元素，显示state.count的值和两个按钮
  return (
    <div>
      <p>当前计数：{state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+1</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-1</button>
    </div>
  );
}
```

好的，我可以再给你一些useCallback和useContext的示例代码，但是请注意，这些代码只是为了演示Hooks的用法，并不一定代表最佳实践。如果你想要更深入地学习Hooks，请参考React官方文档¹²³或者其他可靠的资源。

以下是一些useCallback和useContext的示例代码：

### `useCallback`

```js
import React, { useCallback, useState } from "react";

function Child({ increment }) {
  // 返回一个JSX元素，显示一个按钮
  return <button onClick={increment}>+1</button>;
}

function Parent() {
  // 定义一个名为count的状态变量，初始值为0
  const [count, setCount] = useState(0);

  // 使用useCallback定义一个函数，用于增加count的值，并将count作为依赖项
  const increment = useCallback(() => {
    setCount((prevCount) => prevCount + 1);
  }, [count]);

  // 返回一个JSX元素，显示count的值和Child组件
  return (
    <div>
      <p>当前计数：{count}</p>
      <Child increment={increment} />
    </div>
  );
}
```

### `useContext`

```js
import React, { useContext } from "react";

// 创建一个名为ThemeContext的上下文对象，并设置默认值为light
const ThemeContext = React.createContext("light");

function App() {
  // 定义一个名为theme的状态变量，初始值为dark
  const [theme, setTheme] = React.useState("dark");

  // 定义一个函数，用于切换theme的值
  function toggleTheme() {
    setTheme((prevTheme) => (prevTheme === "dark" ? "light" : "dark"));
  }

  // 返回一个JSX元素，使用ThemeContext.Provider包裹子组件，并传递theme作为value属性
  return (
    <ThemeContext.Provider value={theme}>
      <div>
        <p>当前主题：{theme}</p>
        <button onClick={toggleTheme}>切换主题</button>
        <Toolbar />
      </div>
    </ThemeContext.Provider>
  );
}

function Toolbar() {
```
