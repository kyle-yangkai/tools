***前沿：***
*1. 本文是学习* **廖雪峰git教程** *时的一些笔记，大家想看详细版请阅读[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*
*2.本文中使用到的系统版本为windows10*

## 一. 安装git
1. 下载安装程序 -->[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)

2. 安装

3. 安装完成，在开始菜单中打开`Git --> Git Bash`

4. 在打开的窗口中输入配置信息
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 二. 创建版本库
***相关知识点：***
*此处用到一些命令行语句`mkdir git`-->创建一个名字为git的文件夹；`cd git`-->进入git文件夹 `pwd`-->查看当前所在位置*
1. 创建gitlearn文件夹作为版本库保存地址（请确保地址没有中文）
```
$ mkdir gitlearn
$ cd gitlearn
$ pwd
/Users/michael/gitlearn
```

2. 初始化版本库
```
$ git init
Initialized empty Git repository in /Users/michael/gitlearn/.git/
```

## 三. 版本库文件的增删改查与版本选择
***相关知识点：***
*1. **工作区：**当前的`gitlearn`文件夹即为一个工作区*
*2. **版本库：**`gitlearn`文件夹中隐藏的`.git` 目录为版本库（其不算工作区的内容）*
*3. **暂存区：**`.git`目录里面的`stage`文件（或者时`index`文件）*
*4. 暂存区存放的时`add`命令添加的文件，通过`commit`命令提交更改后，暂存区的内容就被提交到当前分支*

1. 新增文件
    1. 将文件放入gitlearn文件夹下（一定要是这个文件夹下，不然git会找不到文件）
        **两种方法**
        - 将编辑好的文件直接放入gitlearn文件夹下
        - 使用`vim readme.txt`  创建一个readme.txt文件并可以直接编辑文本内容，编辑完成后按`esc`,`:`,然后键入`wq`即可保存文件并跳出vim模式进入ex模式
    2. 使用`git add`命令
    ```
    $ git add readme.txt
    ```
    3. 使用`git commit`命令
    ```
    $ git commit -m "wrote a readmw file"    /*-m 后面输入的是本次提交的说明*/
    ```
    **如果希望一次性添加多个文件可以使用以下命令**
    ```
    $ git add file1.txt
    $ git add file2.txt file3.txt
    $ git commit -m "add 3 files."
    ```

2. 文件修改
    需要`git add`和`git commit`命令，与添加文件操作一样

3. 文件查询
```
/*相关命令*/
git status  -->仓库当前的状态
git diff   -->当前版本与上次提交的版本之间的不同
git log  -->历史提交版本信息
git reflog -->历史版本操作记录
```

4. 文件删除
    1. 使用`git rm`命令（或者直接在工作区中将文件删除）
    ```
    $ git rm readme.txt
    ```
    2.  使用`git commit`命令
    ```
    $ git commit -m "remove readme.txt"    
    ```

5. 撤销修改
    1. 还未`add`的修改撤销（需要撤销修改的文件还没有放到暂存区）
     ```
    /*撤销`readme.txt`的修改(-- 很重要，而且前后都要有空格）*/
    $ git checkout -- readme.txt 
    ```
    2. 已经`add`但是还没有`commit`的修改撤销
    ```
    /*将暂存区对readme.txt的修改退回到工作区*/
    $ git reset HEAD readme.txt
    /*撤销工作区中`readme.txt`的修改*/
    $ git checkout -- readme.txt 
    ```
    3. 已经`commit`的修改撤销（提交到本地库）
    ```
    /*退回到上个版本*/
    $ git reset --hard HEAD^
    ```
    4. 已经推送到远程版本库
    **无法在本地撤销！！！**

6. 版本选择
    * `$ git reset --hard HEAD^` -->退回上个版本
    * `$git reset --hard 1094a`  -->退回到版本号中带有`1094a`的版本（通常都是版本号前几位数字）

## 四. 远程版本库
*创建远程版本库的目的是为了备份，同时充当主机服务器的作用*
1. 创建（使用github作为远程仓库）
    1. 创建SSH key（使用Git Bash）
    ```
    /*使用以下命令然后一路回车（默认无密码）*/
    $ ssh-keygen -t rsa -C "youremail@example.com"
    ```
    以上操作成功后，在用户主目录里面找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人
    2. 登录[github](https://github.com/)，打开“Account settings”，“SSH Keys”页面；点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容；然后点击“Add Key”，完成！

2. 将本地库推送到远程库
    1. 创建新git库
       登录github，Create a new repository
    2. 复制git库SSH地址 `git@github.com:kyle-yangkai/gitlearnning.git `（也可以使用https地址）然后输入命令
       ```
       $ git remote add origin git@github.com:kyle-yangkai/gitlearnning.git
       ```
    3. 将本地库所有内容推送到远程库中
       ```
       $ git push -u origin master  /*-u只需要第一次推送的时候使用 如果要推送到远程的dev分支就将master换成dev*/
       ```
    4. 新建远程分支
        ```
        $ git push origin <当前分支>:<新分支名>
        ```
    5. 删除远程分支（两种方法）
        * `$ git push origin :<name>` -->推送空分支到远程分支
        * `$ git push origin --delete <name>` -->直接删除远程分支
3. 从远程库克隆到本地库
```
$ git clone git@github.com:kyle-yangkai/gitlearnning.git
```
## 五. 分支管理
1. 一次完整的分支操作
    ```
    /*相关命令*/
    $ git branch -->查看分支
    $ git branch -d <name> -->删除分支 （如果没有合并过请使用 -D 强制删除）
    $ cat readme.txt --> 查看readme.txt中的内容
    $ git branch <name> -->创建分支
    $ git checkout <name> -->切换到分支
    $ git checkout -b <name> --> 创建 并切换分支
    $ git merge <name> --no--ff --> 合并分支到当前分支( --no--ff 为普通模式合并，不加该参数则为快速合并，      历史没有该合并记录）
    $ git branch -d <name> --> 删除分支
    ```
    1. 创建分支
    ```
    /*git checkou 命令加上 -b 表示创建并切换 相当于$ git branch dev,  git checkout dev这两条命令  */
    $ git checkout -b dev
    ```
    2. 修改分支内容并提交（同上面版本库文件修改步骤）
    3. 切换回`master`分支并将`dev`分支的内同合并到`master`上
    ```
    $ git merge dev
    ```
    4. 删除`dev`分支
    ```
    $ git branch -d dev
    ``` 
     ***注意：***
*1. 各分支之间其实没有任何差别，任何分支都可以称为主分支，我们一般默认master分支为主分支，最终版本都会合并到master分支中*
*2. 如果两个分支在同一个文件中都做了不同的修改，那么此时合并操作不会完成，我们需要手动修改该文件，通过`$ git status`查询冲突文件，然后通过`$ cat <file>`查看冲突内容，修改完成后再进行合并操作*

2. bug分支处理
      ```
    /*相关命令*/
    $ git stash-->保存当前还未提交的工作状态，恢复到最近一次提交的状态
    $ git stash list --> 查看当前stash保存的状态列表
    $ git stash apply --> 恢复上次stash保存的状态
    $ git stash pop -->恢复上次stash保存的状态并删除该次stash状态
    $ git stash drop --> 删除上次保存的状态
    $ git merge <name> --no-ff --> 合并分支到当前分支( --no-ff 为普通模式合并，不加该参数则为快速合并，      历史没有该合并记录）
    $ git branch -d <name> --> 删除分支
    ```
    1. 使用`git add -A`暂存当前`dev`分支新修改和新增的还不能提交的内容
    2. 使用`git stash`保存当前状态
    3. 确定需要修复的分支，如果是`maste`分支，先返回到`master`分支
    4. 创建`issue-001`分支，完成修复不过的代码提交
    5. 通过master合并完成bug修复
    6. 返回`dev`分支通过`$ git stash apply`恢复之前的状态，急需工作
## 六. 标签管理
1. 创建标签
    * `$ git tag v1.0` --> 给当前分支打标签v1.0 该标签默认搭载最新提交的commit上面
    * `$ git tag v0.9  f24sweq` -->给commit id是`f24sweq`的提交打上标签v0.9
    * `$ git tag -a v0.1 -m "explain" f24sweq` -->`-a`为标签名 `-m`为说明文字
2. 查看标签
    * `$ git show v0.1` -->查看该标签的说明文字
3. 操作标签
    1. 本地标签操作
        * `$ git tag -d v0.1` -->删除本地v0.1标签
    2. 远程标签操作
        * `$ git push origin v1.0` -->将v1.0标签推送到远程
        * `$ git push origin --tags` -->推送所有本地标签到远程
        * `$ git push origin :refs/tags/v0.9` --> 删除远程v0.9标签
## 七. 自定义Git
1. 忽略特殊文件
2. 配置别名
    * 全局配置 `$ git config --global alias.<自定义命令> <git命令>` 例子-->`$ git config --global alias.ci commit`
    * 当前仓库配置 --> 每个仓库的Git配置文件都放在.git/config文件中，在该文件中加上以下内容
    ```
    [alias]
      co = checkout
      ci = commit
      br = branch
      st = status
    ```
    
    
