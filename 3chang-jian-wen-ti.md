
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
- **css相关问题**
不能使用css选择器
padding margin要分开left top等写
元素要设置高度,否则app无法显示高度


## weex playground问题
- playground扫描显示空白
答：手机和电脑要在同一局域网内，https://github.com/alibaba/weex/issues/3002

![](/assets/124734_a91b2758_1345928.png)



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