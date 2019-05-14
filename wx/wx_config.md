# 微信小程序配置  

> 全局配置  

app.json 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。QuickStart 项目里边的 app.json 配置内容如下：

```JSON
{
  //页面路径
  "pages": ["pages/index/index", "pages/logs/index"],
  //全局默认窗口样式
  "window": {
    "navigationBarBackgroundColor":"#fff",//导航栏背景颜色
    "navigationBarTitleText": "Demo",//导航栏标题文字内容
    "navigationBarTextStyle":"black",//导航栏标题颜色
  },
  //底部tab栏的表现
  "tabBar": {
    //其中 list 接受一个数组，只能配置最少 2 个、最多 5 个 tab。
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页",
        "iconPath":"",//图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px，不支持网络图片,当 postion 为 top 时，不显示 icon。
        "selectedIconPath":"",//选中时的图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px，不支持网络图片。当 postion 为 top 时，不显示 icon。
      },
      {
        "pagePath": "pages/logs/logs",
        "text": "日志"
      }
    ]
  },
  //网络超时间
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "debug": true,
  "navigateToMiniProgramAppIdList": ["wxe5f52902cf4de896"],//跳转到其他小程序时，需要先在配置文件中声明需要跳转的小程序 appId 列表，最多允许填写 10 个。
  "usingComponents":{},//在此处声明的自定义组件视为全局自定义组件，在小程序内的页面或自定义组件中可以直接使用而无需再声明。
  "permission": {//小程序接口权限相关设置
    "scope.userLocation": {
      "desc": "你的位置信息将用于小程序位置接口的效果展示"
    }
  },
}
```  

> 页面配置

```json
{
  "navigationBarBackgroundColor": "#ffffff",
  "navigationBarTextStyle": "black",
  "navigationBarTitleText": "微信接口功能演示",
  "backgroundColor": "#eeeeee",
  "backgroundTextStyle": "light",
  "navigationStyle": "default",
  "enablePullDownRefresh": "false",//是否开启当前页面下拉刷新。
  "onReachBottomDistance": "100",//页面上拉触底事件触发时距页面底部距离，单位为px。
  "pageOrientation": "portrait",//屏幕旋转设置，支持 auto / portrait / landscape 
  "disableScroll": "false",//设置为 true 则页面整体不能上下滚动。只在页面配置中有效，无法在 app.json 中设置
  "disableSwipeBack": "false",//禁止页面右滑手势返回
  "usingComponents":{},//页面自定义组件配置
}
```

页面配置中只能设置 app.json 中 window 对应的配置项.  

> sitemap配置

```json
{
    "rules": [
        {
            "action":"allow",//命中该规则的页面是否能被索引("allow"、"disallow")
            "page":"*",//* 表示所有页面，不能作为通配符使用("*"、页面的路径)
            "params":["a", "b"],//当 page 字段指定的页面在被本规则匹配时可能使用的页面参数名称的列表（不含参数值）
            "matching":"inclusive",//当 page 字段指定的页面在被本规则匹配时，此参数说明 params 匹配方式(exact、inclusive、exclusive、partial)
            "priority":1,//优先级，值越大则规则越早被匹配，否则默认从上到下匹配
        },{
            "action": "disallow",
            "page": "path/to/page"
        }
    ]
}
```

* path/to/page?a=1&b=2 => 优先索引
* path/to/page => 不被索引
* path/to/page?a=1 => 不被索引
* path/to/page?a=1&b=2&c=3 => 不被索引
* 其他页面都会被索引

$\color{#FF0000}{以上所有配置中的注释使用时必须去掉，不然报错。}$

## 工具配置 project.config.json

小程序开发者工具在每个项目的根目录都会生成一个 project.config.json，你在工具上做的任何配置都会写入到这个文件，当你重新安装工具或者换电脑工作时，你只要载入同一个项目的代码包，开发者工具就自动会帮你恢复到当时你开发项目时的个性化配置，其中会包括编辑器的颜色、代码上传时自动压缩等等一系列选项。
