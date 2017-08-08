## 资料
- 酒店设计稿 //192.168.0.200/share/diandian_design/app/Hotel
- 酒店设计稿(改) //192.168.0.200/share/FANG _design/酒店改
- hybrid-native对接文档     https://git.oschina.net/igolatech/iGola_docs/tree/master/hotel-hybrid-web
- 静态资源与接口 (见/src/constants/imgurl.js)

```javascript

let baseUrl = 'http://static.dev.gz.yougola.com:8080/HYBRIDAPP/' // 静态资源
let apiGatewayHost = 'http://gateway.dev.gz.yougola.com:8000' // 接口

const env = weex.config.env.appEnv

switch (env) {
    case 'dev':
        baseUrl = 'http://static.dev.gz.yougola.com:8080/HYBRIDAPP/' // 静态资源
        apiGatewayHost = 'http://gateway.dev.gz.yougola.com:8000' // 接口
        break
    case 'sit':
        baseUrl = 'http://static.dev.gz.yougola.com:8080/HYBRIDAPP/' // 静态资源
        apiGatewayHost = 'http://192.168.0.164:8000' // 接口
        break
    case 'uat':
        baseUrl = 'http://static.dev.gz.yougola.com:8080/HYBRIDAPP/' // 静态资源
        apiGatewayHost = 'http://172.16.10.41:8000' // 接口
        break
    case 'prod':
        baseUrl = 'https://static1.igola.com/HYBRIDAPP/' // 静态资源
        apiGatewayHost = 'https://api.igola.com:9006' // 接口
        break
    default:
        baseUrl = 'http://static.dev.gz.yougola.com:8080/HYBRIDAPP/' // 静态资源
        apiGatewayHost = 'http://gateway.dev.gz.yougola.com:8000' // 接口
        break
}


``` 

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


##架构

###单页or多页
- 参见webpack打包 ``` /webpack.config.js````

- web采用单页架构，因为涉及到状态管理及路由。
- native采用多页架构下
- native环境下有页面回退(iOS右滑或Android回退键), 采用单页面的话会一次退出, 而且如果单页面随着业务的迭代, jsbundle势必越来越臃肿, 不利于加载(导致出现白屏)

###状态管理 ``` /src/services/hotel-data.js```
- 采用vuex进行应用状态管理(多页面不适用)
- 用storage管理

###数据传递
#### 多页面间
storage (参见/src/services/hotel-data.js)
query参数 (参见/src/views/HotelIndex.vue)
```
this.router.push({
  page: this.routerPage.hotelResult,
  query:{
    city: this.formData.city.code,
    cityName: this.formData.city.name,
    checkInDate: this.formData.checkInDate,
    checkOutDate:this.formData.checkOutDate,
    keyword:encodeURI(this.formData.keyword),
    adult:this.formData.adult,
    children:this.formData.child,
    roomCount:this.formData.room,
    childrenAge:this.formData.childAge.join(',').replace(/<1/g, '0')
  }
})
```
broadcastchannel(暂未使用, 因为在使用时报错"broadcastchannel未定义")


#### 单页面内(与Vue web开发一直, 参见Vue api)
Vue.$emit


###路由 
#### 路由模块(路由模块实现跳转参见/src/utils/modules/navigator.js)

- web采用vue-router进行路由跳转
- ios采用weex提供的navigator模块
- android 采用自定义模块```myNavigator```跳转, 因为igola Android采用的是fragment架构, weex提供的navigator只支持activity的跳转
- 嵌套路由回到首页调用 popToHotelFrontPage方法

``` javascript
// 如果是igola就返回首页, 其他只是pop
function popHomePage() {
    if (weex.config.env.appName.toLowerCase() == 'igola') {
        myNavigator.popToHotelFrontPage()
    } else {
        navigator.pop({
            animated: 'true'
        }, event => {
            console.log('callback: ', event)
        })
    }
}
```

#### 路由定义(路由定义参见/src/router/)

```javascript
page.js(供navigator用)

hotel: {
    title: '酒店',    // 页面title
    path: '/hotel',    // web路由
    jsPath: 'views/HotelIndex'    // jsbundle路由
}

```

```javascript
map.js(供vue-router用)
const hotel = require('views/HotelIndex')    //加载模块
{ path: '/hotel', component: hotel }    //定义路由

```

### 数据刷新(参见/src/views/HotelIndex.js)
- weex支持viewappear生命周期
- ios首页不能触发(其他深层页面可以), 需要ios发送一个fireEvent通知
- igola android目前是通过pop时发送globalEvent通知


###组件编码规范
-按https://blog.ygxdxx.com/2017/03/09/Vuejs-Component-Style-Guide/ 执行

### 全局配置
- 在src/constants里配置
- config.js是配置关键信息的
- common.js是配置普通信息的
- imgurl.js配置了资源信息
使用到多次的参数需要在此处配置


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

###测试
- 单元测试采用krama+mocha+chai
- 测试用例应该写在test目录下，用例以``` .spec.js```结尾

###语法检查
- 是用eslint进行语法风格检查

