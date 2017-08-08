##前期准备
- node
- weex-toolkit ``` npm install -g weex-toolkit```
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