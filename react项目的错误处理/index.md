# handle errors in React


<!--more-->
实际应用中，总会因为测试范围不够，或者编码过程中边界情况考虑不足等等原因导致错误的出现。为了避免一个小的报错就导致页面卡死这样的尴尬，如何优雅地处理错误始终是项目上线前我们需要思考的问题。

如果在实际开发中，你问我：React 项目中如何处理错误？那我直接推荐 [react-error-boundary](https://github.com/bvaughn/react-error-boundary)
```js
import { ErrorBoundary, useErrorBoundary } from 'react-error-boundary'

const SomeComponent = () => {
  const { showBoundary } = useErrorBoundary()

  useEffect(() => {
    throw new Error('some render error')
  }, [])

  const handleClick = () => {
    showBoundary(new Error('some async error'))
  }

  return (
    <div onClick={handleClick}>
      some component
    </div>
  )
}


const App = () => {
  return (
    <ErrorBoundary fallback={<div>Someting went wrong!</div>}>
      <SomeComponent />
    </ErrorBoundary>
  )
}
```

可以认为这个库是官方 [Error Boundaries](https://legacy.reactjs.org/docs/error-boundaries.html) 的增强版，使用它就能获得足够优雅的错误处理功能。如果这就是你需要的，那看看 [它的文档](https://www.npmjs.com/package/react-error-boundary) 就够了，如果还想了解更多，那继续往下看。

## try/catch
如何捕获 JavaScript 中的错误？通常我们使用 `try/catch` 语句，`Promise` 中的 `catch` 方法也是同理：
```js
try {
  // function might throw error
  func()
  await asyncFunc()
} catch (error) {
  // catch error
  doWhenError()
}
```

如果是在 React 组件中，我们很容易想到为组件设置一个错误状态，如果产生错误，就改变错误状态，从而渲染错误提示组件
```js
const SomeComponent = () => {
  const [hasError, setHasError] = useState(false)

  useEffect(() => {
    try {
      func()
    } catch (error) {
      setHasError(true)
    }
  }, [])

  if (hasError) return <CommonError />

  return <div>some component</div>
}
```

不过这个方法有几点不足：
1. ❗️不能直接包裹 hook、请求等异步方法，必须在每个异步操作中单独捕获  
    ```js
    try {
      useEffect(() => {
        throw new Error('some error!')
      }, [])
    } catch (error) {
      // can't catch error
    }
    ```
2. ❗️无法捕获子组件，子组件只是被创建，实际渲染时才会触发错误，而这种方法是无法捕获的
    ```js
    const SomeComponent = () => {
      try {
        return <SomeErrorChild />
      } catch (error) {
        // can't catch error
      }
    }
    ```
3. ❗️如果在渲染过程中重置错误状态，会造成渲染死循环
    ```js
    const SomeComponent = () => {
      const [hasError, setHasError] = useState(false)

      try {
        someErrorFunc()
      } catch (error) {
        // might loop rendering
        setHasError(true)
      }

      // ...
    }
    ```

## ErrorBoundary
React 16 版本后引入了[「错误边界」](https://legacy.reactjs.org/docs/error-boundaries.html)的概念，处理子组件渲染过程中的错误。按照官方文档说明，通过实现 `getDerivedStateFromError`、`componentDidCatch` 两个方法，我们可以封装一个这样的错误处理组件 `ErrorBoundary`：
```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true }
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo)
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return this.props.fallback
    }

    return this.props.children
  }
}
```

用它包裹子组件，可以捕获子组件渲染错误，并展示优雅降级的页面：
```js
<ErrorBoundary fallback={<>Something went wrong.</>}>
  <SomeComponent />
</ErrorBoundary>
```

不过错误边界只能够处理发生在 React 生命周期中的错误，异步请求中、点击事件处理等生命周期之外的错误，将无法被捕获，比如：
```js
const SomeComponent = () => {
  useEffect(() => {
    // can catch
    throw new Error('error!!!')
  }, [])

  const handleClick = () => {
    // can't catch
    throw new Error('error!!!')
  }

  return (
    <div onClick={handleClick}>
      some component
    </div>
  )
}

const App = () => {
  return (
    <ErrorBoundary fallback={<>Something went wrong.</>}>
      <SomeComponent />
    </ErrorBoundary>
  )
}
```

回顾前文，对于这样的错误，我们可以使用 `try/catch` 来捕获，将组件错误状态抛给上层统一处理。那结合 `try/catch` 和 `ErrorBoundary`，我们就能完整地捕获组件错误了：
```js
const SomeComponent = ({ onError }) => {
  const handleClick = () => {
    try {
      throw new Error('error!!!')
    } catch (error) {
      onError()
    }
  }

  return (
    <div onClick={handleClick}>
      some component
    </div>
  )
}

const App = () => {
  const [hasError, setHasError] = useState(false)

  const fallback = <>Something went wrong.</>

  if (hasError) return fallback

  return (
    <ErrorBoundary fallback={fallback}>
      <SomeComponent onError={() => setHasError(true)} />
    </ErrorBoundary>
  )
}
```

功能上是满足了，但还不够优雅：
1. 需要分别维护 `ErrorBoundary` 和顶层组件的 `hasError` 错误状态
2. 每个子组件都需要做重复的处理，需要处理 `onError` 参数

为了解决这个问题，需要考虑是否能够只处理 `ErrorBoundary` 的状态，让它也能处理异步错误？

## more
因为 `ErrorBoundary` 只能够处理 React 生命周期中的错误，那对于异步错误，我们把它抛回重渲染的生命周期，是不是就能够重新被 `ErrorBoundary` 捕获了呢？  
参考 [这个方法](https://github.com/facebook/react/issues/14981#issuecomment-468460187)，我们在 `try/catch` 捕获到错误时，通过 `setState` 回调函数抛出错误，引发重新渲染，这样就可以被最近的 `ErrorBoundary` 捕获到
```js
const SomeComponent = () => {
  const [errorState, setErrorState] = useState()

  const handleClick = () => {
    try {
      throw new Error('error!!!')
    } catch (error) {
      setErrorState(() => {
        throw error
      })
    }
  }

  return (
    <div onClick={handleClick}>
      some component
    </div>
  )
}

const App = () => {
  return (
    <ErrorBoundary fallback={<>Something went wrong.</>}>
      <SomeComponent />
    </ErrorBoundary>
  )
}
```

为了提高复用能力，可以封装一个简单的 `hook`:
```js
const useShowError = () => {
  const [errorState, setErrorState] = useState()

  return (error) => {
    setErrorState(() => throw error)
  }
}

const SomeComponent = () => {
  // ...
  const showError = useShowError()

  const handleClick = () => {
    try {
      throw new Error('error!!!')
    } catch (error) {
      showError(error)
    }
  }

  // ...
}
```
到此为止，是不是已经很像 `react-error-boundary` 的使用方法了，那恭喜你也已经掌握了 React 错误处理需要了解的知识。

> https://www.developerway.com/posts/how-to-handle-errors-in-react

