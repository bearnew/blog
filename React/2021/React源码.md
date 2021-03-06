# React源码
## 1.Virtual DOM（虚拟dom）
1. `Virtual Dom`是一种编程概念，UI以理想化的，或者说“虚拟化”表现形式保存在内存中，并通过`ReactDOM`等类库使之与真实的DOM同步，这一过程叫做协调。
2. `React`使用`fibers`的内部对象来存放组件树的附加信息。
3. 虚拟Dom能够将开发者从属性操作、事件处理、手动DOM更新等操作解放出来
4. 使用js对象来描述真实的dom节点
5. 原生dom节点对象很大，Dom操作很慢，轻量的操作都可能导致页面重新排列，非常耗性能，相对于DOM对象，js对象处理起来更快，更简单。通过diff算法对比新旧vdom之间的差异，可以批量的、最小化的执行dom操作，从而提高性能。
6. `React`中用jsx语法描述视图，该函数将生成`vdom`来描述真实的`dom`，状态变化，`vdom`将做出相应变化，再通过`diff`算法对比新老`vdom`区别从而做出最终`dom`操作。
## 2.JSX
1. 什么是`jsx`
    * 语法糖
    * `React`中使用`jsx`来代替常规的`javascript`
    * `jsx`是一个看起来像`XML`的`javascript`语法扩展
2. 为什么使用`jsx`
    * 开发效率：使用`jsx`编写语法模版简单快速
    * 执行效率：`jsx`编译为`javascript`代码后进行了优化，执行更快
    * 类型安全：在编译过程中就能发现错误
3. `React 16`原理：`babel-loader`会预编译`jsx`为`React.createElement(...)`
4. `React 17`原理：`React 17`中的转换不会将`jsx`转换为`React.createElement(...)`, 而是自动从`React`的`package`中引入新的入口函数并调用。另外此次升级不会改变`jsx`语法，旧的`jsx`转换也将继续工作。
```js
function App() {
  return <h1>Hello World</h1>;
}

// 转换后
// 由编译器引入（禁止自己引入！）
import {jsx as _jsx} from 'react/jsx-runtime';

function App() {
  return _jsx('h1', { children: 'Hello world' });
}
```
5. 与`vue`的异同
    * react中的虚拟`dom`+`jsx`是一开始就有，`vue`则是演进过程中才出现的。
    * `jsx`本来就是`js`扩展，转换过程简单直接得多，`vue`把`template`编译为`render`函数的过程需要复杂的编译器转换字符串-ast-js函数字符串
## 3.reconciliation协调
### 1.设计动力
* 在某一时间节点，调用`react`的`render()`方法，会创建一颗由`React`元素组成的树。在下一次`props`或者`state`更新时，相同的`render()`方法会返回一颗不同的树。`React`需要基于这两棵树的差别来判断如何有效率的更新`UI`以保证当前的`UI`与最新的树保持同步。
* 通用的比较解决方案的复杂度是`O(n3)`，n是树中的数量
* `React`提出了一套`O(n)`的`diffing`算法
  * 两个不同类型的元素会产生不同的树
  * 开发者通过`key prop`来暗示哪些子元素在不同的渲染下能保持稳定
  * 在实践中，我们发现以上的假设在几乎所有的实际中都成立
### 2.diff策略
1. 同级比较，`web UI`中的`Dom`节点跨层级的移动操作特别少，可以忽略不计
2. 拥有不同类型的两个组件将会生成不同的树形结构，例: `div->p`, `CompA->CompB`
3. 开发者可以通过`key prop`来暗示哪些子元素在不同的渲染下能保持稳定
### 3.diff过程
* __对比两个虚拟dom时会有三种操作：删除、替换、更新__
* `vnode`是现在的虚拟`dom`，`newVnode`是新的虚拟`dom`
* 删除：`newVnode`不存在时
* 替换：`vnode`和`newVnode`类型不同，或者`key`不同
* 更新：有相同类型和`key`但`vnode`和`newVnode`不同
* 实践中也证明这3个前提策略是合理且准确的，保证了整体界面构建的性能
## 4.fiber
### 1.为什么需要fiber
1. 对于大型项目，组件树会很大，这个时候递归遍历的成本就会很高，会造成主线程被持续占用，结果就是主线程上的布局、动画等周期性的任务无法立即得到处理，造成视觉上的卡顿，影响体验。
2. 任务分解的意义，解决1的问题
3. 增量渲染，把渲染任务拆分成块，匀到多帧
4. 更新时能够暂停、终止、复用渲染任务
5. 给不同类型的更新赋予优先级
6. 并发
7. 更流畅
### 2.什么是fiber
* `fiber`是指组件上将要完成或者已经完成的任务，每个组件可以一个或者多个
