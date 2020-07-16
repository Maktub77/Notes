### git分支使用说明:

| 分支名称      | 名字           | 说明                                                         | 实例                                                         |
| ------------- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| master        | 线上分支       | 不用于开发，需使用tag功能标记版本。 只能由beta和hotfix合并，合并同时打上发布版本tag | v1.0.2                                                       |
| beta          | 灰度分支组     | 灰度分之只能由test合并master产生，在测试通过后进入灰度阶段产生； 灰度通过后合并进入master | beta/sign                                                    |
| test(release) | 测试分支组     | 只能用于测试和修改bug，只能由由master合并进feature产生。 对于测试通过的test，使用merge合并方式合并master产生beta分支； 合并后的release需要删除 | test/sign release/active                                     |
| feature       | 功能分支组     | 从最新master检出用于开发一个新功能，一旦完成开发， 合并master进入下一个test，删除本次feature分支；负责开发中多开发者代码同步使用 | feature/news feature/vote                                    |
| topic         | 本地开发分支组 | 开发人员基于feature/release/hotfix检出自己本地开发(或修改bug)分支， 在开发(或修改bug)中使用rebase合并方式和feature/release/hotfix进行同步。 原则上一个feature/release/hotfix分支对应一个topic分支， 开发完成的feature/release/hotfix删除对应的topic分支 | topic/feature-news-wlp topic/release-new-wlp topic/hotfix-news-wlp |
| hotfix        | 修补分支组     | 对于线上紧急bug修改，产生一个hotfix分支,只能由master上的tag标签签出。 修改完成的hotfix合并回master，并且必须删除 |                                                              |

------

### 代码提交/合并说明

##### ==注意：这个是开发人在日常开发中使用最多的操作。==

#### 获取代码库

```
//拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改
1. $ git clone <版本库地址> 
//克隆完成后，在当前目录下会生成一个 代码目录
2. $ cd <代码目录>

2.1查看所有分支
git branch --all  
# 默认只有master分支，所以会看到如下两个分支
# master[本地主分支] origin/master[远程主分支]
# 新克隆下来的代码默认master和origin/master是关联的，也就是他们的代码保持同步

3. $ git fetch origin feature/<功能分支>:feature/<功能分支>建立自己的本地开发分支

//切换分支命令:
4. $ git checkout feature/<功能分支>

//git checkout -b (branchname) 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。
5. $ git checkout -b topic/<功能分支>-<你的标识>
```

#### 提交修改

```
//git status 命令用于查看项目的当前状态。
//git status 以查看在你上次提交之后是否有修改。
//"AM" 状态的意思是，这个文件在我们将它添加到缓存之后又有改动。
1. $ git status

//git add 命令可将该文件添加到缓存
2. $ git add .

// git commit 将缓存区内容添加到仓库中。
3. $ git commit -am '修改描述'
```

#### 发布你的修改

```
1. $ git fetch origin feature/<功能分支>:feature/<功能分支>
2. $ git rebase feature/<功能分支>
3. $ git push origin topic/<功能分支>-<你的标识>:feature/<功能分支>
```

------

### 代码发布说明

发布代码是针对功能发布而定的，发布又分为测试发布和上线发布。对于发布操作，必须是先到测试环境(test)，再从测试环境(test)到灰度环境(beta)，最后从灰度环境(beta)到生产环境(master)，对于线上每次发布都必须有标签记录，可以回退。 原则上从beta到master只会产生 fast-forward 类型操作。以下所有操作都在自己的开发分支中完成。

#### 发布到测试环境

```
1. # 合并feature分支
2. $ git fetch origin master:master
3. $ git fetch origin feature/<功能分支>:feature/<功能分支>
4. $ git checkout feature/<功能分支>
5. $ git merge master
6. ~~解决冲突~~
7. # 生产test分支
8. $ git checkout -b test/<功能分支>
9. $ git push origin test/<功能分支>: test/<功能分支>
10. # 清理feature分支
11. $ git push origin :feature/<功能分支>
12. $ git branch -D feature/<功能分支>
```

#### 发布到灰度环境

```
1. # 合并master到测试
2. $ git fetch origin test/<功能分支>:test/<功能分支>
3. $ git fetch origin master:master
4. $ git checkout test/<功能分支>
5. $ git merge master
6. ~~解决冲突~~
7. # 生成beta分支
8. $ git checkout -b beta/<功能分支>
9. $ git push origin beta/<功能分支>:beta/<功能名称>
10. # 清理 test
11. $ git push origin :test/<版本>
12. $ git branch -D test/<版本>
```

#### 发布到生产环境

```
1. # 合并到master
2. $ git fetch origin beta/<版本>:beta/<版本>
3. $ git fetch origin master:master
4. $ git checkout master
5. $ git merge beta/<版本>
6. $ git tag -a <发布版本号> -m "发布功能描述"
7. $ git push origin --tags
8. $ git push origin master:master
9. # 清理 beta
10. $ git push origin :beta/<版本>
11. $ git branch -D beta/<版本>
```

#### 修改生产环境bug

```
1. # 创建补丁版本，进行修改
2. $ git fetch origin --tag
3. $ git checkout -b hotfix/<版本号> <版本号>
4. # 修改完成发布
5. # 1. 合并到master
6. $ git fetch origin master:master
7. $ git checkout master
8. $ git merge hotfix/<版本号>
9. $ git tag -a <发布版本号> -m "发布功能描述"
10. $ git push origin --tag
11. $ git push origin master:master
12. # 清理 hotfix
13. $ git push origin :hotfix/<版本号>
14. $ git branch -D hotfix/<版本号>
```

#### Git中从远程的分支获取最新的版本到本地

```git
$ git status
$ git fetch origin feature/v1.1.0:feature/v1.1.0
$ git rebase feature/v1.1.0
```

#### 清理远程已删除本地还存在的分支 

```
git fetch --prune origin 
或者 
git fetch -p 
或者 
git pull -p 
```

