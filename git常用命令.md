### git常用命令

![image-20220714003405479](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mactypora202207140034606.png)

#### 1、初始化本地库

git inti 

#### 2、查看本地库的状态

git status

```bash
git status
On branch master
#指当前分支为master
No commits yet
#表示当前没有提交任何的东西
nothing to commit (create/copy files and use "git add" to track)
#表示当前是否有文件，没有最终过文件
```

#### 3、提交到暂存区

git add  xxx

git add -A    将已经修改的文件添加到暂存区

git status

```bash
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   hi.txt
	#已经追踪到了当前文件
	#如果要删除git rm --cached xxx，只是删除到了暂存区
	
```

#### 4、提交到本地库

git commit -m   “日志信息”  xxx

git status 

```bash
 git status
On branch master 
#目前是主分支
nothing to commit, working tree clean
#已经提交，提交过后目前这个文件既没有新增也没有修改，工作的树是干净的
```

查看版本信息

方法一：git reflog

```bash
b4f2758 (HEAD -> master) HEAD@{0}: commit (initial): fist
#b4f2758版本号   fist 提交时的版本信息
```

方法二： git  log

```bash
commit b4f2758a4d46dbf894af05fcd0f67dadc04f5165 (HEAD -> master)#版本号以及分支
Author: RocBrid <1126465609@qq.com>#提交时的用户名称
Date:   Thu Jul 14 01:02:10 2022 +0800 # 提交时间

    fist#提交的版本信息
```

#### 5、修改文件后

执行添加git  add xxxx到暂存区，提交到git  commit -m  “”  xxxx本地库后，再次 git status

```bash
 git commit -m  "seond" hi.txt
[master 4d8a4ab] seond
 1 file changed, 2 insertions(+), 1 deletion(-)
 #一个改变，两行新增，一行修改
```

#### 6、切换版本历史版本查看

git reset --hard 版本号

```bash
git reset --hard b4f2758
```

```bash
b4f2758 (HEAD -> master) HEAD@{0}: reset: moving to b4f2758
4d8a4ab HEAD@{1}: commit: seond
b4f2758 (HEAD -> master) HEAD@{2}: commit (initial): fist
#此时指针已经指向了切换的版本号的文件，当打开查看时就是当前版本的内容
```

#### 7、git分支

![image-20220714014150801](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mactypora202207140141881.png)

- ##### 查看分支

git branch -v 

```bash
 git branch -v
* master b4f2758 fist
```

- ##### 创建分支

git branch 分支名

```bash
 git branch  hot-fix
 git branch -v
  hot-fix b4f2758 fist
* master  b4f2758 fist
```

- ##### 删除分支

###### 删除本地分支

git   branch   -d   分支名称 （例：hot-fix）

###### 删除远程分支

 git push  地址（例：https://github.com/RocBrid/gitstudy.git ） --delete   分支名(例：hot-fix)

```bash
$ git push  https://github.com/RocBrid/gitstudy.git  --delete hot-fix
To https://github.com/RocBrid/gitstudy.git
 - [deleted]         hot-fix
```

清理本地无效分支 (远程已经删除本地没有删除的分支) 

git fetch -p

- ##### 切换分支

git checkout 分支名

```bash
 git checkout hot-fix
Switched to branch 'hot-fix'

 git branch -v
* hot-fix b4f2758 fist
  master  b4f2758 fist
```

- ##### 合并分支

将hot-fix分支合并到master分支

注意：要在master分支上合并

git merge hot-fix（合并的分支名称）

```bash
 git merge hot-fix
Updating b4f2758..ad46329
Fast-forward
 hi.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

- 合并冲突

当master和hot-fix都修改了代码时，合并时会存在冲突，需要人工手动的去修改

```bash
git merge hot-fix
Auto-merging hi.txt
CONFLICT (content): Merge conflict in hi.txt
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
 cat hi.txt                                                                                                      
isdsdadasdad

<<<<<<< HEAD #表示当前分支存在的位置
hell world
hell world  master
=======   #下一个分支的起始位置
hell world  hot-fix
hell world
>>>>>>> hot-fix  #要合并的分支的位置
```

修改

```bash
                                                                                               
isdsdadasdad

hell world  hot-fix
hell world
```

注意修改完成以后一定要添加到本地库，然后在提交，提交时不能带文件名

git add hi.txt

git commit   -m "xxxx"   (这里不带文件名否则报错)

#### 8、给远程仓库创建别名

git remote -v 查看当前所有远程地址别名

git remote add  别名（最好和库名一致） https://github.com/RocBrid/gitstudy.git

```bash
gaopeng@gaopeng MINGW64 /e/tool/girProdout (master)
$ git remote add  gitstudy https://github.com/RocBrid/gitstudy.git

gaopeng@gaopeng MINGW64 /e/tool/girProdout (master)
$ git remote -v
gitstudy        https://github.com/RocBrid/gitstudy.git (fetch)
gitstudy        https://github.com/RocBrid/gitstudy.git (push)
```

#### 9、推送本地库到远程仓库

一开始推送如果报错输入下面命令

```bash
$ git config --global http.sslVerify false
```

推送指令(gitstudy是别名)

```bash
$ git push gitstudy master
```

#### 10、远程仓库拉取到本地仓库

指令

```bash
$ git pull gitstudy master
#出现以下信息表示拉取成功
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 728 bytes | 36.00 KiB/s, done.
From https://github.com/RocBrid/gitstudy
 * branch            master     -> FETCH_HEAD
   4b2c4ab..6fc6f7e  master     -> gitstudy/master
Updating 4b2c4ab..6fc6f7e
Fast-forward
 hell.txt | 1 +
 1 file changed, 1 insertion(+)
```

#### 11、克隆远程仓库的代码

git clone   地址

克隆的操作：拉取代码，初始化本地库，创建别名
