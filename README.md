# 简答题

## 1. 请简述 React 16 版本中初始渲染的流程
1. render阶段
负责创建Fiber数据结构并为Fiber节点打标记，标记当前Fiber节点要进行的DOM操作
2. commit阶段
负责根据Fiber节点标记effectTag进行相应的DOM操作

## 2. 为什么 React 16 版本中 render 阶段放弃了使用递归
1. 在react 15采用循环加递归的方式进行virtualDOM的对吧，由于递归使用js自生的执行栈，一旦开始就无法停止，直到认为执行完毕，如果virtualDOM树的层级比较深，virtualDOM的对比占用js主线程，由于js又是单线程无法同时执行其他任务，所以在对比的过程中，无法响应用户操作，无法及时执行元素动画，造成页面卡顿
2. React 16放弃了递归进行对比，采用循环模拟递归，对比过程采用浏览器的空闲时间完成，不会长期占用主线程，解决页面卡顿问题
3. react 16没有采用requestIdleCallback， 因为浏览器兼容性问题，也不太稳定，自己实现了scheduler调度层


## 3. 请简述 React 16 版本中 commit 阶段的三个子阶段分别做了什么事情
1. before mutation阶段，执行dom操作前
调用类组件的 getSnapshotBeforeUpdate 生命周期
2. mutation阶段， 执行dom操作
提交 hostComponent 的 side effect, dom节点的操作
3. layout阶段,执行dom操作后
执行渲染完成之后的会点函数



## 4. 请简述 workInProgress Fiber 树存在的意义是什么
1. 用于双缓存技术，完成fiber树的构建与替换，实现dom对象的快速更新
2. 当发生更新时，react会在内存中重新构建一个fiber树，就是workInProgress Fiber,当fiber树构建完成后，react会使用它替换current Fiber，达到快速更新dom的目的