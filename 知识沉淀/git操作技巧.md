# 切换分支

在wpsmain文件夹里面运行这个命令：

```
#切换分支
krepo-ng checkout func_pdf_i18n_20250928_branch
#再运行这个命令进行重编
krepo-ng build --force-rebuild
```

如果是需要编译release版本：

```
#需要使用这个命令构建release
krepo-ng config --new --build-type release --cmake-argn="-DWPS_PLUGIN_TEST=1 -DWPS_CXX_OPT_NONE=1 -DWPS_DISABLE_PERMISSIVE=ON"
#然后再编译
cd release
krepo-ng build
```

# 代码改动

## stash技巧（更新代码前暂存改动）：

先进入对应的bundle目录里面，打开cmd，然后运行下面这行代码：

```
git stash push -m "临时保存：更新代码前的修改"
//或者简单写
git stash

//可以运行这个看是否stash成功
git stash list

//如果你看到类似这样的输出，说明成功：
//stash@{0}: On func_pdf_i18n_20251106_branch: 临时保存：更新代码前的修改
//也可以进一步确认文件确实恢复为干净状态： git status
//如果输出是： working tree clean
```

然后运行krepo-ng sync去更新代码

更新完后，运行下面命令拉回改动：

```
//恢复但保留 stash 记录：
git stash apply
//恢复并删除 stash 记录:
git stash pop

//验证取回成功
git stash list

//如果你用了 apply，列表里还会有那条 stash。
//如果你用了 pop，那条 stash 会被删掉。
//再执行：git diff
//可以看到恢复的代码改动内容。
```

## commit技巧

看上次commit信息，有下面几种方式（需要在对应bundle下面操作）：

```
//只看最近一次提交概要
git log -1
//看详细改动内容（包括文件差异）
git show  或者  git show HEAD
//想看上上一次（上一个）
git show HEAD~1  或者  git log -2
```

撤回上一次commit的操作：

```
//只是想“改一改”上次 commit（比如信息写错），这会打开编辑器，让你修改 commit message。
git commit --amend
//或者直接运行这个
git commit --amend -m "修改后的信息"
//想彻底撤销上次 commit（但保留代码改动）
git reset --soft HEAD~1
//想彻底撤销上次 commit（连修改的文件也回滚）
git reset --hard HEAD~1
```

## 提交流程

先commit代码改动，可以在fork软件里面提交，这样操作就行：

