# Git Skills and Tips

## 基础操作

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

### Checkout, Branch

```shell
# Create a new local branch
git checkout -b new_branch
# Do some modifications
# ...
# Commit the changes to local branch
git add * -A
git commit -m"Messages"
# Push the branch to remote repository
git push "origin" new_branch:new_branch
```

## References

* [Git Pro](https://git-scm.com/book/en/v2)
* [Git GUI Clients](https://git-scm.com/downloads/guis)

