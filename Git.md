

# Git流程

1.git clone 仓库

2.git checkout -b my-feature

3.在my-feature分支内操作；操作完可以git diff查看修改区别，使用git add <文件名>来放到暂存区，最后git commit同步到本地仓库

4.git push origin my-feature到远端

如果遇到远端main更新：

1.git checkout main

2.git pull origin main 将更新同步到本地

3.git checkout my-feature 回到自己的分支，但此时该分支并没有main中更新的内容

4.git rebase main 先将该分支的内容替换为新的main，再将my-feature修改内容尝试添加上去，但可能rebase conflict。rebase之后相当于在最新的main上进行了my-feature的修改。

5.git push -f origin my-feature 同步到远端my-feature

pull request，要求仓库管理者把my-feature合并到main中去。

仓库管理者使用Squash and merge将所有提交合成一个进行merge。最后需要删除不用的feature。

`find . -name ".git" | xargs rm -Rf` 删除git控制，使其成为一个普通文件夹

# 01.Git配置

## Ubuntu安装

```she
apt-get install git
```

## 用户名配置

```shell
git config --global user.name "JaniusB"
git config --global user.email jbsama@sina.com
```

## Instead of 配置

```shell
git config --global url.git@github.com:.insteadOf https://github.com/
```

## 设置命令别名

```shell
git config --global alias.cin "commit --amend --no-edit"
```

## 设置SSH免密登录

```shell
ssh-keygen -t ed25519 -C "jbsama@sina.com"
```

之后一路回车,生成完成密钥。到/root/.ssh/目录下找到id_ed25519.pub，里面保存着SSH密钥，将其粘贴到Github上，新建一个SSH的New key，测试是否成功

```shell
ssh -T git@github.com
```

# 02.创建一个本地仓库并提交到远端

## 创建一个新的仓库

```shell
mkdir testgit
cd testgit
git init
```

​		添加一个文件

```shell
vim abd.md
```

​		可以随时使用tree .git来查看文件的改变

## 在git中追加文件

```shell
git add <filename>
```

​		如果是git add .则是当前文件夹，git add 命令告诉 Git 开始对这些文件进行跟踪,将他们存在本地的暂存区。

​		status可以查看状态，随便输入看看效果

```shell
git status
```

## 提交文件

​		将暂存区的文件提交到本地仓库中

```shell
git commit -m '附带的信息'
```

## 提交到远端仓库

先在Github上创建一个仓库

![image-20220530170050538](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530170050538.png)

![image-20220530170100226](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530170100226.png)

创建好仓库后，自动跳转到该页面，记下红色方框内容，我们需要它来配置远端仓库

![image-20220530170229911](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530170229911.png)

## 设置Git Remote

​	查看Remote

```shell
git remote -v
```

​	添加Remote

```shell
git remote add origin git@github.com:JaniusBisecond/testgit.git
```

​	删除Remote，当然这里不需要用

```shell
git remote rm <RemoteName>
```

## 使用push将本地仓库提交到远端

```shell
git push origin master
```

再次刷新GitHub页面，就会发现文件已经被提交上去了。![image-20220530171905966](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530171905966.png)

刚刚命令中的origin是刚刚设置好的remote，而master是分支(branch)了。如果之前使用过git status，就会发现有一条提示叫做“On branch master”，当然在GitHub页面上也能清楚的看到一个叫'branch'的按钮，点击进去就可以看到当前仓库的分支。

![image-20220530173203019](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530173203019.png)

![image-20220530173228393](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530173228393.png)

接下来就创建一个新的分支。

# 03.分支操作

什么是分支？

可以理解为类似河流一样的东西，分支都是从主干(master)分离出来，分支的操作不会影响到主干。

![<!--分支图--来源runoob.com-->](https://static.runoob.com/images/svg/git-brance.svg)

直接上操作，实践看看到底什么意思。

使用branch命令可以创建新的分支，如果不带参数则查看已有分支。查看分支中前面带*号的就是当前所在分支。

```shell
git branch newbranch
git branch
```

使用checkout命令切换分支，接下来的操作都是在newbranch这个分支中进行

```shell
git checkout newbranch
```

创建一个新的文件，并提交到本地仓库

```shell
vim branchfile.md
git add branchfile.md
git commit -m 'just add branchfile'
```

分支之间的对比可以直接使用diff命令来比较，不过他们比较的是commit之后的。

```shell
git diff master newbranch
```

当然在本地目录也可以看到，此时在newbranch分支下查看目录得到的结果是

```shell
$ls
abc.md  branchfile.md
```

切换为master分支后再查看目录（git checkout master）

```shell
$ls
abc.md
```

最后再切换回newbranch并提交到远程仓库

```shell
git checkout newbranch
git push origin newbranch
```

可以在Github上看到一个显眼的提示，注意我这里已经进入到了newbranch分支了，可以看到多了一个branchfile.md

![image-20220530174722891](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530174722891.png)

再一次切回master，而master中是没有branchfile的

![image-20220530175134171](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530175134171.png)

接下来再次创建两个分支

```shell
git checkout master
git checkout -b master_branch

git checkout newbranch
git checkout -b branch_branch
```

分别进入master_branch和branch_branch使用ls看看效果。

结果是master_branch与master中文件相同，branch_branch与newbranch中文件相同。由此可知，创建的分支是以创建时你所在的分支为“主干”进行创建。

再分别为两个新的分支添加一点文件

```shell
#branch_branch分支中
git checkout newbranch
vim b_b.md
git add .
git commit -m 'add b_b'

#master_branch分支中
git checkout master_branch
vim m_b.md
git add .
git commit -m 'add m_b'
```

现在有了四个分支，下面列出四个分支中的文件

master:   abc.md

master_branch:  abc.md  m_b.md

newbranch:  abc.md  branchfile.md

branch_branch:  abc.md  b_b.md  branchfile.md

## 合并分支

我们将newbranch和branch_branch进行合并，将merge <目标分支> 合并到当前所在分支

```shell
git checkout newbranch
git merge branch_branch
```

此时newbranch文件为 abc.md  b_b.md  branchfile.md，将branch_branch删除(工作位置不能于branch_branch中)

```shell
git branch -D branch_branch
```

## Pull同步

将newbranch同步到远程仓库

```shell
git checkout newbranch
git push origin newbranch
```

直接在GitHub上添加一个文件

![image-20220530201913223](https://github.com/JaniusBisecond/Janius-note/blob/online/images/Git/image-20220530201913223.png)

本地仓库比远程仓库上少一个repo文件

使用pull将远程仓库的文件拉取到本地进行同步

```shell
git pull origin newbranch
```

此时远程仓库与本地仓库的newbranch分支内容相同。

在测试中也遇到一个情况，如果直接删除了本地仓库中的文件，从相同的远程分支中Pull不会拉取被删除的文件。
