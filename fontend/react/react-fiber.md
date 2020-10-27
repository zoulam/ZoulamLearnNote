# \[react\]-fiber

[React Fiber是什么](https://zhuanlan.zhihu.com/p/26027085)

[React Fiber架构](https://zhuanlan.zhihu.com/p/37095662)作者司徒正美

fiber：中文意思是纤维，即极小的粒度，此处**【可以理解为】**是比线程更细的粒度

## 初览

架构的好处

* 组件不能返回数组
* 新增 [portal](https://zh-hans.reactjs.org/docs/portals.html) 传送门，

  在过去添加`dialog`，只能插入到root节点**【在确定其位置时十分困难】**，使用传送门就不用把所有的内容全部塞在root节点上，而是可以自定义化，

* 更好的异常处理
* HOC无法传输 `ref`**【createRef与forwardRef】** 和`context`**【new Context API】**的问题
* 对react做出性能优化**【涉及到批量更新及基于时间分片的限量更新。】**

  过去的缺陷：JS的单线程出现过长的同步任务就会出现明显的卡顿。

  ​ 网友生动的比喻：🏊‍的时候需要浮上去换气，而不是一口气游完

### 优化原理

粗暴的理解：以插入10000个节点为例

​ 传统：创建一个就插入一个，插入10000次

​ fiber：一次插入一坨，如：先创建一百个，再插入，插入一百次

好处：前者reflow一万次，性能差

### 为什么需要了解fiber

组件更新分为两个阶段：Reconciliation Phase【调度阶段，会被打断】Commit Phase【提交阶段，不会被打断】

