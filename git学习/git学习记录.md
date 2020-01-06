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
        1. 删除文件: rm test.php
        2. 从git中删除文件： git rm test.php
        3. 提交操作： git commit -m '提交描述'
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
        3. 推送本地库到远程库(origin)指定分支(master是主分支)：`git push origin master`
        4. 如果第一次会让你输入用户名和密码
    - 问题：无法同步或没有权限(the requested URL returned error:403 Forbidden while accessing)
      答案：私有项目，没有权限，输入用户名密码或者远程地址采用这种类型: vi .git/config 将 
      [remote "origin"] 
        url = https://github.com/用户名/仓库名.git 
      修改为：
      [remote "origin"]
        url = https://用户名:密码@github.com/用户名/仓库名.git
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