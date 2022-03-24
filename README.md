# 我的 React 心智模型基础

在使用 React 的时候，我时常会冒出为什么 React 是这个表现，它在这里到底是怎么工作的等问题。这些问题文档上可能没讲可能一笔带过，但是它们却是理解 React 的基础。现将这些问题总结下来，一方面作为自己的备忘，一方面希望能帮助更多有相同困惑的人。我深知，我对这些问题的理解尚浅，我旨在用简短的描述将它们的基本原理叙述出来，并附上相应的参考链接，如有错误或者描述不清的地方，还请指正。

## 问题1：React 组件什么时候 render ？

当一个组件的 state 改变时，它就会 render。

这时候对于它的子组件：

如果子组件是 class 类型，那么它是否 render 由 shouldComponentUpdate 决定。

如果子组件是 function 类型，那么它无论如何都会 render。

如果子组件是 memo 类型，那么会根据 props 是否改变来决定是否 render。

> 这里的 render 现象就是 render 函数被调用。

## 问题2：React 如何判断 state 改变？

React 用 Object.is() 来确定 state 是否改变。

参考链接：[objectIs.js](https://github.com/facebook/react/blob/main/packages/shared/objectIs.js)

## 问题3：React 如何判断 hook 的依赖改变？

React 用 shallowEqual 来判断依赖是否改变。

参考链接：[shallowEqual.js](https://github.com/facebook/react/blob/main/packages/shared/shallowEqual.js)


## 问题4：React 在 class 组件中的 setState 的 shallow merge 是什么？

React 用 Object.assign 来实现 shallow merge。

参考链接：[assign.js](https://github.com/facebook/react/blob/main/packages/shared/assign.js)

> useState hook 返回的 set 函数是直接替换。

## 问题5：React 为什么有 useLayoutEffect？

useLayoutEffect 发生在 React 修改 dom 之后，在浏览器渲染之前。这中间一段时间可能被因为一些原因被阻塞。而 useEffect 则发生在浏览器渲染之后。因此如果你在 useEffect 中做一些修改 dom 的操作，可能会出现延迟的现象。

参考链接：[ReactFiberWorkLoop.new.js](https://github.com/facebook/react/blob/main/packages/react-reconciler/src/ReactFiberWorkLoop.new.js)