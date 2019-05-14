# 微信小程序生命周期

## 生命周期

小程序生命周期App();  
App() 必须在 app.js 中调用，必须调用且只能调用一次。不然会出现无法预期的后果。

> ### 示例代码

```js
App({
  onLaunch(options) {
    // Do something initial when launch.
    //小程序初始化完成时触发，全局只触发一次。参数也可以使用 wx.getLaunchOptionsSync 获取。
  },
  onShow(options) {
    // Do something when show.
    //小程序启动，或从后台进入前台显示时触发。也可以使用 wx.onAppShow 绑定监听。
  },
  onHide() {
    // Do something when hide.
    //小程序从前台进入后台时触发。也可以使用 wx.onAppHide 绑定监听。
  },
  onError(msg) {
    //小程序发生脚本错误或 API 调用报错时触发。也可以使用 wx.onError 绑定监听。
    console.log(msg);
  },
  onPageNotFound(res) {
    wx.redirectTo({
      url: 'pages/...'
    }) // 如果是 tabbar 页面，请使用 wx.switchTab
  },
  globalData: 'I am global data'//开发者可以添加任意的函数或数据变量到 Object 参数中，用 this 可以访问
})
```

> ### 页面生命周期

```js
Page({
  data: {//data 是页面第一次渲染使用的初始数据。
    text: 'This is page data.'
  },
  onLoad(options) {
    // Do some initialize when page load.
    //页面加载时触发。一个页面只会调用一次，可以在 onLoad 的参数中获取打开当前页面路径中的参数。
  },
  onShow() {
    // Do something when page show.
    //页面显示/切入前台时触发。
  },
  onReady() {
    // Do something when page ready.
    //页面初次渲染完成时触发。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。
  },
  onHide() {
    // Do something when page hide.
    //页面隐藏/切入后台时触发。 如 wx.navigateTo 或底部 tab 切换到其他页面，小程序切入后台等。
  },
  onUnload() {
    // Do something when page close.
    //页面卸载时触发。如wx.redirectTo或wx.navigateBack到其他页面时。
  },
  onPullDownRefresh() {
    // Do something when pull down.
    //需要在app.json的window选项中或页面配置中开启enablePullDownRefresh
    //可以通过wx.startPullDownRefresh触发下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致。
    //当处理完数据刷新后，wx.stopPullDownRefresh可以停止当前页面的下拉刷新。
  },
  onReachBottom() {
    // Do something when page reach bottom.
    //可以在app.json的window选项中或页面配置中设置触发距离onReachBottomDistance。
    //在触发距离内滑动期间，本事件只会被触发一次。
  },
  onShareAppMessage() {
    // return custom share data when user share.
    //监听用户点击页面内转发按钮（<button> 组件 open-type="share"）或右上角菜单“转发”按钮的行为，并自定义转发内容。
  },
  onPageScroll() {
    // Do something when page scroll
    //请只在需要的时候才在 page 中定义此方法，不要定义空方法。以减少不必要的事件派发对渲染层-逻辑层通信的影响。 注意：请避免在 onPageScroll 中过于频繁的执行 setData 等引起逻辑层-渲染层通信的操作。尤其是每次传输大量数据，会影响通信耗时
  },
  onResize() {
    // Do something when page resize.基础库 2.4.0 开始支持
    //主要为小程序屏幕旋转时触发。
  },
  onTabItemTap(item) {//点击 tab 时触发
    console.log(item.index)//被点击tabItem的序号，从0开始
    console.log(item.pagePath)//被点击tabItem的页面路径
    console.log(item.text)//被点击tabItem的按钮文字
  },
  // 开发者可以添加任意的函数或数据到 Object 参数中，在页面的函数中用 this 可以访问
  viewTap() {
    this.setData({//组件事件处理函数,在渲染层的组件中加入事件绑定，当事件被触发时，就会执行 Page 中定义的事件处理函数。
      text: 'Set some data for updating view.'
    }, function () {
      // this is setData callback
    })
  },
})
```

* this.route获得当前页面路径
* getCurrentPages()。获取当前页面栈。数组中第一个元素为首页，最后一个元素为当前页面。
  * 不要尝试修改页面栈，会导致路由以及页面状态错误。
  * 不要在 App.onLaunch 的时候调用 getCurrentPages，此时 page 还没有生成。
