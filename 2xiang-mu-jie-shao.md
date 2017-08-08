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

##关于架构
###单页or多页
- 采用单页架构，因为涉及到状态管理及路由。在多页架构下，无法使用vuex进行应用状态管理（需要用到storage实现共享），另外多页面路由
需要app扩展

###状态管理(多页面不适用)
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

### 全局配置
- 在src/constants里配置
- config.js是配置关键信息的
- common.js是配置普通信息的
- imgurl.js配置了资源信息
使用到多次的参数需要在此处配置

### 说明
- 样式需要按照在页面上从上到下的顺序写,通用的样式写在顶部,方便其他人查看
- export default里的属性按照字母顺序排列,方便他人查看

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

### 状态管理
- 使用storage进行状态管理,vuex在多页面情形下已不能使用
- 如何初始化缓存


### 静态资源管理
- 需要统一使用getSource方法(因为上线时要更改资源路径,否则到时候要大面积改动)


