# Git Skills and Tips

## Basic Operations

### Clone, Pull, Commit and Push

```shell
# Clone the repository from GitHub
git clone https://github.com/alreadyaabb/blog.git

# Update the repository local
git pull

# Commit the modifications and Push them to the server
git add * -A
git commit -m"A comment about this modification"
git push
```

## Git 基础知识

### state of Git

Git 有三种状态，你的文件可能处于其中一种：已提交 (committed) 、已修改 (modified) 、和已暂存 (staged).
1. 已提交表示数据已经安全的保存在本地数据库中。
2. 已修改表示修改了文件，但还没保存到数据库中。
3. 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

### workflow of Git

基本的 Git 工作流程如下：
1. 在工作目录中修改文件。
```shell
git add XX
```
2. 暂存文件，将文件的快照放入暂存区域。
```shell
git commit -m "XX"
```
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。
```shell
git push
```
### Git 忽略文件

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。通常都是些自动生成的文件，比如日期文件，或者编译过程中创建的临时文件等。在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。

#### the example of gitignore

```ini
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

### Git 移除文件

```shell
#只是简单地从工作目录中手工删除文件
$ rm

#将指定文件从已跟踪文件清单及工作目录中删除
$ git rm 

#把文件从 Git 仓库中删除，但仍然保留在当前工作目录中
$ git rm --cached
```

### Git 移动文件
```shell
#命令基本语法
$ git mv file_from file_to
```

这句命令相当于下面三条命令：
```shell
$ mv file_from file_to
$ git rm file_from
$ git add file_to
```

### Git 查看提交历史
```shell
#基本语法
$ git log
```
该命令会按提交时间列出所有的更新，最近的更新排在最上面。
这条命令的选项：
1. -p -2
> -p 是一个常用选项，用来显示每次提交的内容差异。-2 用来控制显示最近两次提交。当进行代码审查，或者快速浏览某个搭档提交的 commit 所带来的变化的时候，这个参数就非常有用了。
2. --stat
> 这个选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。在每次提交的最后还有一个总结。
3. --pretty
> 这个选项可以指定使用不同的默认格式的方式展示提交历史。这个选项有一些内建的子选项供你使用。由于内建的子选项比较多，笔者在这里就不一一赘述了。在这里要提的是 format 子选项，这个选项的作用是可以定制要显示的记录格式。这样的输出对后期提取分析格外有用——因为输出的格式不会随着 Git 的更新而发生改变。

Table 1. git log --pretty=format 常用的选项

|选项  |说明                                       |
| :--: | ----------------------------------------- |
|%H    |提交对象(commit)的完整哈希字串|
|%h    |提交对象的简短哈希字串|
|%T    |树对象(tree)的完整哈希字串|
|%t    |树对象的简短哈希字串|
|%P    |父对象(parent)的完整哈希字串|
|%p    |父对象的简短哈希字串|
|%an   |作者的电子邮件地址|
|%ad   |作者修订日期(可以用--date=选项定制格式)|
|%ar   |作者修订日期，按多久以前的方式显示|
|%cn   |提交者(committer)的名字|
|%ce   |提交者的电子邮件地址|
|%cd   |提交日期|
|%cr   |提交日期，按多久以前的方式显示|
|%s    |提交说明|

当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用。这个选项添加了一些 ASCII 字符串来形象地展示了你的分支、合并历史。
```shell
#具体用法
$ git log --pretty=format:"%h %s" --graph
```
![Git配图08](https://github.com/alreadyaabb/blog/blob/master/images/Git08.png)
![Git配图09](https://github.com/alreadyaabb/blog/blob/master/images/Git09.png)

以上只是简单介绍了一些 git log 命令支持的选项。git log 的常用选项列出了我们目前涉及到的和没涉及到的选项，以及它们是如何影响 log 命令的输出的：

Table 2. git log 的常用选项

|选项|说明|
|----|----|
|-p|按补丁格式显示每个更新之间的差异|
|--stat|显示每次更新的文件修改统计信息|
|--shortstat|只显示 --stat 中最后的行数修改添加移除统计|
|--name-only|仅在提交信息后显示已修改的文件清单|
|--name-status|显示新增、修改、删除的文件清单|
|--abbrev-commit|仅显示 SHA-1 的前几个字符，而非所有的40个字符|
|-relative-date|使用较短的相对时间显示(比如,"2 weeks ago")|
|--graph|显示 ASCII 图形表示的分支合并历史|
|--pretty|使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format(后跟指定格式)|

#### 限制输出长度

之前笔者提到过一个 git log -2 的命令，其实那是 $ git log -<n> 选项的一个实例，n 可以是任何整数，表示仅显示最近的若干条提交。不过实践中我们是不太用这个选项的，Git 在输出所有提交时会自动调用分页程序，所以你一次只会看到一页内容。
另外还有按照时间限制的选项，比如 --since 和 --until 也很有用。例如，下面的命令列出所有最近两周内的提交：
```shell
$ git log --since=2.weeks
```
这个命令可以在多种格式下工作，比如说具体的某一天"2008-01-15",或者是相对地多久以前"2 years 1 day 3 minutes age"。
还可以给出若干搜索条件，列出符合的提交。用 --author 选项显示指定作者的提交，用 -grep 选项搜索提交说明中的关键字。(**注意**:如果要得到同时满足这两个选项搜索的提交，就必须用 -all-match 选项。否则满足任意一个条件的提交都会被匹配出来)
另一个非常有用的筛选选项是 -S，可以列出哪些添加或移除某些字符串的提交。比如说，你想找出添加或移除了某一个特定函数的引用的提交，你可以这样使用：
```shell
$ git log -Sfunction_name
```
最后一个很实用的 git log 选项是路径(path)，如果只关心某些文件或者目录的历史提交，可以在 git log 选项的最后指定它们的路径。因为是放在最后位置上的选项，所以用两个短划线(--)隔开之前的选项和后面的路径名。

Table 3. 限制 git log 输出的选项

|选项|说明|
|----|----|
|-(n)|仅显示最近的 n 条提交|
|--since,--after|仅显示指定时间之后的提交|
|--until,--before|仅显示指定时间之前的提交|
|--author|仅显示指定作者相关的提交|
|--committer|仅显示指定提交者相关的提交|
|--grep|仅显示含指定关键字的提交|
|-S|仅显示添加或移除了某个关键字的提交|

### 撤销操作
在任何阶段，你都有可能想要撤销某些操作。下面这一节讲述的就是如何撤销操作。
在开始之前，需要大家注意的就是，有些撤销操作是**不可逆**的。这是在使用 Git 的过程中，会因为操作失误而导致之前的工作丢失的少有几个地方之一。
有时候我们提交完了才发现楼倒了几个文件没有添加，或者提交信息写错了。此时，可以运行带有 --amend 选项的提交命令尝试重新提交：
```shell
$ git commit --amend
```
这个命令会将暂存区中的文件提交。如果自上次提交以来你还未做任何修改(例如，在上次提交后马上执行了此命令)，那么快照会保持不变，而你所修改的只是提交信息。
例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：
```shell
$ git commit -m "initial commit"
$ git add forgotten_file
$ git commit --amend
```
最终你只会有一个提交-第二次提交将代替第一次提交的结果。

#### 取消暂存的文件
接下来将讲解如何操作暂存区域与工作目录中已修改的文件。这些命令在修改文件状态的同时，也会提示如何撤销操作。例如，你已经修改了两次文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 git add * 暂存了它们两个。如何只取消暂存两个中的一个呢？git status 命令提示了你：
```shell
$ git add *
$ git status
On branch master
Changes to be committen:
   (use "git reset HEAD <file>..." to unstage)

   renamed:  README.md -> README
   modified: CONTRIBUTING.md
