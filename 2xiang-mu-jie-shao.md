##目录结构
```
|--dist                                    -------------------------npm run build生成的目录文件
    |--web
    |--weex
|--node_modules                            ------------------------安装依赖
|--platforms                               ------------------------app目录，用于调试
    |--android
    |--ios
|--src                                     ------------------------开发目录
    |--assets                                 ------------------------文件
    |--components                              ------------------------组件
    |--constants                                 ------------------------常量配置
    |--entry                                     ------------------------webpack生成的页面入口
    |--filters                                 ------------------------全局过滤器
    |--locales                                 ------------------------本地文件
    |--mixins                                  ------------------------全局混合(全局方法写这里)
    |--mock                                    ------------------------数据mock
    |--plugins                                 ------------------------插件
        |--i18n 
    |--services                                ------------------------服务
    |--router                                   ------------------------路由
    |--styles                                  ------------------------全局样式表
    |--utils                                   ------------------------工具
    |--views                                   ------------------------页面
    App.web.vue                                 ------------------------web启动页
    entry.js                                ------------------------入口
|--test                                    ------------------------测试目录
    |--unit                                    ------------------------单元测试
|--web                                     ------------------------web启动文件夹
    |--assets                                  ------------------------
    index.html
android.config.json                     ------------------------安卓配置文件
ios.config.json                         ------------------------苹果配置文件
package.json                            ------------------------依赖
start                                   ------------------------开始命令
webpack.config.js                       ------------------------webpack配置
webpack.dev.js                          ------------------------webpack启动服务器配置
webpack.test.conf.js                    ------------------------webpack测试配置
```

