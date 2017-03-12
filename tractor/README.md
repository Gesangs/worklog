# 下拉加载（刷新）和滚动加载

> 由于 MSUI 自带的下拉刷新不能满足业务中下拉加载新内容的需求，并且如果用作下拉加载的话会有很严重的性能问题！所以只能使用其它插件的方式实现业务需求。然而在找插件的过程中也并没有发现什么跟项目接地气的好轮子，so... 自己参考一些轮子的实现然后自己写了一个自认为很 OK 的集成下拉加载（刷新）和滚动加载特性的轮子，不幸的是还没等到在项目中使用，项目突然就被砍了，并且我也失业啦。

<!-- more -->

[插件地址](https://github.com/Monine/worklog/tree/master/tractor)

**此轮子不依赖任何插件**，只是如果对兼容性要求特别高，比如 IE9 以下，请自行修改 tractor.js 源码内的事件绑定，添加 IE 的事件绑定实现 `attachEvent`，我个人对 IE 的兼容性从来都是无视的，实在不想被折磨。然后轮子有使用到 `requestAnimationFrame`，同样如果需要极其苛刻的兼容性请自行添加 [Polyfill](https://github.com/darius/requestAnimationFrame)。

其实这个轮子两周之前就写出来了，但是一直没用到项目上是因为当时这个轮子还存在一个未知之谜：下拉开始的时候会莫名其妙出现闪屏的现象，通过分析发现是因为使用了 `transition` 的原因，但是我这的实现方案必须依赖 `transition`。使用了一系列解决办法都没什么用，比如设置进行转换的元素的背面在面对用户时隐藏 `-webkit-backface-visibility: hidden;`，保留 3D `-webkit-transform-style: preserve-3d;`，然而就在今天我把代码的结构调整了一下之后，这么问题（现象）就莫名奇妙解决了，实现的方案没有任何改变，可就是这么莫名奇妙的好啦...出于不达目的不罢休的心态，实在想弄明白咋回事，就对比了下前后代码，发现 JS 实现除了结构其它没有任何变化，没办法再看看 Demo 的 html 文件，发现样式有些小删减，还原之后再查看 Demo 发现尼玛闪屏的现象又出现啦！这尼玛能忍？害我纳闷了这么久，最终定位问题是出在 `overflow` 属性上，绕了这么大一圈，原来是 CSS 的问题。

此轮子的实现核心有以下几个点：

1. touch 事件和 scroll 事件
  touch 事件控制下拉加载（刷新），scroll 事件控制滚动加载。
2. touchstart 时容器的 `scrollTop` 值
  先确认容器滚动目前处于顶部（`scrollTop` 值为 0），此时记录手指在屏幕的坐标值并标识当前处于下拉状态。
3. 确定触发下拉状态之后手指在屏幕上移动的距离
  touchmove 时计算出手指在 y 轴滑动的距离，用来计算容器偏移的 `translate` 值，此时要随着偏移的距离切换显示不同的提示信息。
4. scroll 时容器的 `scrollTop` 值
  容器滚动到离底部还剩一定距离（阈值）时触发加载事件，`容器 scrollHeight - 容器 height - 容器滚动距离 scrollTop <= 滚动阈值 tractor.scrollValve`。

下拉时在不同的状态下，会给容器添加不同的 className 和 hock：

1. 当确定触发下拉状态并滑动添加 className `tractor-touching`，触发 onDragStart hock。
2. 当下拉的距离小于等于设置的阈值添加 className `tractor-less`，触发 onDragLessValve hock。
3. 当下拉的距离大于设置的阈值添加 className `tractor-greater`，触发 onDragGreaterValve hock。
4. 当手指离开屏幕触发 touchend 事件，并且下拉的距离大于设置的阈值，添加 className `tractor-refreshing`，触发 onDragDone hock。

滚动时只会添加一个 hock：当滚动到阈值时触发 onScroll2Valve hock。

*Tips: 以上 className 都会在适当的时候移除*

**注意：**
1. 下拉加载（刷新）完成之后请务必调用 Tractor 原型上的 dragLoadingDone 方法，表示此次下拉结束。
2. 滚动加载完成之后请务必调用 Tractor 原型上的 scrollLoadingDone 方法，表示此次滚动结束。

最后，上 [Demo](https://cdn.rawgit.com/Monine/worklog/432d9a2a/tractor/tractor.html) 和参数介绍：

|  参数(Argument)  |  类型(Type)  |  描述(Describe)  |  默认(Default)  |
|   :--:  |  :--:  |  :--:  |  :--:  |
|  `scroller`  |  String  |  滚动容器  |  `body`  |
|  `openDragLoading`  |  Boolean  |  是否开启下拉加载（刷新）  |  `true`  |
|  `openScrollLoading`  |  Boolean  |  是否开启滚动加载  |  `true`  |
|  `dragValve`  |  Number  |  下拉阈值  |  40  |
|  `scrollValve`  |  Number  |  滚动阈值  |  40  |
|  `onDragStart`  |  Function  |  hock  |  空函数  |
|  `onDragLessValve`  |  Function  |  hock  |  空函数  |
|  `onDragGreaterValve`  |  Function  |  hock  |  空函数  |
|  `onDragDone`  |  Function  |  hock  |  空函数  |
|  `onScroll2Valve`  |  Function  |  hock  |  空函数  |