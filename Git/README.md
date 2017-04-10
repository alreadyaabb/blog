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
## References

* [Git Pro](https://git-scm.com/book/en/v2)
* [Git GUI Clients](https://git-scm.com/downloads/guis)

