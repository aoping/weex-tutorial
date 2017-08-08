## 环境
### 区分iOS Android web (参见/src/mixins/index.js)

```
platform: weex.config.env.platform.toLowerCase()
this.platform === 'web'
this.platform === 'android'
this.platform === 'ios'

```

### 区分dev sit uat 生产(参见/src/constants/imgurl.js)

```
weex.config.env.appEnv === 'dev'
weex.config.env.appEnv === 'sit'
weex.config.env.appEnv === 'uat'
weex.config.env.appEnv === 'prod'

```

### 区分igola和其他(参见/src/utils/modules/navigator.js)

```

weex.config.env.appName.toLowerCase() == 'igola'
```

## 样式注意点
- 宽度 高度 margin 用百分比% 在app端都会失效
- ios状态栏40px(750px)
  - 根据设备加上40px(参见/src/components/hotel-home-top.vue)
- 安卓机一屏问题 
  - 可获取设备高度进行处理(weex.config.env.deviceHeight)
- 元素高度问题 
- gif图片仅Ios支持，android需单独添加支持库
  - 安卓进行扩展或使用支持gif的图片库
- div无法渲染文本需使用<text>
2. 不支持富文本，需native单独封装module
3. 不支持z-index
  - 层级靠后优先
4. class表达式需使用数组形式，否则会报错
5. css无法简写（例:padding: 0 0 0 0 不支持，需padding-left padding-right各写属性）
6. 不支持dom操作
7. weex原生滑动事件不支持嵌套<list>和<scroller>标签
8. 使用vue-router会出现跳转僵硬，如使用跳转动画需单独打包各个vue成jsbundle，用navigtor跳转，但单独打包后无法使用vuex
9. 添加ios平台后需将podfile文件ATSDK-weex改为ATSDK否则会报错
11. 使用v-if会有闪屏现象
    - 推荐使用opacity显示(opacity为0时android会隐藏层级，但ios不会，需做判断)
12. weex全局变量无法在vue模板渲染使用,可在vue实例中data添加
13. 不支持百分比，宽度默认750px
14. 图片必须定义高度宽度，否则无法显示
15. 长列表分页渲染会出现内存泄漏
- iconfont问题
  - 支持ttf和woff字体格式
  - 使用unicode
  - 字体名要小写
  - 首页最好不要用iconfont,会有一个渲染失败的问题


### 事件问题
1. image load事件在Android上必须设置width height才会触发\
2. slider 在Android iOS上没有index属性, 所以不能设置想要显示第几张图片, 需要采用稍微绕一点的方式实现, 可参考HotelDetailPics PicsPreviewer的实现
3. weex 中Android的v-model双向绑定 输入第一个字符时无响应

    用v-bind 和@change代替, 此外还要使input失去焦点, 还要再延时才会更新到数据



## 其他问题
- 数组数据变化，dom不立即更新
  - 使用特定的数组操作方法(vue.set)相关链接
  - 使用setTimeout看是否跟生命周期有关


##  android问题

-  **build tools版本问题** 

可以在app文件夹里的build.gradle改成自己已安装的tools版本，顶层的build.gradle不能有android {}

-  **device supports x86,but APK only supports armeabi** 

如果你和我用的开发工具都是 Android Studio 的话，那么在程序的主module 的 build.gradle中你会发现有这么个代码:

```
 ndk {
     abiFilters "armeabi"
 }
```
注释掉这段代码

 **参考链接** 
[http://blog.csdn.net/qq_32452623/article/details/71076023](http://blog.csdn.net/qq_32452623/article/details/71076023)
-  **Gradle sync failed** 

```
Gradle sync failed: Cause: org.gradle.api.internal.tasks.DefaultTaskInputs$TaskInputUnionFileCollection cannot be cast to org.gradle.api.internal.file.co
```
参考链接 http://blog.csdn.net/rodulf/article/details/50679928

-  CreateProcess failure, error 2 * could not start server *

参考链接 http://blog.csdn.net/figosoar/article/details/41357421


## weex android问题
-  **图片显示不了** 
在ImageAdapter里加入如下代码
```
import com.squareup.picasso.Picasso;
 Picasso.with(view.getContext()).load(url).into(view);//获取网络图片
```
![](/assets/124734_a91b2758_1345928.png)


## weex playground问题
- playground扫描显示空白
答：手机和电脑要在同一局域网内，https://github.com/alibaba/weex/issues/3002






`ios, android 端` 以下简称原生端或native端

###样式问题
weex 的样式支持是 css 的一个小子集，有很多属性不能支持。请细读 [weex 通用样式](https://weex.apache.org/cn/references/common-style.html)

完全按照官网上的手册编写样式，可能还会遇到以下问题：
1. 样式在 web 端完好，在 native 端错乱 .  
```
检查：
a. weex 里元素默认是， display: flex, position: relative , 做相应检查
b. weex 的内建标签，对样式有特殊要求，做相应检查
```

2. ios 端跟 android 端样式不一致  // 这个也是可能出现的，开发时尽量做好测试

3. 图文混排， 请查阅 [weex richtext](https://github.com/alibaba/weex/issues/835), [weex richtext2](https://github.com/alibaba/weex/issues/834)


###native 端调试
1. 一般来说在 android studio 或 xcode 下使用模拟器调试，可以在 log 里看到由 weex 抛出的错误，这部分错误信息能解决部分问题。

2. 另外需要 native 端的同事集成 weex 的调试工具， [weex devtools](https://weex.apache.org/cn/references/advanced/integrate-devtool-to-android.html)

3. android studio 用安卓模拟器跑的时候，有可能会遇到
`device supports x86,but APK only supports armeabi`

将 build.gradle(app) 里的这段代码注释掉即可
![](/assets/161518_66a2c5ea_617787.png)


###weex组件

参考 [hacksnew的例子](https://github.com/Hanks10100/vue-snippets)

不能使用css选择器

padding margin要分开left top等写

元素要设置高度,否则app无法显示高度











### ios20px问题 (状态栏20px)
- ios 750*1334的状态栏是40px 按此比例进行换算
- Android的状态栏不能显示内容, iOS的状态栏可以显示内容
- 解决方案: 参照RoomChoose ,用hotel-top-navbar包一下
- 状态栏字的颜色????需要扩展

### z-index使用不了
- 暂时解决方案是:在上面显示的,放在下面
- 有没有其他解决方案

### 动画效果

### 目前发现的问题
- 成人和儿童选择 儿童可以选到8人
- 20px问题
- 全局样式没有起作用
- 字体文件很大,不建议使用

### android 资源下载
- 谷歌被墙,翻墙也经常下不了
- 把下载资源地址改为 http://mirrors.neusoft.edu.cn/android/repository/repository-12.xml



##待解决问题
1. app和h5调用原生方法时兼容问题(解决方案:判断平台)
2. 需要扩展哪些组件模块(需要看需求三端共同讨论)
3. 数据传输 用户认证 数据安全等问题(Lewis觉得没问题)
4. 性能问题,如POC中api调用结果返回缓慢(要安装APP后看是接口问题还是weex的问题)
5. IOS不支持热更新(集成到APP里)
6. 之前h5,小程序,app开发好的组件,样式,功能等怎么复用
7. 支付如何调用
8. 架构设计需要考虑哪些问题,如调试等
9. 如何沿用 iconfont 字体图标
10. 包大小问题