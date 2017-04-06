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

## References

* [Git Pro](https://git-scm.com/book/en/v2)
* [Git GUI Clients](https://git-scm.com/downloads/guis)

