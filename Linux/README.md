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

### vi 的使用

基本上 vi 共分为三种模式，分别是"一般模式"、"编辑模式"、"指令列命令模式"。

* 一般模式：
> 以 vi 打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中，你可以使用"上下左右"按键来移动光标，也可以使用"删除字符"或"删除整行"来处理档案内容，也可以使用"复制，粘贴"来处理你的文件数据。

* 编辑模式：
> 在一般模式中可以进行删除、复制、粘贴等等的动作，但是却无法编辑文件内容的！要等到你按下"i,I,o,O,a,A,r,R"等任何一个字母之后才会进入编辑模式。通常在 Linux 中，按下这些按键时，在画面的左下方会出现"INSERT 或 REPLACE"的字样，此时才可以进行编辑。而如果要回到一般模式时，则必须要按下"ESC"这个按键即可退出编辑模式。

* 指令列命令模式：
> 在一般模式当中，输入":/?"三个中的任何一个模式，就可以将光标移动到最底下那一行。在这个模式当中，可以提供你"搜寻资料"的动作，而读取、存盘、大量取代字符、离开 vi 、显示行号等等的动作则是在此模式中达成的！

我们可以将三个模式用图片的形式表现出来：

![Linux_img](https://github.com/alreadyaabb/blog/blob/master/images/Linux01.png)

## References

* [鸟哥私房菜](http://cn.linux.vbird.org/linux_basic/linux_basic.php)

