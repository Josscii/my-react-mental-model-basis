# 我的 React 心智模型基础

在使用 React 的时候，我时常会冒出为什么 React 是这个表现，它在这里到底是怎么工作的等问题。这些问题文档上可能没讲可能一笔带过，但是它们却是理解 React 的基础。现将这些问题总结下来，一方面作为自己的备忘，一方面希望能帮助更多有相同困惑的人。我深知，我对这些问题的理解尚浅，我旨在用简短的描述将它们的基本原理叙述出来，并附上相应的参考链接，如有错误或者描述不清的地方，还请指正。

## 问题1：React 组件什么时候 render ？

> 这里的 render 现象就是 render 函数被调用。

对于 function component，当 useState 或者 useReducer 返回的 state 改变时，它会 render。

对于 class component，当调用 setState 且 shouldComponentUpdate 返回 true 或者调用 forceUpdate 时，它会 render。

这时候对于它的子组件，如果其 props 改变了，那么它就会 render。

> 这里的子组件指的是 render 函数返回的内容。

因为父组件 render 必定会调用其 render 函数，而 render 函数返回的每次都是通过 React.createElement 来创建的子组件，传入的 props 是新创建的对象，而判断 props 改变用的是 ===，所以 props 一般来说一定是改变的。

不过也有下面几种特殊情况：

1. 如果子组件是 class 类型，那么它是否 render 由 shouldComponentUpdate 决定。
2. 如果子组件是 memo 类型，那么会根据 props 是否改变来决定是否 render。
3. 如果子组件是传入的 children，那么这时候不会 render，因为它没有重新 createElement，自然 props 也没有改变。

## 问题2：React 如何判断 state 改变？

对于 function component，React 用 Object.is() 来确定 state 是否改变。

对于 class component，React 不会判断 state 是否改变。

参考链接：[objectIs.js](https://github.com/facebook/react/blob/main/packages/shared/objectIs.js)

## 问题3：React 如何判断 hook 的依赖改变？

React 用 shallowEqual 来判断依赖是否改变。

参考链接：[shallowEqual.js](https://github.com/facebook/react/blob/main/packages/shared/shallowEqual.js)


## 问题4：React 在 class 组件中的 setState 的 shallow merge 是什么？

React 用 Object.assign 来实现 shallow merge。

参考链接：[assign.js](https://github.com/facebook/react/blob/main/packages/shared/assign.js)

> useState hook 返回的 set 函数是直接替换。

## 问题5：useLayoutEffect 和 useEffect 有什么区别？

useLayoutEffect 发生在 React 修改 dom 之后，在当前宏任务里面同步回调的。

而 useEffct 则是异步的。

参考链接：[ReactFiberWorkLoop.new.js#L2073](https://github.com/facebook/react/blob/e7d0053e65db49a536440eb24e6c1e4961d976f6/packages/react-reconciler/src/ReactFiberWorkLoop.new.js#L2073)

## 问题6：改变 state 是异步的？还是 batch 的？

react 18 之前：

如果是在 react 事件回调里面，就是异步且 batch 的。否则，是同步的。

react 18 之后：

无论更新出现在哪里，都是异步且 batch 的。

参考链接：[ReactFiberWorkLoop.new.js#L723](https://github.com/facebook/react/blob/2e0d86d22192ff0b13b71b4ad68fea46bf523ef6/packages/react-reconciler/src/ReactFiberWorkLoop.new.js#L723)

> 上述两个问题提到的异步都指的是不在当前宏任务里同步执行。具体在何时执行由实现决定，有可能是微任务，也有可能是下一个宏任务。
