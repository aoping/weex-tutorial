##调试 

```
注意在web上显示的效果极有可能在真机上有所不同，所以开发时也要在真机上看效果
```

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

####C.igola调试 (推荐)
- 步骤与playground一致
- 参考 https://weex.apache.org/cn/references/advanced/integrate-devtool-to-android.html
https://weex.apache.org/cn/references/advanced/integrate-devtool-to-ios.html

###二、[真机或模拟器调试](https://www.npmjs.com/package/weexpack)(可以不使用, 需要有native知识)

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

