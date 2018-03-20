# Rendering&Updates

以一个示例说明React是如何渲染与更新的。![](/assets/renders-update.png)

图中的Shop和Users组件很多情况下是containers组件。每个组件负责管理与它相关的state,下面也包含子组件。

**更新都是从上到下，比如在Shop组件中调用setState方法导致state发生改变，默认情况下Shop更新会导致下面的子组件同样发生updates。即使传入子组件的props没有发生变化。**

**List和Cart组件的props属性是依赖与父组件Shop。所以在Shop中使用shouldComponentUpdate\(\)方法来检查state是不是真的发生了改变，进而可以停止往下更新。**

**在Shop组件中可能调用了setState方法，但是List和Cart的props值还是与之前一样，这个时候就没有必要重新渲染一遍组件。**

![](/assets/shouldCpmUpdate.png)

像上图示例中一样，在有些情况下在containers组件中进行一些检查可以有效防止一些不必要的更新。

# How React Updates the DOM

一张图说明更新过程

![](/assets/v-dom.png)







