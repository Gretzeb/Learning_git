
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
## 3.用法
### 版本创建与回退
1. 创建一个版本库  
   > +在目标文件夹下创建版本库
   > ```
   > mkdir ~/Learning_git
   > git init
   > ```  
   > 在~/Learning_git下创建了一个.git隐藏目录，即版本库目录。
2. 创建文件
3. 创建版本
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
4. 查看版本记录
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
5. 退回上个版本，使用命令：
   > ```
   > git reset --hard HEAD^
   > ```
   > _^的数表示前几个版本_
   > 或者
   > ```
   > git reset --hard HEAD~1
   > ```
   > _~后数字表示前几个版本_
6. 再次返回到版本2，使用命令：
   > ```
   > git reset --hard 版本号前几位
   > ```
7. 查看操作记录
   > ```
   > git reflog
   > ```
   > 借此在新终端中实现查阅版本号并退回版本

### 工作区和暂存区
1. 工作区(Working Directory)  
   > Learning_git就叫工作区
2. 版本库(Repository)与暂存区(index)
   >+ .git目录即为版本库，其中有一个叫index的暂存区，有自动创建的第一个分支master，以及指向master的一个指针HEAD.    
   >+ 'git add'实际上是将修改过的文件添加到暂存区；   
   >+ 'git commit'实际上是将暂存区的所有内容提交到当前分支.   
3. 管理修改
   >+ git管理的文件修改，只提交暂存区的修改来创建版本
   >+ 若改动并没有添加到暂存区，那么创建版本时不会被提交
4. 撤销修改
   >+ 用'git checkout -- <文件>'来丢弃工作区的改动；前提是还没提交到暂存区，即还没add
   >+ 用'git reset HEAD <文件>'可以把暂存区的修改撤销；前提是还没有提交到版本库，即还没commit
   >+ 非凡的手贱让你把错误的版本一路提交到了版本库，那就只能直接版本回退
5. 对比文件的不同
   >```
   > git diff HEAD -- <文件>对比工作区和某个版本文件的不同
   > git diff HEAD HEAD^ -- <文件>对比两个版本文件的不同
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
   
### 分支管理
1. 概念
   + 主分支：master. HEAD指向master, master指向提交版本。   

   + 创建一个新分支即创建一个新指针，master仍指向原提交，HEAD指向新的分支，新的分支指向原提交，版本更新后HEAD和新的分支指向新的提交，而master仍指向原提交。   

   + 合并分支只要让master指向和当前新分支指向的提交即可，事后可以删掉新分支，因为它只是个指针
2. 创建和合并分支的相关操作
   >```
   > git branch                                                       用于查看当前有哪些分支以及在哪个分支下工作   
   > git branch <新的分支>                               用于创建分支   
   > git branch -d <分支名>                              用于删除分支(-D无视未合并)   
   > git checkout -b <新的分支>                     用于创建并移动到新的分支   
   > git checkout <分支名>                               用于移动到现有分支   
   > git merge <分支名>                                     用于将目标分支合并到当前分支   
   > git log --pretty=oneline --graph            是个小工具
   >```
3. 解决冲突
   + 当两个分支的内容都经过修改，再想合并就会产生冲突   
   + 冲突会以>>>>> ===== <<<<<等符号标记出内容与分支的对应，需要手动修改后重新提交   
   + 重新提交后master将指向最新版本,将master合并到新分支后两边会同时指向最新提交，但回退版本时会回到各自分支下的提交 
4. 分支管理策略
   + git默认使用fast forward，但有的时候虽然合并时没有冲突但还是不能成功，比如合并的是另一个文件
   + 出现不能fast forward的时候需要输入合并说明或者
      ```
      git merge --no-ff -m '<合并说明信息>' <分支名>
      ```
      以此禁用fast forward一次性完成合并
5. stash使用情景   
   + 假如说此时在分支work上工作，但是还没完成所以不能提交，突然来了一个修改bug的任务，这时候我们应该：  
   >(1). 'git stash'储存进度   
   >(2). 创建新分支new，完成临时任务，提交   
   >(3). 回到master，禁用fast forward合并，填好合并信息说明   
   >(4). 回到work，'git stash pop'恢复进度  
   + *stash命令：*  
   >```
   >git stash save "save message"      执行存储时，添加备注，方便查找，只有git stash 也要可以的，但查找时不方便识别。
   >git stash list                                           查看stash了哪些存储
   >git stash show                                      显示做了哪些改动，默认show第一个存储,如果要显示其他存贮，后面加stash@{$num}，比如第二个 git stash show stash@{1}
   >git stash show -p                                 显示第一个存储的改动，如果想显示其他存存储，命令：git stash show  stash@{$num}  -p ，比如第二个：git stash show  stash@{1}  -p
   >git stash apply                                      应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash apply stash@{$num} ， 比如第二个：git stash apply stash@{1} 
   >git stash pop                                         命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下,默认为第一个stash,即stash@{0}，如果要应用并删除其他stash，命令：git stash pop stash@{$num} ，比如应用并删除第二个：git stash pop stash@{1}
   >git stash drop stash@{$num}        丢弃stash@{$num}存储，从列表中删除这个存储
   >git stash clear ：                                  删除所有缓存的stash 
## 4. github
### 创建仓库
   ```
   我的用户名：Gretzeb   
   我的邮箱：g1665196730@outlook.com
   ```
### 添加ssh账户
   配置某台机器的git配置   
   ```
   vim ~/.gitconfig
   ``` 
   > ```
   > [user]    
   >          email = g1665196730@outlook.com     
   >           name = Gretzeb    
   > ```
   使用如下命令生成ssh密钥，一路按回车直到结束
   ```
   ssh-keygen -t rsa -C 'g1665196730@outlook.com'
   ```
   此时~/.ssh目录下出现两个文件：   
   >*公钥为 id_rsa.pub*   
   >*私钥为 id_rsa*     

   cat公钥，复制其内容到    
   > github -> settings -> SSH and GPG keys -> New SSH key    

   并填写标题
### 克隆项目
   使用如下代码从用ssh地址克隆代码
   ```
   git clone git@github.com:<XXX>.git
   ```
### 推送拉取分支
   1. 添加一个新的远程库，指定简称
   ```
   git remote add [简称] git@github.com:<XXX>.git
   git remote add origin git@github.com:Gretzeb/Learning_git.git
   ```
   2. 查看当前远程库配置
   ```
   git remote -v
   ```
   3. 提取远程库
   ```
   git fetch origin                                  从远程库下载分支数据
   git merge origin/master               将远程仓库的分支合并到当前分支
   git branch --set-upstream-to=origin/master master        本地分支跟踪服务器分支
   ```
   4. 推送
   ```
   git push origin master
   ```
   5. 拉取
   ```
   git pull origin master
   ```
