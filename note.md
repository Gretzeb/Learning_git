
# git note  


## 1. 安装  
```
sudo apt-get install git
```

## 2. 命令

### 这些是各种场合常见的 Git 命令（使用命令git）：

1. 开始一个工作区（参见：git help tutorial）
```
   clone      克隆一个仓库到一个新目录
   init       创建一个空的 Git 仓库或重新初始化一个已存在的仓库
```
2. 在当前变更上工作（参见：git help everyday）
```
   add        添加文件内容至索引
   mv         移动或重命名一个文件、目录或符号链接
   reset      重置当前 HEAD 到指定状态
   rm         从工作区和索引中删除文件
```
3. 检查历史和状态（参见：git help revisions）
```
   bisect     通过二分查找定位引入 bug 的提交
   grep       输出和模式匹配的行
   log        显示提交日志
   show       显示各种类型的对象
   status     显示工作区状态
```
4. 扩展、标记和调校您的历史记录
```
   branch     列出、创建或删除分支
   checkout   切换分支或恢复工作区文件
   commit     记录变更到仓库
   diff       显示提交之间、提交和工作区之间等的差异
   merge      合并两个或更多开发历史
   rebase     在另一个分支上重新应用提交
   tag        创建、列出、删除或校验一个 GPG 签名的标签对象
```
5. 协同（参见：git help workflows）
```
   fetch      从另外一个仓库下载对象和引用
   pull       获取并整合另外的仓库或一个本地分支
   push       更新远程引用和相关的对象
```
**命令 'git help -a' 和 'git help -g' 显示可用的子命令和一些概念帮助。  
查看 'git help <命令>' 或 'git help <概念>' 以获取给定子命令或概念的
帮助。**
## 3.初级用法
### 创建一个版本库  
> +在目标文件夹下创建版本库
> ```
> mkdir ~/Learning_git
> git init
> ```  
> 在~/Learning_git下创建了一个.git隐藏目录，即版本库目录。

### 版本创建与回退

+ __使用__
1. 创建文件code.txt:
   > ```
   > this is the new shit
   > ```
   >
2. 创建版本
   > ```
   > git add code.txt
   > git commit -m 'version_01'
   > ```
   >> 此时报错 '***请告诉我你是谁'，解决办法：
   >> ```
   >> cd .git
   >> vim config
   >> ```
   >> 添加如下内容：
   >> ```
   >> [user]
   >>       email = g1665196730@outlook.com
   >>       name = Gretzeb
3. 查看版本记录
   > ```
   >  git log
   > ```
   > 此时显示：
   > ```
   > commit 版本号
   > Author: 用户名<邮箱>
   > Date    日期时间
   > 
   >       version01
   > ```
4. 添加一行：
   > ```
   > this is the new shit
   > sorry for that shit
   > ```
5. 再创建一个版本并查看记录，步骤同上 
6. 退回版本1，使用命令：
   > ```
   > git reset --hard HEAD^
   > ```
   > _^的数表示前几个版本_
   > 或者
   > ```
   > git reset --hard HEAD~1
   > ```
   > _~后数字表示前几个版本_
7. 再次返回到版本2，使用命令：
   > ```
   > git reset --hard 版本号前几位
   > ```
8. 查看操作记录
   > ```
   > git reflog
   > ```
   > 借此在新终端中实现查阅版本号并退回版本

+ __工作区和暂存区__
1. 工作区(Working Directory)  
   > Learning_git就叫工作区
2. 版本库(Repository)与暂存区(index)
   >+ .git目录即为版本库，其中有一个叫index的暂存区，有自动创建的第一个分支master，以及指向master的一个指针HEAD.    
   >+ 'git add'实际上是将修改过的文件添加到暂存区；   
   >+ 'git commit'实际上是将暂存区的所有内容提交到当前分支.   

   示例：
   > 先后编辑 code.txt 与 note.md  
   > code.txt: 
   > ```
   > this is the new shit
   > sorry for that shit
   > I'm just kidding
   > ```
   > 将两个文件同时add并commit：
   > ```
   > git add code.txt
   > git add note.md
   > git commit -m '合体1'
   > ```
   > 这个版本里有两个修改后的文件    
3. 管理修改
   > git管理的文件修改，只提交暂存区的修改来创建版本
   >+ 编辑code.txt并add.
   > ```
   > this is the new shit
   > sorry for that shit
   > I'm just kidding
   > silence, wench
   > ```
   > ```
   > git add code.txt
   > ```
   >+ 再次修改，不add
   > ```
   > this is the new shit
   > sorry for that shit
   > I'm just kidding
   > silence, wench
   > I don't wanna be a whorn anymore
   > ```
   >+ commit一个新的版本，并查看status   
   >
   > 第二次的改动并没有添加到暂存区，所以创建版本时没有被提交
4. 撤销修改
   >+ 用'git checkout -- <文件>'来丢弃工作区的改动；前提是还没提交到暂存区，即还没add
   >+ 用'git reset HEAD <文件>'可以把暂存区的修改撤销；前提是还没有提交到版本库，即还没commit
   >+ 非凡的手贱让你把错误的版本一路提交到了版本库，那就只能直接版本回退
5. 对比文件的不同
   >+ 对比工作区和某个版本文件的不同
   >```
   > git diff HEAD -- <文件>
   >```
   >+ 对比两个版本文件的不同
   >```
   > git diff HEAD HEAD^ -- <文件>
   >```
   >+ 后者比前者多的标绿色，少的红色
6. 删除文件   

   *删除文件 ' rm <文件> '之后*
   >+ 确实要删除
   >```
   > git rm <文件>
   > git commit -m '删除文件<文件>'
   >```
   >+  并不想删除
   >```
   > git checkout - <文件>
   >```
   >(只能恢复最新版本，暂存区中的改动不会复原)  
   
+ __分支管理__
1. 概念
   > 主分支：master. HEAD指向master, master指向提交版本。   

   > 创建一个新分支即创建一个新指针，master仍指向原提交，HEAD指向新的分支，新的分支指向原提交，版本更新后HEAD和新的分支指向新的提交，而master仍指向原提交。   

   > 合并分支只要让master指向和当前新分支指向的提交即可，事后可以删掉新分支，因为它只是个指针
2. 创建和合并分支的操作
   > 