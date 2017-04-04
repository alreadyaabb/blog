# Linux Skills

## 美化 Shell 和 VIM

```shell
# Install powerline
apt install powerline
```

添加如下代码到用户目录的 .bashrc 中

> 注意：不同用户的目录不同，.bashrc 是在用户的家目录中，
> 比如，用户铭为 `director` ，通常家目录为 `/home/director` ，
> 则这个文件的路径为 `/home/director/.bashrc` ，在一个新的 bash
> session 启动之后会首先加载这个文件，设置一些配置信息。

```shell
# PowerLine configuration
POWERLINE_SCRIPT=/usr/share/powerline/bindings/bash/powerline.sh
if [ -f $POWERLINE_SCRIPT ]; then
    powerline-daemon -q
    POWERLINE_BASH_CONTINUATION=1
    POWERLINE_BASH_SELECT=1
    source $POWERLINE_SCRIPT
fi
```

注释掉之前的默认配置

```shell
# if [ "$color_prompt" = yes ]; then
#    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
#else
#    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
#fi
```

为 VIM 添加行号和常用配置

```vim
se nu ts=4 sw=4 et hls is si sm ai
colo industry
se fileencodings=utf-8,gb2312,gbk,gb18030
se termencoding=utf-8
se encoding=utf-8
se laststatus=2
se showtabline=2
se noshowmode
```

## 创建普通用户

```shell
# Create a new user, aim to replace root
adduser director
```

## References

* [鸟哥私房菜](http://cn.linux.vbird.org/linux_basic/linux_basic.php)

