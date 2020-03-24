# git学习
## 1.了解Git和Github
### 1.1 什么是GitHub
Github是全球最大的社区编程及代码托管网站( [https://github.com](https://github.com/) )
Github可以托管各种git库，并提供一个web界面( 用户名.github.io/仓库名 )
### 1.2 Github与Git是什么关系
Git是版本控制软件
Github是项目代码托管平台，借助git来管理项目代码
### 1.3 为什么学习github
学习优秀的开源项目
关注行业前辈，了解最新的行业动态

## 2. gitHub网页的使用
### 2.1基本概念
+ 仓库(Repository)：仓库用了存放项目代码,每个项目对应一个仓库，多个开源项目则有多个仓库
+ 收藏(Star)：收藏项目，方便下次查看
+ 复制克隆项目(Fork)：fork的项目是独立存在的。 eg: forked from 张三/test仓库
+ 发起请求(Pull Request)：修改了fork项目的的文件，希望更新到原来的仓库，这时可以给原项目作者发起请求
+ 关注(Watch):关注项目，当项目更新可以接收到通知
+ Github主页
+ 事务卡片(issues)：作用是提交发现的代码bug,或者使用开源项目出现问题时使用.
+ 仓库主页：主要显示项目的信息，如：项目代码，版本，收藏/关注/fork情况等
+ 个人主页：个人信息：头像，用户名，关注的项目等

### 2.2注册github账号
+ 地址：https://github.com
+ 常用单词：Sign in: 登录，Sign up: 注册
+ 因为github是国外服务器，所有访问较慢或者无法访问，需要翻墙（Shadowsocks）
+ 新注册的用户必须验证邮箱后才能使用

### 2.3 创建仓库/项目
### 2.4 仓库管理
+ 新建文件
+ 修改文件
+ 删除文件
+ 上传文件
+ 搜索仓库文件：Find file
+ 下载/检出项目时
### 2.5 开源项目贡献流程
+ 新建issue:提交使用问题或建议或想法
+ pull Request:
    步骤
    1. fork项目
    2. 修改自己仓库的项目代码
    3. 新建pull request
    4. 等待原作者操作审核

## 3.git安装及使用
## 4.git基本工作流程
+ Git工作区域: 
    1. Git Repository(git仓库)：最终确定的文件保存到仓库，成为一个新的版本，并且对他人可见
    2. 暂存区: 暂存已经修改的文件，最后统一提交到git仓库中
    3. 工作区(Working Directory): 添加、编辑、修改文件等动作
+ 常用命令：
    1. 查看状态：`git status`
    2. 将文件添加到暂存区：` git add testPhp.test `或 ` git add . ` 
    3. 将暂存区的文件提交到git仓库：` git commit -m "提交描述" ` 或 ` git commit -m "提交描述" filename `
    4. 将本地git仓库的文件提交到github: `git push 远程库地址别名 远程分支名`
## 5.git初始化及仓库创建和操作
+ 基本信息设置
    1. 设置用户名： ` git config --global user.name 'your name' `
    2. 设置用户名邮箱： ` git config --global user.email '12345678@qq.com' `
    3. 查看设置： `git config --list`
    注意：该设置在github仓库显示谁提交了该文件
+ 初始化一个新的git仓库
    1. 新建一个文件夹
    2. 进入该文件，并执行`git init`将该文件变成git仓库文件
    3. 向仓库添加文件: `git add test.php` 和 ` git commit -m '提交描述' `
    4. 修改仓库文件
        1. 修改文件： `vi test.php`
        2. 将修改文件添加到暂存区： `git add test.php`
        3. 将暂存区的修改文件提交到git仓库：` git commit -m '提交描述' `
    5. 删除仓库文件
        1. 删除文件: `rm test.php`
        2. 从git中删除文件： `git rm test.php`
        3. 提交操作： ` git commit -m '提交描述' `
## 6.git管理远程仓库
+ git远程仓库实际上就是保持在服务器上的git仓库文件
+ 使用远程仓库的目的：备份、实现代码共享集中化管理
+ git克隆(clone): 将远程仓库(github上的项目)复制到本地。
        - 命令: `git clone 仓库地址`，eg：`git clone https://github.com/itcastgit1/test.git`
        - 效果：完整把远程库下载到本地，创建origin远程地址别名，初始化本地库
+ git上传(push)：将本地仓库上传到远程仓库：
    - 在本地创建远程库地址别名（本地与远程库可以不同名）：
        1. 查看远程库设置状态：`git remote -v`
        2. 添加远程库地址，并设置别名为origin：`git remote add origin https://github.com/atguigu2018ybup/huashan.git`
        3. 推送本地库到远程库(origin)指定分支(master是主分支)：`git push -u origin master`
        4. 如果第一次会让你输入用户名和密码
        > ### 注意：
        >   1. `git push origin`表示将当前分支推送到origin主机的对应分支。如果当前分支只有一个追踪分支，那么主机名都可以省略，直接用`git push`;如果当前分支与多个主机存在追踪关系，那么这个时候 **-u选项** 会指定一个默认主机，这样后面就可以不加任何参数使用`git push`.
        >   2. `git push -u origin master`将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使`git push`了
    - 问题1：无法同步或没有权限(the requested URL returned error:403 Forbidden while accessing)
      答案：私有项目，没有权限，输入用户名密码或者远程地址采用这种类型: vi .git/config 将 
      [remote "origin"] 
        url = https://github.com/用户名/仓库名.git 
      修改为：
      [remote "origin"]
        url = https://用户名:密码@github.com/用户名/仓库名.git
    - 问题2：文件不同，报错error: failed to push some refs to 'https://github.com/muyin/myStudy.git'。
        1. 答案1：进行push前先将远程仓库pull到本地仓库，再提交
            ```bash
            git pull origin master      # git pull --rebase origin master
            # 远程仓库不是一个空仓库，里边有文件，但是那部分文件没有和本地仓库关联，所有我们需要使用一下操作进行关联，如果初初建的是空仓库则没什么问题。使用 "git pull origin master --allow-unrelated-histories"
            git push -u origin master   # git push origin master
            ```
            > `git pull` 和 `git pull --rebase`的区别:   
            > git pull = git fetch + git merge   
            > git pull --rebase = git fetch + git rebase  
            > 从目的上来说，两者没差别，运行之后，你能获得一样的code base。但从版本管理的角度，两者有各自的使用意义。  
            > `git merge`:它把两条不同分支历史的所有提交合并成一条线，并在“末端”打个结，即生成一次合并提交。最后形成一条单一的提交线。  
            > `git rebase`:总的来说，它相当于把分叉的两条历史提交线中的一条，每一次提交都捡选出来，在另一条提交线上提交。最后也形成一条单一的提交线。  
            > 对比来看，`git merge`多了一次合并提交。`git rebase`则没有。不过，`git merge`保存了两条线的历史，而`git rebase`则会破坏历史。因为每次rebase,提交的id会变化。
        2. 答案2：强制push本地仓库到远程，即以本地库替换远程库 (这种情况不会进行merge, 强制push后远程文件可能会丢失 不建议使用此方法)
            ```bash
                git push -u origin master -f    # git push origin master
            ```
        3. 答案3：避开解决冲突, 将本地文件暂时提交到远程新建的分支中
            ```bash
                git branch name      # 创建完branch后, 再进行push
                git push -u origin name
            ```
+ 本地团队合作
    - 远程库修改的拉取：`git pull origin master`
    - 拉取合并：pull = fetch + merge
    - 只拉取：git fetch 远程库地址别名 远程分支名
    - 只合并：git merge 远程库地址别名/远程分支名
    - 如果不是基于github远程库的最新版所做的修改，不能推送，必须先拉取
    - 拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可。

## 7.创建github pages个人站点
+ 访问：[https://用户名.github.io](https://用户名.github.io)
+ 搭建步骤：
    1. 创建个人站点 》 新建仓库（注：仓库名必须是 **用户名.github.io**）
    2. 在仓库下新建index.html文件即可。 
+ 注意:github pages只支持静态网页，仓库里只能是.html文件
## 8.创建Project pages项目站点
+ 访问：[https://用户名.github.io/仓库名](https://用户名.github.io/仓库名)
+ 搭建步骤:
    1. 进入项目主页，点击settings
    2. 在settings页面，点击**launch automatic page generator**，选择主题，修改内容，发布即可。

## 9.常见问题

1.  问题：查看git状态(git status)时中文文件名乱码。乱码情况如: \345\233\242\351\230\237\hello.jpg  
    解决方法：运行git config --global core.quotepath false
    > core.quotepath设为false就不会对0x80以上的字符进行quote。中文显示正常
2.  问题：如何在提交多个项目勒？  
    解决方法：进入新的本地项目，使用 `git remote -v` 查看本项目的远程地址时为空，此时可使用 `git remote add origin github上项目的地址` 给该本地项目设置一个远程地址，这个设置是放在该本地项目文件夹中的。与其他的项目是分离的。所有不会影响。设置好后，最后使用 `git push origin master` 就可以提交了

## 10.git创建新分支

1. 创建本地分支

    git branch 分支名，例如：git branch aaa2.1
    注：aaa2.1是分支名称，可以随便定义。
 
2. 切换本地分支

    git checkout 分支名，例如从master切换到分支：git checkout aaa2.1
        > 创建并切换分支（上边两个操作合并到一起）： git checkout -b aaa2.1

3. 远程分支就是本地分支push到服务器上。比如master就是一个最典型的远程分支（默认）。
    git push origin aaa2.1
 
4. 远程分支和本地分支需要区分好，所以，在从服务器上拉取特定分支的时候，需要指定远程分支的名字。

    git checkout --track origin/aaa2.1
    注意该命令由于带有--track参数，所以要求git1.6.4以上！这样git会自动切换到分支。
 
5. 提交分支数据到远程服务器

    git push origin <local_branch_name>:<remote_branch_name>
    例如：
    git push origin aaa2.1:aaa2.1
    一般当前如果不在该分支时，使用这种方式提交。如果当前在 aaa2.1 分支下，也可以直接提交
    git push
 
6. 删除远程分支
    
    git push origin :develop

7. 与分支相关的命令
```bash
    # 假设远程有master和dev分支。
    # 1.克隆代码到本地
    git clone https://github.com/master-dev.git     
    
    # 2.查看所有的分支。会显示所有的本地和远程分支
    # 会看到三个分支，master[本地主分支]， origin/master[远程主分支]， origin/dev[远程开发分支]。
    # 新克隆下来的代码默认master和origin/master是关联的，也就是他们的代码保持同步
    git branch --all    
    
    # 3.创建本地关联origin/dev的分支
    # 创建本地分支dev,并和远程origin/dev分支关联，本地dev分支的初始代码和远程的dev分支代码一样
    git checkout dev origin/dev 

    # 4.切换到dev分支进行开发
    git checkout dev    # 切换到dev分支，然后就是常规的开发

```
```bash
    # 假设远程只有master分支
    # 1.克隆代码到本地
    git clone https://github.com/master-dev.git     
    
    # 2.查看所有分支
    git branch --all    # 会看到两个分支。master[本地分支]和origin/master[远程主分支]

    # 3.创建本地新的dev分支
    git branch dev      # 创建本地分支
    # 会看到master和dev,而且master上会有一个星号，这个时候dev是一个本地分支，远程仓库不知道它的存在
    # 本地分支可以不同步到远程仓库，我们可以在dev开发，然后merge到master,使用master同步代码。当然也可以同步
    git branch          # 查看分支

    # 4.发布dev分支（发布dev分支指的是同步dev分支的代码到远程服务器）
    git push origin dev:dev     # 这样远程仓库也有一个dev分支了

    # 5.在dev分支开发代码
    git checkout dev    # 切换到dev分支进行开发
    # 开发代码后，我们有两个选择。
    # 第一个：如果功能开发完成了，可以合并到主分支
    git checkout master # 切换到主分支
    git merge dev       # 把dev分支的更改和master合并
    git push            # 提交主分支代码到远程
    git checkout dev    # 切换到dev分支
    git push            # 提交dev分支代码到远程
    # 第二个：如果功能开发完成了，可以直接推送
    git push    # 提交到dev远程分支。注意：在分支切换之前最后先commit全部的改变

    # 6.删除分支
    git push origin  :dev   # 删除远程dev分支，危险命令哦
    git checkout master     # 切换到master分支
    git branch -d dev       # 删除本地dev分支

```
## 11.使用git管理项目的方式
    
    在实际开发中很多基本上都是把git当svn来用。

  ### 主分支

    实际开发中，一个仓库（通常只放一个项目）主要存在两条主分支：**master** 和 **develop** 分支。这两个分支的生命周期是整个项目周期。master分支是创建git仓库是自动生成的，随即我们就会从master分支创建develop分支。  

    **master:** 主分支。这个分支最为稳定，这个分支代表项目处于可发布的状态。master分支不是随随便便就可以签入代码的地方，只有计划发布的版本功能在develop分支上全部完成，而且测试没有问题了才会合并到master分支上。  
    
    **develop：** 开发分支。作为开发分支，平行于master分支。  
    例如二狗要开发一个注册功能，那么他会从develop分支上创建一个feature（特征）分支“fb-register”，在“fb-register”分支上将注册功能完成后，将代码合并到develop分支上。项目在给二狗分了个登录功能。二狗就会重复上面的步骤：从develop分支上新创建一个名为“fb-login”的分支，一个小时后，登录功能写好了，二狗又会将这个分支的代码合并回develop分支后将其删除。

  ### 支持分支/临时分支

    这些分支都是为了程序员协同开发，以及应对项目的各种需求而存在的。这些分支都是为了解决某一个具体的问题而设立，当这个问题解决后，代码会合并到主分支 develop 或 master 后删除，一般我们人为分出三种分支。

    **Feature branches:** 功能分支。必须从develop分支创建，完成后合并回develop分支。日常开发中最为密切。如上二狗开发注册和登录功能时的做法。（分支常用命名：feature-xxx）

    **Release branches:** 预发布分支。是指发布正式版本之前（即合并到master分支之前），我们可能需要一个预发布的版本进行测试。从develop分支创建，预发布结束后必须合并回develop与master分支。这个分支上可以做一些非常小的bug修复，当然你也可以禁止在这个分支做任何bug的修复工作，而只做版本发布的相关操作，例如设置版本号等操作。如二狗开发完登录注册功能后决定发布版本v0.1，那么他先从develop分支上创建一个Release分支“release-v0.1”，二狗在这个分支上把版本号等做了修改。然后愉快的发布了版本v0.1,版本上线后，二狗将这个分支分别合并回了develop与master分支，然后删除了这个分支。(分支常用命名：release-xxx)

    **Hotfix branches:** 修复bug分支。这个分支主要为修复线上特别紧急的bug准备的。必须从 master 分支创建，完成后合并回 develop 和 master 分支。 如突然发生大量用户无法登陆的问题，二狗先找到master分支上“tag v0.1”(一般代码发布后会及时在master上打tag)的地方，然后新建一个“hotfix-v0.1.1”的分支，在这个新分支上改bug，随后发布新版本。最后将代码合并回 develop 和 master 分支后删除这个分支。(分支常用命名：hotfix-xxx)

```bash
    # develop相关常用命令
    # 在master分支上创建develop分支
    git checkout -b develop master
    # 切换到master分支
    git checkout master
    # 对develop分支合并到当前master分支
    git merge --no-ff develop

    # 功能分支相关常用命令
    # 从develop创建一个功能分支
    git checkout -b feature-xxx develop
    # 开发完成后，将功能分支合并到develop分支
    git checkout develop
    git merge --no-ff feature-xxx
    # 删除feature分支
    git branch -d feature-xxx

    # 预发布分支相关常用命令
    # 创建一个预发布分支
    git checkout -b release-xxx develop
    # 确认没有问题后，合并到master分支
    git checkout master
    git merge --no-ff release-xxx
    # 对合并生成的新节点，做一个标签
    git tag -a v1.2
    # 再合并到develop分支
    git checkout develop
    git merge --no-ff release-xxx
    # 最后删除预发布分支
    git branch -d release-xxx

    # bug修复分支相关常用命令
    # 创建一个修复bug分支
    git checkout -b hotfix-xxx master
    # 修复结束后，合并到master分支
    git checkout master
    git merge --no-ff hotfix-xxx
    git tag -a 0.1
    # 再合并到develop分支
    git checkout develop
    git merge --no-ff hotfix-xxx
    # 删除修补bug分支
    git branch -d hotfix-xxx

    # 1.上面合并命令中的--no-ff的意思是no-fast-forward的缩写,使用该命令可以保持更多的版本演进细节。如果不使用该参数，默认使用了fast-forward进行merge.
    # 2.加上-a参数来创建一个带备注的tag，备注信息由-m指定。如果你未传入-m则创建过程系统会自动为你打开编辑器让你填写备注信息。git tag -a tagName -m "备注"
```
    
## 12.git常用命令
1. 查看、添加、提交、删除、找回、重置修改文件
```bash
 `git help <command>` # 显示command的帮助信息 
 `git show` # 显示某次提交的内容 git show #id
 `git rm <file>` # 从版本库中删除文件
 `git rm <file> --cached` #从版本库中删除文件，但不删除文件
 `git reset <file>` # 从暂存区恢复到工作文件
 `git reset --hard` # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改
 `git revert <$id>` # 恢复某次提交的状态，恢复动作本身也创建次提交对象
 `git revert HEAD`  # 恢复最后一次提交的状态

 git branch mybranch        # 创建分支mybranch
 git checkout mybranch      # 切换分支（到mybranch）
 git checkout -b mybranch   # 创建并切换到新建分支上（即切换到mybranch）

 git rebase master          # 更新master主线上的东西到该分支上
 git checkout master        # 切换到master分支
 git rebase mybranch        # 更新mybranch分支上的东西到master上

 git commit -a              # 提交
 git reset HEAD^            # commit之后，如果想撤销最近一次提交（即退回到上一次版本），并本地保留代码

 git branch -d mybranch     # 删除分支
 git branch -D mybranch     # 强制删除分支
 git branch                 # 列出所有分支
 git branch -v              # 查看各个分支最后一次提交
 git branch -merged         # 查看哪些分支合并入当前分支

 git fetch origin           # 更新远程库到本地
 git push origin mybranch   # 推送分支
 git merge origin/mybranch  # 取远程分支合并到本地
 git checkout -b mybranch origin/mybranch   # 取远程分支并分化一个新分支
 git push origin  :mybranch # 删除远程分支

```
## 13.git打tag

通常在发布软件的时候打一个tag,tag会记录版本的commit号，方便后期回溯

```bash
# 列出已有的tag
git tag
# 加上 -l 命令可以使用通配符来过滤tag
git tag -l "v3.3.*"

# 新建tag。
# 使用git tag命令跟上tag名字，直接创建一个tag
git tag v1.0
# 还可以加上 -a 参数来创建一个带备注的tag,备注信息有 -m 指定。如果你未传入 -m 则系统会自动为你打开编辑器让你填入备注信息
git tag -a v1.1 -m "my tag 备注"
# 查看所有的tag，新建的tag也在里边
git tag

# 查看tag详细信息
# git show 命令可以查看tag的详细信息，包括commit号等
git show v1.1


```