![](https://cloud-pic.wpsgo.com/V04xZzJkenRVZktzZ0RqRXNVOHdkVloyQ2h4bTFDa1BkRXFGS1c2aktZVVNyZEtkVjh3U3ZWREZCb2VPZmZ5dGhEcXcwYzJhUVppcUZMZ2VHWWxzd1FJcnJUZS9MWWZHajN4eXd1UEFaWFM4V1BvOFRCeU1xWFBQUVExZ1lxRTZrd3VPWTdMLzRwYWprdyszb1l6cjExWTRNM2g5RFMrQjRDMVpkbU1XWDVLN2FKUWllRkt0MDhOWmtrSnhoZTlzTjhhdERrdVZnbG9aMVhMVkZOeHFEUjQxV3V4Rk8xWXE2dHJHc29vSCs2bEQrV0xxMEpLaHVuTGQ4cUFab0NhS3ZCYSsyUFkrY1l4L09mNDlRVk1IRnc9PQ==/attach/object/F4EUTRJDADQFY?)

如果是多个commit，可以通过一次cr提交，只需要把不同的bundle拖进来，然后和上面一样操作。然后下面是commit的信息规范（下面是三个commit），主要注意“原因”和“改动”和“影响面”这三个栏目，至于标题就随便写吧:

🔔

【项目：2C国际化】【ones：2229362】插入空白页接入kd控件

【原因：插入空白页适配国际版】

【改动：插入空白页接入kd控件】

【影响面：插入空白页界面是否与UI稿一致】

【模块：pdfpageoperator】【公共模块：否】

【影响平台：不区分平台】

【影响配置：不区分配置】

【项目：2C国际化】【ones：2229362】导入页面控件类型及间距与ui稿不符

【原因：控件类型及间距与ui稿不符】

【改动：1.修改控件类型为对应控件；2.调整控件间距】

【影响面：导入页面界面是否与UI稿一致】

【模块：pdfadvancedtool】【公共模块：否】

【影响平台：不区分平台】

【影响配置：不区分配置】

提交commit后，需要更新代码（在coding同级目录下打开cmd，运行krepo-ng sync），这时候需要确保自己代码的改动都已经commit上去了。如果有改动没有commit，而且也还需要留下这部分改动，可以参考stash技巧。

更新完代码后，如果没出问题，依旧是这个目录，运行krepo-ng cr来提交cr。交cr需要配置cppcheck并添加下载路径到环境变量。

cr通过后，还需要更新一次代码，依旧是这个目录，运行krepo-ng sync。如果没问题，就到了最后的一布——提交代码。还是这个目录，运行krepo-ng push。如果不确定自己有没有push成功，可以运行git status看看有没有commit在工作区放着，如果没有，那就是成功了。一个完整的提交代码流程就是这样了。

# Fork操作技巧

## 提交代码

可以看2.3的提交流程

## Commit了，但是cr没过，需要修改commit信息或者代码

如果是前一次的commit，直接点击ammend，并把修改的代码合并进去就行了。

如果提交了多个commit都没有cr，需要撤回某一次的commit，可以利用commit reset命令

## 抵消某个commit 的改动

如果发现代码已经推送，但是这个地方需要使用原来的版本，这时候可以使用Revert Commit进行版本回滚。

# git配置

## 查看

```
#查看当前仓库的提交身份
git config user.name
git config user.email
#查看所有生效配置（重要）
git config --list
git config --list --show-origin
#查看远程仓库地址
git remote -v
```

## 配置

”origin“这个名字可以自己设置，一般都是叫origin（远程仓库的名字）

```
#配置提交身份：全局（默认身份）
git config --global user.name "YourName"
git config --global user.email "you@gmail.com"
#当前仓库（覆盖全局）
git config user.name "CompanyName"
git config user.email "you@company.com"
#添加 / 修改 / 删除远程仓库
git remote add origin <url>  //添加
git remote set-url origin <url>  //修改
git remote remove origin  //删除
#推送（第一次）
#推代码前要add和commit，然后git branch看看分支名是main还是master
git push -u origin main  //第一次需要加-u origin main，后面就不用了
//如果此时你使用的是https方式推送代码，第一次推送git会需要你输入用户名和密码
用户名：你的 GitHub 用户名
密码：粘贴刚才生成的 Token
```

## 单文件夹配置

可以给单文件夹配置.gitnore，使得该文件夹下面的git项目提交代码时使用的name，email信息为自己账户的信息。并配置当前文件夹下面提交项目使用token走https提交。

```
#编辑全局配置
git config --global --edit
#在下面加上
[includeIf "gitdir:D:/nian/"]  //这里可以用自己的文件夹
    path = D:/nian/.gitconfig
#新建目录专属配置文件：D:\nian\.gitconfig，并在里面加入这些信息
[user]
    name = XiaoIcespirit  //你的user名字
    email = your_personal_email@gmail.com  //你的邮箱
[url "https://github.com/"]
    insteadOf = git@github.com:  这个不用改
```

## 提交前自检

```
#推送前快速确认
git config user.email
git remote -v
git config --show-origin --get user.email
#判断是不是 Git 仓库
git status
```

# 从零开始开发项目

## 配置环境

```
#配置提交身份：全局（默认身份）
git config --global user.name "YourName"
git config --global user.email "you@gmail.com"
#当前仓库（覆盖全局）
git config user.name "CompanyName"
git config user.email "you@company.com"
```

## 初始化git

这里会有个坑，在github创建仓库时，仓库分支默认是main分支。但是现在的git初始化时，分支名默认是master。所以在把项目推送到远程时，会出现两个分支的情况（origin/main分支为空，但是origin/master分支为你的代码）

```
#在项目路径下打开cmd
git init
#配置远程环境
git remote add origin "仓库地址"
#验证远程环境是否配置成功
git remote -v
#推代码到远程(记住要编写.gitnore)
git add .  //添加代码到暂存区，也可以一个个文件添加）
git commit -m "Initial commit"
git push -u origin main
————————————————————————其他命令———————————————————————————————
#取消暂存
git restore --staged .
#查看当前add的文件
git status
#丢弃对这个文件的修改，让它恢复到最近一次提交时的状态
git restore 文件名
```

解决方案：

1. 按照git时，好像有个选项是问你以后初始化仓库的默认分支名是main还是master。选main就行了；
2. 如果已经运行了git init命令初始化了git配置，先运行git branch看看分支名是什么，如果是master（非main），就可以运行git branch -M main命令进行强制改名；
3. 如果你是master分支并且已经push代码了。需要按照如下命令进行清除：

```
#先修改当前分支名为main
git branch -M main
#先把远程 main 拉下来
git pull --rebase origin main  //可能会有冲突
#再push
git push -u origin main
```

目前为止，应该保证本地和远程都有一个main分支并且代码存在了。

## 开发

目前主流开发规范是三种分支：main(主干 / 稳定 / 可发布)，develop(日常开发)，feature(功能分支)。一般都是在develop上开发，再合并到主干上。相关命令如下

```
#首先切换分支到main分支
git checkout main
#基于main创建dev分支
git checkout -b dev //从当前 main，复制一份出来，叫 dev，并切过去
#把dev推送到Github
git push -u origin dev //-u的作用是：以后在 dev 上 git push，默认就是推到 origin/dev
```

如果需要写一个独立功能时，再用feature分支。比如：

```
#先切到dev分支
git checkout dev
#基于dev分支创建新分支
git checkout -b feature/music-player  //存疑（没试过）
#合并回 dev
git checkout dev
git merge feature/music-player //然后git push
#删除 feature 分支（干净）
git branch -d feature/music-player
```

## 合并分支

两种方法：命令行merge和github提交PR（PR流程如下）。

![](https://cloud-pic.wpsgo.com/V04xZzJkenRVZktzZ0RqRXNVOHdkVloyQ2h4bTFDa1BkRXFGS1c2aktZVVNyZEtkVjh3U3ZWREZCb2VPZmZ5dGhEcXcwYzJhUVppcUZMZ2VHWWxzd1FJcnJUZS9MWWZHajN4eXd1UEFaWFM4V1BvOFRCeU1xWFBQUVExZ1lxRTZrd3VPWTdMLzRwYWprdyszb1l6cjExWTRNM2g5RFMrQjRDMVpkbU1XWDVLN2FKUWllRkt0MDhOWmtrSnhoZTlzTjhhdERrdVZnbG9aMVhMVkZOeHFEUjQxV3V4Rk8xWXE2dHJHc29vSCs2bEQrV0xxMEpLaHVuTGQ4cUFab0NhS3ZCYSsyUFkrY1l4L09mNDlRVk1IRnc9PQ==/attach/object/6JD525JEAAQA2?)

记住新分支合并好后，需要运行这个命令才能更新代码："git pull origin 分支名"

# 参考文档

[https://365.kdocs.cn/l/coiRMfPJElIE?source=wiki](https://365.kdocs.cn/l/coiRMfPJElIE?source=wiki "Git 常用命令介绍.otl")