```
在 "Changes to be committed" 文字正下方，提示使用 git reset HEAD <file>... 来取消暂存。所以，我们可以这样来取消暂存 CONTRIBUTING.md 文件:
![Git10.png](https://github.com/alreadyaabb/blog/blob/master/images/Git10.png)
*Note*:调用时加上 --hard 选项可以令 git reset 成为一个危险命令(可以导致工作目录中所有当前进度**丢失**)，但本例中工作目录内的文件并不会被修改。不加选项地调用 git reset 并不危险——它只会修改暂存区域。

#### 撤销对文件的修改
如果你并不想保存对 CONTRIBUTING.md 文件的修改怎么办？你该如何方便地撤销修改-将它还原成上次提交时的样子(或者刚克隆完的样子，或者刚把它放入工作目录时的样子)？幸运的是，git status 也告诉了你应该如何做。在最后一个例子中，未暂存区域是这样：
![Git11.png](https://github.com/alreadyaabb/blog/blob/master/images/Git11.png)

它非常清楚地告诉了你如何撤销之前所做的修改。让我们来按照提交执行：
```shell
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   renamed:  README.md -> README
```
可以看到那些修改已经被撤销了。
*Important*:你需要知道 git checkout -- [file] 是一个危险的命令，这很重要。你对那个文件做的任何修改都会消失-你只是拷贝了另一个文件来覆盖它。除非你确实清楚不想要那个文件了，否则不要使用这个命令。
记住，在 Git 中任何*已提交的*东西几乎总是可以恢复的。甚至那些被删除的分支中的提交或使用 --amend 选项覆盖的提交也可以恢复。然而，任何你未提交的东西丢失后很可能再也找不到了。

### 远程仓库的使用

#### 查看远程仓库
如果你想要查看你已经配置的远程仓库服务器，可以运行 git remote 命令。它会列出你的指定的每一个远程服务器的简写。如果你已经克隆了自己的仓库，那么至少应该能看到 origin -这是 Git 给你克隆的仓库服务器的默认名字：
![Git12.png](https://github.com/alreadyaabb/blog/blob/master/images/Git12.png)
你也可以指定选项 -v ，会显示需要读写远程仓库使用 Git 保存的简写预期对应的 URL。
![Git13.png](https://github.com/alreadyaabb/blog/blob/master/images/Git13.png)
如果你的远程从仓库不止一个，该命令会将它们全部列出。例如，与几个协作者合作的，拥有多个远程仓库的仓库看起来像下面这样：
![Git14.png](https://github.com/alreadyaabb/blog/blob/master/images/Git14.png)
这样我们可以轻松拉取其中任何一个用户的贡献。

#### 添加远程仓库
我在之前的章节中已经提到并展示了如何添加远程仓库的示例，不过这里将告诉你如何明确地做到这一点。运行 git remote add <shortname> <url> 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写：
![Git15.png](https://github.com/alreadyaabb/blog/blob/master/images/Git15.png)
现在你可以在命令行中使用字符串 pb 来代替整个 URL。例如，如果你想要拉取 Paul 的仓库中有但你没有的信息，可以运行 git fetch pb：
![Git16.png](https://github.com/alreadyaabb/blog/blob/master/images/Git16.png)
现在 Paul 的 master 分支可以在本地通过 pb/master 访问到-你可以将它合并到自己的某个分支中，或者如果你想要查看它的话，可以检出一个指向本地分支。

#### 从远程仓库中抓取与拉取
就如刚才所见，从远程仓库中获得数据，可以执行：
```shell
$ git fetch [remote-name]
```
这个命令会访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中分支的引用，可以随时合并或查看。
如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 "origin" 为简写。所以，git fetch origin 会抓取克隆(或上一次抓取)后新推送的所有工作。必须注意**git fetch origin 命令会将数据拉取到你的本地仓库-它并不会自动合并或修改你当前的工作。**当准备好时你必须手动将其合并入你的工作。
如果你有一个分支设置为跟踪一个远程分支，可以使用 git pull 命令来自动的抓取然后合并远程分支到当前分支。这对你来说可能时一个更简单或更舒服的工作流程；默认情况下，git clone 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支。运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

#### 推送到远程仓库
当你想分享你的项目时，必须将其推送到上游。这个命令很简单：git push [remote-name] [branch-name]。当你想要将 master 分支推送到 origin 服务器时，那么运行这个命令就可以将你所做的备份到服务器：
```shell
$ git push origin master
```
只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。当你和其他人在同一时间克隆，他们先推送送到上游然后你再推送到上游，你的推送就会毫无疑问地拒绝。你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。

#### 查看远程仓库
如果想要查看某一个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。如果想以一个特定的缩写名运行这个命令，例如 origin ，会得到像下面类似的信息：
![Git17.png](https://github.com/alreadyaabb/blog/blob/master/images/Git17.png)
它同样会列出远程仓库的 URL 与跟踪分支的信息。这些信息非常有用，它告诉你正处于 master 分支，并且合并到本地 master 分支。它也会列出拉取到的所有远程引用。
这是一个经常遇到的简单例子。如果你是 Git 的重度使用者，那么还可以通过 git remote show 看到更多的信息。
![Git18.png](https://github.com/alreadyaabb/blog/blob/master/images/Git18.png)
这个命令列出了当你在特定分支执行 git push 会自动地推动到哪一个远程分支。它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了，还有当你执行 git pull 时哪些分支会自动合并。

#### 远程仓库的移除与重命名
如果想要重命名引用的名字可以运行 git remote rename 去修改一个远程仓库的简写名。例如，想要将 pb 重命名为 paul ，可以用 git remote rename 这样做：
![Git19.png](https://github.com/alreadyaabb/blog/blob/master/images/Git19.png)
值得注意的是这同样也会修改你的远程分支名字。那些过去引用 pb/master 的现在会引用 paul/master。
如果因为一些原因想要移除一个远程仓库-你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了-可以使用 git remote rm :
![Git20.png](https://github.com/alreadyaabb/blog/blob/master/images/Git20.png)

### 打标签
像其他版本控制系统(VCS)一样，Git 可以给历史中的某一个提交打上标签，以示重要。比较有代表性的是人们会使用这个功能来标记发布结点(v1.0 等等)。在本节中，你将会学会如何列出已有的标签、如何创建新标签、以及不同类型的标签分别是什么。

#### 列出标签
在 Git 中列出已有的标签是非常简单直观的。只需要输入 git tag：
![Git21.png](https://github.com/alreadyaabb/blog/blob/master/images/Git21.png)
这个命令以字母顺序列出标签：但是它们出现的顺序并不重要。
你也可以使用特定的模式查找标签。例如，Git 自身的源代码仓库包含标签的数量超过 500 个。如果只对 1.8.5 系列感兴趣，可以运行：
![Git22.png](https://github.com/alreadyaabb/blog/blob/master/images/Git22.png)

#### 创建标签
Git 使用两种主要类型的标签：轻量标签(lightweight)与附注标签(annotated)。
一个轻量标签很像一个不会改变的分支-它只是一个特定提交的引用。
然而，附注标签是存储在 Git 数据库中的一个完整对象。它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard (GPG) 签名与验证。通常建议创建附注标签，这样你可以拥有以上所有信息；但是如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，轻量标签也是可用的。

#### 附注标签
在 Git 中创建一个附注标签是很简单的。最简单的方式是当你在运行 tag 命令是指定 -a 选项：
![Git23.png](https://github.com/alreadyaabb/blog/blob/master/images/Git23.png)
-m 选项指定了一条将会存储在标签中的信息。如果没有为附注标签指定一条信息，Git 会运行编辑器要求你输入信息。
通过使用过 git show 命令可以看到标签信息与对应的提交信息：
![Git24.png](https://github.com/alreadyaabb/blog/blob/master/images/Git24.png)
输入显示了打标签的信息、打标签的日期时间、附注信息，然后显示具体的提交信息。

#### 轻量标签
另一个给提交打标签的方式是使用轻量标签。轻量标签本质上是将提交校验和存储到一个文件中-没有保存任何其他信息。创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字：
![Git25.png](https://github.com/alreadyaabb/blog/blob/master/images/Git25.png)
这时，如果在标签上运行 git show ，你不会看到额外的标签信息。命令只会显示出提交信息：
![Git26.png](https://github.com/alreadyaabb/blog/blob/master/images/Git26.png)

#### 后期打标签
你也可以对过去的提交打标签。假设提交历史是这样的：
![Git27.png](https://github.com/alreadyaabb/blog/blob/master/images/Git27.png)
现在，假设在 v1.2 时你忘记给项目打标签，也就是在 "updated rakefile" 提交。你可以在之后补上标签。要在那个提交上打标签，你需要在命令的末尾指定提交的校验和(或部分校验和)：
![Git28.png](https://github.com/alreadyaabb/blog/blob/master/images/Git28.png)
可以看到你已经在那次提交上打上标签了：
![Git29.png](https://github.com/alreadyaabb/blog/blob/master/images/Git29.png)

#### 共享标签
默认情况下，git push 命令不不会传送标签到远程仓库服务器上。在创建完标签后你必须显式地推送标签到共享服务器上。这个过程就像共享远程分支一样-你可以运行 git push origin [tagname]。
![Git30.png](https://github.com/alreadyaabb/blog/blob/master/images/Git30.png)
如果想要一次性推送很多标签，也可以使用带有 --tags 选项 git push 命令。这将会把所有不在远程仓库服务器上的标签全部传送到那里。
![Git31.png](https://github.com/alreadyaabb/blog/blob/master/images/Git31.png)
现在，当所有人从仓库中克隆或拉取，他们也能得到你的那些标签。

#### 检出标签
在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。如果你想要工作目录与仓库中特定的新标签版本完全一样，可以使用 git checkout -b [branchname] [tagname] 在特定的标签上创建一个新分支：
![Git32.png](https://github.com/alreadyaabb/blog/blob/master/images/Git32.png)
当然，如果在这之后又进行了一次提交，version2 分支会因为改动向前移动了，那么 version2 分支就会和 v2.0.0 标签稍微有些不同，这时就应该当心了。

## References

* [Git Pro](https://git-scm.com/book/en/v2)
* [Git GUI Clients](https://git-scm.com/downloads/guis)

