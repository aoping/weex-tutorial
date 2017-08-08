## 发布

### 1. 修改版本号
按照版本规范修改package.json里的version字段

### 2.提交代码到app仓库

#### android发布
1. 拉取igola-android最新代码
2. 切换到``` hybrid```分支
3. 把``` /dist```文件夹拷贝到``` \igola-android\app\src\main\assets``` 目录下
4. 提交并推送到远程仓库

#### ios发布
1. 拉取igola-app-ios最新代码
2. 切换到ios最新的分支分支(```注意iOS分支经常会变动,可能需要问一下```)
3. 把``` /dist```文件夹里面的文件拷贝到``` \igola-app-ios\bundlejs``` 目录下(```注意与Android不大一样,不需要dist文件夹```)
4. 提交并推送到远程仓库

### 3.通知相关负责人