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

## References

* [Git Pro](https://git-scm.com/book/en/v2)
* [Git GUI Clients](https://git-scm.com/downloads/guis)

