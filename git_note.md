# Git笔记

## git的一些操作指令

![image-20210608082142531](git_note.assets/image-20210608082142531.png)

## 一、git本地仓库

###  1.1、git的工作流程

![image-20210608082432455](git_note.assets/image-20210608082432455.png)

![image-20210608082446809](git_note.assets/image-20210608082446809.png)

### 1.2、本地仓库操作

​	什么是仓库？仓库又名版本库，英文名repository。可以理解为一个目录，用来存放代码的，目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除等操作Git都能跟踪到。

![image-20210608082536726](git_note.assets/image-20210608082536726.png)

`$ git config --global user.name "lulu666667"`

`$ git config --global user.email "15802658256@163.com"`

![image-20210608082555649](git_note.assets/image-20210608082555649.png)



![image-20210608082611547](git_note.assets/image-20210608082611547.png)

  ==不建议使用非空目录，不要出现中文==

- a、创建空目录

  ![image-20210608082630525](git_note.assets/image-20210608082630525.png)

- b、命令行中进入项目目录lulu_git

  ![image-20210608082651224](git_note.assets/image-20210608082651224.png)

- c、git仓库初始化（让Git知道，它需要管理这个目录）

  ![image-20210608082707922](git_note.assets/image-20210608082707922.png)

  执行之后，会在项目目录下创建“.git”的隐藏目录，这个目录是git所创建的，不能删除，也不能随意改动其中的内容

![image-20210608082724149](git_note.assets/image-20210608082724149.png)

- `git add`可以添加一个文件，也可以同时添加多个文件

  ![image-20210608082740823](git_note.assets/image-20210608082740823.png)



### 1.3、版本回退

​	版本回退分为两步骤进行操作：

​	步骤：

 - 1）查看版本，确定需要回到的时刻点

   指令：

   ​	`git log` ，显示日志

   ​	`git log --pretty=oneline` ，推荐用这个

 - 2）回退到以前操作

   指令：

   ​	`git reset --hard` 提交编号

- 3)回退完，如果想又回到最新操作

  指令：

  ​	`git reflog`

  ​	然后用 `git reset --hard "reflog获得的第二个编号"`

![image-20210608082806012](git_note.assets/image-20210608082806012.png)



## 二、远程仓库

### 2.1、线上仓库创建

 ![image-20210608082830688](git_note.assets/image-20210608082830688.png)

### 2.2、两种常规使用方式

#### 2.2.1、基于http协议

- a、创建空目录，名称就为test

![image-20210608082847960](git_note.assets/image-20210608082847960.png)

- b、使用clone指令克隆线上仓库到本地：

  语法：`git clone 线上仓库地址`

  ![image-20210608082908232](git_note.assets/image-20210608082908232.png)

- c、在仓库上做对应的操作（提交暂存区、提交本地仓库、提交线上仓库、拉取线上仓库）

  - 提交暂存区、提交本地仓库的指令：

    `cd 文件目录`

    `git add 文件名`

    `git commit -m "注释"`

  - 提交线上仓库指令：`git push`

    在首次往线上仓库test提交内容的时候出现了403的错误，原因是不是任何人都可以往线上仓库提交内容，必须需要鉴权。

    鉴权：需要修改”.git/config” 文件内容

    ![image-20210608082934983](git_note.assets/image-20210608082934983.png)

    在config文件中的url 的https://和github.com中间插入 `用户名:密码@`

    设置完后重新输入`git push`

    出现下面这个则提交成功

    ![image-20210608082959738](git_note.assets/image-20210608082959738.png)

    此时可以观察浏览器，刷新线上仓库的地址。

    

  - 拉取线上仓库。如果同时有多人在仓库放东西，且别人在线上放了文件，为了保持本地和线上文件一致，应拉取线上仓库。

    指令：`git pull`

每天开机第一件事就是先`git pull`拉取线上最新的版本；每天关机前要做的就是`git push`将本地的代码提交到线上仓库。



#### 2.2.2、基于ssh协议（不用输账号密码，只要生成公钥私钥，推荐用这种方法）

该方式与https相比，只是影响github对于用户的身份鉴权方式，对于git的具体操作（如提交本地、添加注释、提交远程等操作）没有任何影响。

生成公私钥对指令（**需先自行安装OpenSSH**）：`ssh-keygen -t rsa -C "注册邮箱"`

