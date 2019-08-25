
# git 的 基本命令

```js
// 配置
$ git config --global user.name 用户名
$ git config --global user.email 邮箱

// 初始化
init 

// 添加到暂存区
add 

// 提交到本地仓库
commit          

// 查看提交历史
log              // 查看完整信息
log --online     // 查看简便信息（在一行内显示）

// 查看 / 创建 分支
branch

// 提交到 GitHub
push

// 查看区域文件信息
status

// 回溯 或 切换分支
checkout 
```



# 添加 add

- 将工作区的文件提交的暂存区，添加的是发生了变化的文件，（增删文件）（修改文件内容）

- git add .                         全部添加（所有的已修改文件）
- git add 文件名               添加单个文件（使用名称）
- git add 文件路径           添加文件夹



# 提交 commit

- 将暂存区的文件提交到本地仓库
- git commit -m '描述信息'            提交所有暂存区的文件
- git commit                                  提交文件，会启用vi编辑器，输入的内容为描述信息
- git commit -a -m '描述信息'        





# 提交步骤

- **注意：** 在本地上传时，要注意 github 中的内容要与 git 中的一致，若是 GitHub 中的内容大于 git 中，要 先拉取，再上传（readme）

- **提交到本地仓库**

```js
// 1.第一次使用 git 要进行配置
git config global user.name '用户名'
git config global user.email '邮箱'

// 2.对选择的文件夹进行初始化，创建本地仓库
git init

// 3.添加工作区的文件到暂存区
git add .

// 4.提交暂存区的文件到本地仓库
git commit -m '描述信息'
```

数据已经存在与本地仓库

- **设置 `ssh key`**

```js
// 设置 ssh 密钥   只要进行一次
ssh-keygen -t rsa -C "youremail@example.com"

// 去路径中查找 id_rsa.pub 文件，打开复制里面内容，前往 github 中，设置 setting => SSH and GPG keys ,描述随意填写，下面的文本域粘贴 id_rsa.pub 中内容 确定
```



- **从本地仓库提交到远程仓库（两种情况）**

```js
// 第一种情况，不使用 readme 文件

// 6.关联 git 与 github 中的仓库 （origin 是地址的别名，）
git remote add origin 仓库地址

// 7.提交到GitHub (第一次提交需要使用 -u 以后则不用)
// -u 表示记录推送到哪，以后使用可以直接 git push 不用 remote 地址了
git push -u origin master
// git push -u origin   master:master
// 远程是master时可省略  本地分支:远程分支


```

```js
// 第二种情况，使用了 readme 文件（GitHub中存在readme文件，本地git没有，所以会报错要先拉取下来）
git pull --rebase origin master

// 然后在上传
git push -u origin master


```

# 克隆

- 把 GitHub 中的仓库克隆下来 



```js
// 克隆到本地
git clone 仓库地址

// 拉取
git pull --rebase origin master

// 上传
git push -u origin master

```

git