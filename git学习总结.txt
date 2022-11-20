参考：[(2 封私信 / 30 条消息) 编程 - 收藏夹 - 知乎 (zhihu.com)](https://www.zhihu.com/collection/650322279)

**1、基本命令**

```python
# 安装Git
$ sudo apt install git

# 配置个人信息
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

# 切换目录初始化
$ git init

# 文件添加到仓库
$ git add -p <file>
注：这里是添加到缓存区，最终还是要commit的

# 把文件提交到仓库
$ git commit -m "add LICENSE"
注：-m 主要是附注一些信息，接str
注：在commit之前，必须先添加到暂存区-->add

# 查看仓库当前状态
$ git status

# 查看difference
$ git diff
注：git diff HEAD -- ces.txt 要大写，是什么分支
注：--- 变动之前的文件；+++变动之后的文件。
注：@@ -1，2 +2，3 表示：-前一个我呢见，+后一个，3表示修改的第3行。

# 显示从最近到最远的提交日志
$ git log --pretty=oneline # 格式化输出信息
注：包括作者信息等，git log即可 git log 5-->输出5行，依此类推
注：加入--pretty=oneline，就是一行一行输出-简化
注： cat ces.txt 可以进行查看
# 版本退回
$ git reset --hard HEAD^ 
注：当前版本HEAD,上一个版本HEAD^,上上个版本HEAD^^

$ git reset --hard 130f10a # 或HEAD~100
# 回退50版本那就是HEAD~50
注：如果要返回最新版本或某个版本：
添加版本标识符-->130f10a(保证前几位不重复即可，位数自己决定。)

# 查看命令记录
$ git reflog #查看和编辑引用日志

git log -g 查看引用日志的历史记录
git log --since=2.weeks 查询两周内的提交记录
git log --author 查询某个用户的提交记录
git log --grep=“搜索信息” 搜索提交信息
git log --first-parent 查看当前元素的第一个父对象
git log --max-parents=0 查询项目的起点

# 丢弃工作区的修改，回到最近一次git commit或git add时的状态：
$ git checkout -- README.md

# 把暂存区的修改撤销掉（unstage）
$ git reset HEAD READER.md

# 从版本库中删除该文件
$ git rm README.md
$ git commit -m "remove READER.md"

# 把误删的文件恢复到最新版本，checkout其实用版本库里的版本替换工作区的版本
$ git checkout -- README.md

$ mkdir -p "a1/a2" # 创建文件夹
$ touch xxx.py # 创建相应后缀的文件
$ git rm xxx.md #删除相应的文件
注：删除后在恢复时，就别HEAD^了，需要hard到相应编码的文件，如下：
$ git reset --hard dadad
```

## 2、**远程仓库**

```python
$ ssh-keygen -t rsa -C "youremail@example.com"
1、填写自己的邮箱，然后会生成，".ssh"，如图。
注：不能用md或者txt打开，而要。之后会自己复制到clipboard
win:clip < ~/.ssh/id_rsa.pub
linux：cat ~/.ssh/id_rsa.pub
2、进入setting：https://github.com/settings/keys
3、add sshkey
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCjthRsFD8OaVnMUmbmm4s16Clyysr4GeZk4ywrAYIKGKBX4GRL79PZZAIe4PMXtmHARdSWorCNiZgsW5ya77gGPfzwI8HCCQQ+9sVkreWgKYsn+UFJ79MDVl4tqGIyY7uFfllW2jy9YJnIq+bTfXyPeMgAwXepAu8ftqObdoe0C5NsFl+DE5QbiFjkeCkskbkRkWBsF4QWfE/1SRhYaeTA9AQiYYeewYSTmTAjwX3QVvwAQIhj1k9UOwuuy0dqJ+yqsub27TFH+OVb4DtR84UQ3h3qFUYYvM8uPsMYUgNqe9x0MGKsH1NWlnbTWjF701uXIvyVLz8A8e1OQgbWTqZDHTVNDntn3qOihqQIbovD1iSm7STqHIsHFbZ0u7Ug9lhAh/BbZiEVKphnsDmsfBu46c7sbi6eyryYHbVKF4SMkVVdI/IUy2pJ/GGvySa/k1TJddTvAnDMOVuWbKn9O/QShrwBMJvyogfaRA5onqp7MaYgDtZYLj9sjcd+jgcdjsE= 1145986820@qq.com


# 测试是否成功
$ ssh -T git@github.com

```

![image-20220511153052950](C:/Users/Lori/AppData/Roaming/Typora/typora-user-images/image-20220511153052950.png)

```python
# 先在gihub网站创新一个repository
git init
git add README.md
git commit -m "first commit"
注：上述操作都是在本地仓库进行操作的。

# 把一个已有的本地仓库与之关联
注：origin是起的一个alias，可以自己定义。http或者ssh都可，后者更安全
$ git remote add origin https://github.com/renengade/ces2022.git

$ git remote add origin git@github.com:renengade/ces2022.git

# 把本地库的所有内容推送到远程库上（推送master分支的内容）
$ git push -u origin master

# 向远程库推送更新
$ git push origin master

# 从远程库克隆
$ git clone git@github.com:michaelliao/gitskills.git

```

### 3、分支管理

```python
#查看当前分支的文件
$ git ls-files
注：master有什么，新建分支也同样有。分支的目的就是不影响master功能的同	时进行修改，修改后再合并至master。

# 创建+切换dev分支
$ git checkout -b dev

# 相当于
$ git branch dev # 创建分支
$ git checkout dev

# 查看当前分支，当前分支前面标有×号
$ git branch

# 切换回master分支
$ git checkout master

# 合并指定分支到当前分支
注：将 merge的添加到当前的分支。e.g：当前master-->git merge branch
$ git merge dev

# 修改分支名称
$ git branch -m oldname newname 
注：-M意味着要强制修改，例如存在b1，现在要修改为b1。

# 分支的pull和push
# 注：将本地分支push的前提，
$ git branch -a  # 查看本地和远程所有分支
$ git push origin branch_name  # 推送本地分支到远程分支

# 推送本地的同时删除远程分支，此时本地的分支是保留的。
注:冒号的位置要留意
$ git push origin :remote_branch_name  

# 读取远程分支状态:必须已经分支有改动，这一步是拉去remote分支的前提。
$ git fetch
 
# 拉取remote分支，并在本地创建分支。
$ git checkout -b local_branch origin/remote_branch  

# 删除dev分支
$ git branch -d dev

# 查看分支合并情况
$ git log --graph --pretty=oneline --abbrev-commit
*   59bc1cb conflict fixed
|\
| * 75a857c AND simple
* | 400b400 & simple
|/
* fec145a branch test
```

### 4、分支操作冲突问题

```python

```

### 5、其他

```python
# 删除feature1分支
$ git branch -d feature1

# 修改readme.txt文件，并提交一个新的commit
$ git add readme.txt
$ git commit -m "add merge"


# 合并dev分支，请注意--no-ff参数，表示禁用Fast forward
$ git merge --no-ff -m "merge with no-ff" dev


# 如果需要临时修复Bug，可以把当前工作现场“储藏”起来，等Bug修复后恢复现场后继续工作
$ git stash

# 此时查看工作区是干净
# 切换到需要修复Bug的分支，创建临时分支来修复
$ git checkout master
$ git checkout -b issue-101

# 修复完成后切换到master分支，完成合并，删除临时分支
$ git checkout master
$ git merge --no-ff -m "merged bug fix 101" issue-101
$ git branch -d issue-101

# Bug修复后，切换回dev分支继续干活
$ git checkout dev

# 查看工作现场列表
$ git stash list

# 恢复工作现场
$ git stash pop # 恢复的同时把stash内容也删了
$ git stash apply # 恢复，不删除stash的内容，使用git stash drop

# 再次查看工作现场列表，干净
$ git stash list

# 可以多次stash，恢复时指定恢复
$ git stash apply stash@{0}

# 强行删除一个没有合并过的分支
$ git branch -D <name>

# 要查看远程库的信息
$ git remote
$ git remote -v

# 推送其他分支
$ git push origin dev

# 从远程库clone，默认情况只能看到master分支，需要在dev分支，必须创建远程origin的dev分支到本地
$ git checkout -b dev origin/dev
$ git checkout -b branch-name origin/branch-name
$ git branch --set-upstream branch-name origin/branch-name # 关联

# 向远程库推送dev有冲突
$ git pull # 抓取到本地合并解决冲突，再向远程推送
$ git push origin dev
```

### 6、标签管理

```python
$ git tag tag_name # 添加一个标签

$ git tag -a tag_name -m "message" # 添加标签并指定标签的描述信息

$ git tag # 查看所有标签

$ git tag -d tag_name # 删除本地标签

$ git push origin tag tag_name # 推送本地标签到远程

$ git push origin :refs/tags/tag tag_name # 删除一个远程标签
```