* 小程序的数据更新，不能直接修改this.data,而是使用this.setData(Object data, Function callback)方法.
* 仅支持设置可 JSON 化的数据。
* 单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据。
* 请不要把 data 中任何一项的 value 设为 undefined ，否则这一项将不被设置并可能遗留一些潜在问题。

> ### 页面生命周期图示

![页面生命周期](../static/page-lifecycle.png)

> ### 组件生命周期

* 生命周期方法可以直接定义在 Component 构造器的第一级参数中。
* 其中，最重要的生命周期是 created attached detached ，包含一个组件实例生命流程的最主要时间点。

  * 组件实例刚刚被创建好时， created 生命周期被触发。此时，组件数据 this.data 就是在 Component 构造器中定义的数据 data 。 此时还不能调用 setData 。 通常情况下，这个生命周期只应该用于给组件 this 添加一些自定义属性字段。
  * 在组件完全初始化完毕、进入页面节点树后， attached 生命周期被触发。此时， this.data 已被初始化为组件的当前值。这个生命周期很有用，绝大多数初始化工作可以在这个时机进行。
  * 在组件离开页面节点树后， detached 生命周期被触发。退出一个页面时，如果组件还在页面节点树中，则 detached 会被触发。

自小程序基础库版本 2.2.3 起，组件的的生命周期也可以在 lifetimes 字段内进行声明（这是推荐的方式，其优先级最高）。

```js
Component({
  lifetimes: {
    created:(){
      //在组件实例刚刚被创建时执行
    },
    attached() {
      // 在组件实例进入页面节点树时执行
    },
    ready() {
      // 在组件在视图层布局完成后执行
    },
    moved() {
      // 在组件实例被移动到节点树另一个位置时执行
    },
    detached() {
      // 在组件实例进入页面节点树时执行
    },
    error(e) {
      // 每当组件方法抛出错误时执行
    },
  },
  pageLifetimes: {
    show() {
      // 页面被展示
    },
    hide() {
      // 页面被隐藏
    },
    resize(size) {
      // 页面尺寸变化
    }
  }
})
```

在 behaviors 中也可以编写生命周期方法，同时不会与其他 behaviors 中的同名生命周期相互覆盖。但要注意，如果一个组件多次直接或间接引用同一个 behavior ，这个 behavior 中的生命周期函数在一个执行时机内只会执行一次。  

还有一些特殊的生命周期，它们并非与组件有很强的关联，但有时组件需要获知，以便组件内部处理。这样的生命周期称为“组件所在页面的生命周期”，在 pageLifetimes 定义段中定义。其中可用的生命周期包括。

>小程序运行环境和渲染

* 小程序在IOS,Android中运行环境不同。
  * 在 iOS 上，小程序逻辑层的 javascript 代码运行在 JavaScriptCore引擎 中，视图层是由 WKWebView 来渲染的；
  * 在 Android 上，新版本小程序逻辑层的 javascript 代码运行在v8引擎中，视图层是由自研 XWeb 引擎基于 Mobile Chrome 67 内核来渲染的；
  * 渲染层和逻辑层通过微信客户端 Native 来做中转

* 页面渲染时，先根据 json 配置文件生成对应界面（页面没有json配置就会调用app.json配置），然后解析 WXML 结构文件和 WXSS 样式文件，最后解析 js 文件。同时会将当前页面放入页面栈中。
* 小程序生成界面后，ui 开始生成抽象结构层，然后等待逻辑层处理完数据，并且是在页面显示的生命周期，向逻辑层请求数据，开始生成完整的抽象页面层，然后渲染出最后的页面。
* 页面在 onReady 生命周期后能通过操作进行页面交互，当页面隐藏后会保存当前页面的状态，如果重新进入当前页面，会从内存中将页面重新取出显示。如果重新进入的非当前页，就会清空页面栈，从首页重新进入。
* 页面返回上一个页面会执行 onUnload 卸载页面，并且删除页面栈中当前页。

![渲染过程](../static/ui-render.png)
