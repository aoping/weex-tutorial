## 分支管理

```
https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-gitflow.md
```
![](/assets/git-workflow-release-cycle-4maintenance.png)

### 历史分支
- master    存储了正式发布的历史
- dev    开发分支,功能的集成分支

### 功能分支
- feature    每个新功能位于一个自己的分支
    - 每个sprint都应该新开一个feature进行开发, 再合到dev分支
    - 分支命名feature-1.1.0
        - 第1位 - 大版本  (重大功能及重大技术架构升级)
        - 第2位 - 发布顺序
        - 第3位 - 统一为0
    

### 发布分支
- release    一个用于发布准备的专门分支

### 维护分支
- hotfix    唯一可以直接从master分支fork出来的分支


## 版本管理

发布必须是master分支上的代码, 每次发布必须在master打tag,发布需要通知负责人

tag命名规范:
- 命名：v1.0.0
    - 第1位 - 大版本  (重大功能及重大技术架构升级)
    - 第2位 - 发布顺序
    - 第3位 - 统一为0