##前期准备
- node
- weex-toolkit  ``` npm install -g weex-toolkit```
- 安卓/ios开发环境 (参考https://weex.apache.org/cn/guide/integrate-to-your-app.html)
- 安装[playground](https://weex.apache.org/cn/playground.html) 

##执行
- 安装依赖 ``` npm install```
- 生成 ``` npm run build```
- 启动服务器 
    * ``` npm run serve2```
        - 浏览器打开 http://localhost:8081 进入,可以用playground扫描二维码查看在native的表现
            ![](/assets/001aserve.jpg)
    
        - 浏览器打开 http://localhost:8081/web 进入首页，其他页面可查看路由
            ![](/assets/001首页.jpg)
    * ``` npm run serve``` (支持热刷新)
    * ``` ./start``` (启动服务, 监听文件变化重新build, 启动调试)

##调试 ```注意在web上显示的效果极有可能在真机上有所不同，所以开发时也要在真机上看效果```
###一、网页调试
####A.网页调试web
- 在chrome里按照以往经验调试

####B.playground调试 [网页调试native或web](https://weex.apache.org/cn/guide/tools/toolkit.html)
- 执行``` weex debug```

  这里单纯启动一个调试服务器，并同时唤起Chrome浏览器打开调试主页。这个调试主页上会有一个二维码
![](/assets/002weex-debug.jpg)
  
- 连接调试

 使用 Playground App 扫这个二维码可以开启 Playground 调试。开启调试后,设备列表中会出现您的设备，根据提示进行后续的调试操作。
 
![](/assets/003weex-debug2.jpg)

- 启动serve ``` npm run serve```
- 浏览器打开 http://localhost:8081 
- 用playground扫描二维码

![](/assets/005weex-debug3.jpg)

- 点击debugger或者inspect进行测试(参考https://weex.apache.org/cn/guide/tools/toolkit.html)
![](/assets/006weex-debug4.jpg)

####C.igola调试
- 步骤与playground一致
- 参考 https://weex.apache.org/cn/references/advanced/integrate-devtool-to-android.html
       https://weex.apache.org/cn/references/advanced/integrate-devtool-to-ios.html
        


###二、[真机或模拟器调试](https://www.npmjs.com/package/weexpack)

- 全局安装 weex-pack 命令 ``` npm install -g weexpack```
- 创建weexpack project ```weexpack create [appName]``` 此时会生成[appName]文件夹
- 安装weex应用模版 
进入到[appName]文件夹
```
weexpack platform add ios
```
```
weexpack platform add android
```
注意模版名称均为小写，模版被安装到platforms目录下
- 打开安卓模拟器

    ![](/assets/007weexpack-run.jpg)
    ![](/assets/008weexpack-run2.jpg)
- 打包应用并安装运行
```
weexpack run android
```
 :exclamation: 可能的问题：使用Java7 build可能会报错，请将系统的默认配置改为Java8

你可以更改项目目录下的android.config.json, 文件名则以本地文件的方式加载bundle,url则以远程的方式加载bundle 如果以本地方式指定bundle .we文件请放到src目录。
```javascript
AppName: 应用名
AppId: application_id 包名
SplashText: 欢迎页上面的文字
WeexBundle: 指定的weex bundle文件（支持文件名和url的形式）
```

    ```
    weexpack run ios
    weexpack build ios
    ```
![](/assets/009weexpack-run3.jpg)



##关于架构
###单页or多页
- 采用单页架构，因为涉及到状态管理及路由。在多页架构下，无法使用vuex进行应用状态管理（需要用到storage实现共享），另外多页面路由
需要app扩展

###状态管理
- 采用vuex进行应用状态管理

###路由
- 采用vue-router进行路由跳转，native跳转到hybrid可以在jsbundle后面接参数，如下面的query参数表示跳转到hotel页面

```
// android.config.js
{
    "AppName": "WeexApp",
    "AppId": "com.alibaba.weex",
    "SplashText": "Hello\nWeex",
    "WeexBundle": "http://192.168.15.194:8081/dist/index.weex.js?query=hotel"
}
```
路由跳转实现如下：
```javascript
// entry.js
let instance = new Vue(Vue.util.extend({ el: '#root', router, store }, App))

let query = instance.getQuery('query')
    // console.log(query)
    // 这里实现路由跳转
switch (query) {
    case 'top':
        router.push('/top')
        break
    case 'hotel':
        router.push('/hotel')
        break
    default:
        router.push('/')
        break
}
```

###测试
- 单元测试采用krama+mocha+chai
- 测试用例应该写在test目录下，用例以``` .spec.js```结尾

###语法检查
- 是用eslint进行语法风格检查

###插件
- 插件应该放在``` src/plugins```目录下

```
现有插件:
- i18n
- 
```
### 静态资源
- 静态资源地址（http://192.168.0.82:8181/svn/p1.static/HYBRIDAPP）
- 图标，背景图片等文件都应该放在这个地方


###组件编码规范
-按https://blog.ygxdxx.com/2017/03/09/Vuejs-Component-Style-Guide/ 执行


## 20170619更新

### 全局配置
- 在src/constants里配置
- config.js是配置关键信息的
- common.js是配置普通信息的
- imgurl.js配置了资源信息
使用到多次的参数需要在此处配置

### 说明
- 样式需要按照在页面上从上到下的顺序写,通用的样式写在顶部,方便其他人查看
- export default里的属性按照字母顺序排列,方便他人查看


### ios20px问题 (状态栏20px)
- ios 750*1334的状态栏是40px 按此比例进行换算
- Android的状态栏不能显示内容, iOS的状态栏可以显示内容
- 解决方案: 参照RoomChoose ,用hotel-top-navbar包一下
- 状态栏字的颜色????需要扩展

### z-index使用不了
- 暂时解决方案是:在上面显示的,放在下面
- 有没有其他解决方案

### 动画效果


### 中英文配置
- 在src/mixins/index.js里配置,目前有两处需要改动 中文ZH 英文EN
```
Vue.use(VueI18n)
var i18n = new VueI18n({
    locale: 'ZH',
    messages: locales
})

data() {
    return {
        model,
        routerPage,
        router: {},
        imgUrl,
        platform: weex.config.env.platform.toLowerCase(),
        lang: 'ZH'
    }
},

```
- 使用方法
[国际化使用的是vue-i18n插件](http://kazupon.github.io/vue-i18n/en/)

- 英文单复数问题
```
在EN.js文件中如下定义
 Room: 'Room | Rooms'
使用如下
{{ $tc('Room', 1) }}
{{ $tc('Room', 2) }}
 
```


-中英文样式不一样问题



### 状态管理
- 使用storage进行状态管理,vuex在多页面情形下已不能使用
- 如何初始化缓存


### 目前发现的问题
- 成人和儿童选择 儿童可以选到8人
- 20px问题
- 全局样式没有起作用
- 字体文件很大,不建议使用

### 静态资源管理
- 需要统一使用getSource方法(因为上线时要更改资源路径,否则到时候要大面积改动)

### android 资源下载
- 谷歌被墙,翻墙也经常下不了
- 把下载资源地址改为 http://mirrors.neusoft.edu.cn/android/repository/repository-12.xml

