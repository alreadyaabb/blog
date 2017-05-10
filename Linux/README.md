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

## Vi 与 Vim

### 为什么要学 Vi

* 所有的 Unix Like 系统都会内建 vi 文本编辑器，其他的文本编辑器则不一定会存在；
* 很多个别软件的遍及接口都会主动呼叫 vi ；
* vim 具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计；
* 因为程序简单，编辑速度相当快速。

### 什么是 vim

vim 可以视作 vi 的进阶版本，vim 可以用颜色伙底线等方式来显示一些特殊的信息。它可以一句大难的扩展名或是档案内的开头信息，判断该档案的内容二自动的呼叫改程序的语法判断式，再对其进行语法高亮。

**简单来说**:vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。vim 则可以说是程序开发者的一项很好用的工具，就连 vim 的官网自己也说 vim 是一个 “程序开发工具” 而不是文字处理软件。因为 vim 里面加入了很多额外的功能，例如支持正规表示法的搜寻架构、多档案编辑、区块复制等等。这对于我们在 Linux 上面进行一些配置文件的修订工作时，时很棒的一项功能！
## References

* [鸟哥私房菜](http://cn.linux.vbird.org/linux_basic/linux_basic.php)