- a、打开提示

- b、创建公私钥对文件

  ![image-20210608083030698](git_note.assets/image-20210608083030698.png)

  ![image-20210608083050128](git_note.assets/image-20210608083050128.png)

- c、上传公钥文件内容(id_rsa.pub)

  ![image-20210608083104084](git_note.assets/image-20210608083104084.png)

  填写完毕之后保存即可。

- d、执行后续git操作，操作与先前一样

  - 1）clone 线上仓库到本地(`git clone`)

  - 2)   修改文件后添加缓存区

    ![image-20210608083132199](git_note.assets/image-20210608083132199.png)

### 2.3、分支管理

![image-20210608083147325](git_note.assets/image-20210608083147325.png)

​	在版本回退的章节里，每次提交后都会有记录，Git把它们串成时间线，形成类似于时间轴的东西，这个时间轴就是一个分支，我们称之为==master分支==。

​	分支相关指令：

- 查看分支：`git branch`

- 创建分支：`git branch 分支名`
- 切换分支：`git chechout  分支名`
- 创建并切换分支：`git checkout -b 分支名`
- 删除分支：`git branch -d 分支名`
- 合并分支：`git merge 被合并的分支名`



结果：

- 查看分支：![image-20210608083212538](git_note.assets/image-20210608083212538.png)

  注意：当前分支前面有个标记 ’* ‘

- 创建分支：![image-20210608083227036](git_note.assets/image-20210608083227036.png)

- 切换分支：![image-20210608083249223](git_note.assets/image-20210608083249223.png)

- 合并分支：

  先在dev分支下的readme文件中新增一行并提交本地仓库

  ![image-20210608083302734](git_note.assets/image-20210608083302734.png)

  然后切换到main分支下，观察readme.txt文件

![image-20210607165053695](C:\Users\szs\AppData\Roaming\Typora\typora-user-images\image-20210607165053695.png)

​	刚刚 在dev分支的readme文件下新增的文字已经消失

​	将dev分支的内容和main合并：

![image-20210608083417963](git_note.assets/image-20210608083417963.png)

合并之后新增的文字又出现

- 删除分支：![image-20210608083433935](git_note.assets/image-20210608083433935.png)

  注意：在删除分支的时候，一定要先退出要删除的分支，然后才能删除。



### 2.4、冲突的产生与解决

案例：模拟产生冲突。

1）在线上修改了线上仓库的代码

![image-20210608083454542](git_note.assets/image-20210608083454542.png)

 此时本地仓库的内容与线上不一致的。

2） 我自己没有下载线上仓库的文档，自己又在本地仓库写了

![image-20210608083507442](git_note.assets/image-20210608083507442.png)

3） 然后我又提交了，就会报错

![image-20210608083520458](git_note.assets/image-20210608083520458.png)

4）解决冲突，先git pull，再git push

![image-20210608083533551](git_note.assets/image-20210608083533551.png)

此时git已经将线上与本地仓库的冲突合并到了对应的文件中

![image-20210608083557790](git_note.assets/image-20210608083557790.png)

5）打开冲突文件，解决冲突

解决方法:需要和另一个人（谁先提交的）商量，看代码如何保留，保留需要的，不需要的进行删除。

6）重新提交

## 三、Git实用技能

### 1、图形管理工具

- Github for Desktop
- Source treee
- TortoiseGit

### 2、忽略文件

​	忽略文件需要新建一个名为.gitignore的文件（`touch .gitignore`），该文件用于声明忽略文件或不忽略文件的规则，规则对当前目录及其子目录生效，在.gitignore文件中以#开头的都是注释。

​	常见的规则写法：

- 过滤整个mtk文件夹：`/mtk/`
- 过滤所有.zip文件：`*.zip`
- 过滤某个具体文件: `/mtk/do.c`
- 不过滤具体某个文件: `lindex.php`

案例：

1）先在本地仓库中新建一个js目录以及一个js文件

2）一次提交本地与线上

![image-20210608083614840](git_note.assets/image-20210608083614840.png)

3) 新增.gitignore文件：`touch .gitignore`

4) 编写文件规则

![image-20210608083646108](git_note.assets/image-20210608083646108.png)

5）重新提交后线上并没有index.js文件

![image-20210608083713464](git_note.assets/image-20210608083713464.png)

