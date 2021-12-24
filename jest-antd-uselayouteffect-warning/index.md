# ⚠️Jest Antd useLayoutEffect Warning


<!--more-->
**场景：** 使用 [antd](https://github.com/ant-design/ant-design) 二次封装的组件库，使用 [jest](https://jestjs.io/zh-Hans/) 进行组件单元测试

**问题：** 实际渲染成功，但是控制台提示
    ```
    Warning: useLayoutEffect does nothing on the server, because its effect cannot be encoded into the server renderer’s output format. This will lead to a mismatch between the initial, non-hydrated UI and the intended UI. To avoid this, useLayoutEffect should only be used in components that render exclusively on the client.
    ```

**参考：** [stackoverflow](https://stackoverflow.com/questions/58070996/how-to-fix-the-warning-uselayouteffect-does-nothing-on-the-server)

**原因：** useLayoutEffect 渲染之前同步执行，可能会导致渲染结果不一样，所以不应该在 ssr 时使用，所以在使用 enzyme 的 render 方法渲染组件时，就会报这个警告

**修复：**
1. testSetUp.js 文件中，替换 useLayoutEffect 方法  
    ```js
    import React from 'react'

    React.useLayoutEffect = React.useEffect
    ```
2. jest mock react
    ```js
    jest.mock('react', () => {
      ...jest.requireActual('react'),
      useLayoutEffect: jest.requireActual('react').useEffect
    })
    ```

