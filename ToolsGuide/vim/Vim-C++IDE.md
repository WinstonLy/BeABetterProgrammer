## 写在前面

vim 初始只是一个文本编辑工具，但其支持各种插件，其用户手册也是多半在讲如何高效的利用 vim 编写代码，本文就是介绍 vim 的一些基本操作能力，同时也是介绍如何将 vim 打造成为一个高效的 C/C++ 语言的 IDE。

### vim 的基本操作

vim 的键盘图如下所示：

![](images/vim键盘图.jpg)

- **vim 的三种模式**

    >**Command mode**
    >
    >启动便会进入命令模式，这是键盘对于编辑器来说是命令，而不是字符。
    >常见命令：
    >
    >- `i`：进入编辑模式
    >- `:`：进入底线命令模式
    >- `x`：删除当前光标位置的字符
    >- `dd`：删除当前行并存到剪贴板里
    >
    >**Insert mode**
    >类似于Windows的记事本，快捷键也基本通用
    >
    >- 字符按键以及Shift组合，输入字符
    >- ENTER，回车键，换行
    >- BACK SPACE，退格键，删除光标前一个字符
    >- DEL，删除键，删除光标后一个字符
    >- 方向键，在文本中移动光标
    >- Page Up/Page Down，上/下翻页
    >- Insert，切换光标为输入/替换模式，光标将变成竖线/下划线
    >- ESC，退出输入模式，切换到命令模式
    >
    >**Last line mode**
    >底线命令模式有很多实用功能，最基本的几个字符如下：
    >
    >- `w`：保存
    >- `q`：退出
    >- `!`：强制
    >    可以组合命令，比如`wq!`表示强制保存退出

- **进入 vim**

    ```sh
    vim 文件名
    ```

    文件名必须完整，包括后缀，若存在该文件，则编辑，否则会新建文件

- **常用命令**

    - ==移动光标的方法==

        | 快捷键             | 说明                                                         |
        | ------------------ | ------------------------------------------------------------ |
        | h 或 向左箭头键(←) | 光标向左移动一个字符                                         |
        | j 或 向下箭头键(↓) | 光标向下移动一个字符                                         |
        | k 或 向上箭头键(↑) | 光标向上移动一个字符                                         |
        | l 或 向右箭头键(→) | 光标向右移动一个字符                                         |
        | 30j / 30↓          | 向下移动30行                                                 |
        | <Page Down>        | 屏幕『向下』移动一页                                         |
        | <Page Up>          | 屏幕『向上』移动一页                                         |
        | +                  | 光标移动到非空格符的下一行                                   |
        | -                  | 光标移动到非空格符的上一行                                   |
        | n<space>           | n 表示数字，按数字之后按下空格键，光标会向右移动这一行的 n 个字符 |
        | 0/<End>键          | 移动到这一行的最前面字符处                                   |
        | $/<Home>键         | 移动到这一行的最后面字符处                                   |
        | G                  | 移动到这个档案的最后一行                                     |
        | nG                 | n 为数字。移动到这个档案的第 n 行                            |
        | gg                 | 移动到这个档案的第一行，相当于 1G                            |
        | n<Enter>           | n 为数字。光标向下移动 n 行                                  |

    - ==搜索替换==

        | 快捷键                                         | 说明                                                         |
        | ---------------------------------------------- | ------------------------------------------------------------ |
        | /word                                          | 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可 |
        | ?word                                          | 向光标之上寻找一个字符串名称为 word 的字符串                 |
        | n                                              | 这个 n 是英文按键。代表重复前一个搜寻的动作                  |
        | N                                              | 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作 |
        | :n1,n2s/word1/word2/g                          | 在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代为 word2 |
        | `:1,$s/word1/word2/g `或 `:%s/word1/word2/g`   | 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 |
        | `:1,$s/word1/word2/gc` 或` :%s/word1/word2/gc` | 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！且在取代前显示提示字符给用户确认 (confirm) 是否需要取代 |

    - ==删除、复制和贴上==

        | 快捷键      | 说明                                                         |
        | ----------- | ------------------------------------------------------------ |
        | x,X         | 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符(相当于 [backspace] 亦即是退格键) |
        | nx          | n 为数字，连续向后删除 n 个字符                              |
        | dd          | 删除游标所在的那一整行                                       |
        | ndd         | n 为数字。删除光标所在的向下 n 行                            |
        | d1G         | 删除光标所在到第一行的所有数据                               |
        | dG          | 删除光标所在到最后一行的所有数据                             |
        | yy          | 复制游标所在的那一行                                         |
        | nyy         | n 为数字。复制光标所在的向下 n 行                            |
        | y1G         | 复制游标所在行到第一行的所有数据                             |
        | yG          | 复制游标所在行到最后一行的所有数据                           |
        | p, P        | p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行     |
        | u           | 复原前一个动作，后退                                         |
        | [Ctrl]+r    | 重做上一个动作，前进                                         |
        | .（小数点） | 重复前一个动作的意思。 如果想要重复删除、重复贴上等等动作，按下小数点『.』就好了 |

    - ==命令模式切换到编辑模式的按钮==

        | 命令  | 说明                                                         |
        | ----- | ------------------------------------------------------------ |
        | i，I  | 进入输入模式(Insert mode)：<br/>i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』 |
        | a，A  | 进入输入模式(Insert mode)：<br/>a 为『从目前光标所在的下一个字符处开始输入』， A 为『从光标所在行的最后一个字符处开始输入』 |
        | o, O  | 进入输入模式(Insert mode)：<br/>这是英文字母 o 的大小写。o 为『在目前光标所在的下一行处输入新的一行』； O 为在目前光标所在处的上一行输入新的一行 |
        | r, R  | 进入取代模式(Replace mode)：<br/>r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止 |
        | [Esc] | 退出编辑模式，回到一般模式中                                 |

    - ==一般模式切换到指令行模式的可用的按钮==

        w、q、！的组合使用

        `:w [filename]`:将编辑的数据存储成另一个档案

        `:r [filename]`:在编辑的数据中，读入另一个档案的数据

    - ==vim 环境的变更==

        | 快捷键    | 说明                                               |
        | --------- | -------------------------------------------------- |
        | :set nu   | 显示行号，设定之后，会在每一行的前缀显示该行的行号 |
        | :set nonu | 与 set nu 相反，为取消行号                         |

    - ==**批量操作**==

        批量注释

        - 块选择模式

            **Ctrl + v** 进入块选择模式，然后移动光标选中你要注释的行，再按大写的 **I** 进入行首插入模式输入注释符号如 **//** 或 **#**，输入完毕之后，按两下 **ESC**，**Vim** 会自动将你选中的所有行首都加上注释，保存退出完成注释。

            取消注释：

            **Ctrl + v** 进入块选择模式，选中你要删除的行首的注释符号，注意 **//** 要选中两个，选好之后按 **d** 即可删除注释，**ESC** 保存退出。

        - 替换命令

            使用下面命令在指定的行首添加注释。

            使用名命令格式： **:起始行号,结束行号s/^/注释符/g**（注意冒号）。

            ​								**:起始行号,结束行号s#^#注释符#g**（注意冒号）。

            ​						

            取消注释：

            使用名命令格式： **:起始行号,结束行号s/^注释符//g**（注意冒号）。

            ​								**:起始行号,结束行号s#^注释符##g**（注意冒号）。

    - ==分屏操作==

        **启动分屏**

        1. **打开文件时候分屏**

            通过参数`-On`或者`-on`来启动分屏

            - **n** 代表整数，表示将整个屏幕分成n部分
            - 大写 *O* 表示进行**垂直方向**分屏，小写 *o* 表示水平方向进行分屏

        2. **vim 内部分屏**

            - 通过命令`:vsplit filename`或者`vsp filename`可实现垂直方向分屏

            - 通过命令 `:split filename` 或者`:sp filename`可实现水平方向分屏

        3. **垂直分割打开当前的文件**

            Vim命令行模式下执行命令 `Ctrl+w v` 可将当前打开的文件进行垂直分割

            上述命令 `Ctrl+w v` 的意思是：先同时按键 **Ctrl** 和 **w**，再按键 **v**

        4. **水平分割当前打开的文件**

            Vim命令行模式下执行命令 `Ctrl+w s` 可将当前打开的文件进行水平分割

            上述命令 `Ctrl+w s` 表示先同时按键 **Ctrl** 和 **w**，再按键 **s**

        **切换分屏**

        1. **轮流切换**

            在Vim命令行模式下，同时按键 `Ctrl+w w` 可以在当前的分割屏幕中按顺时针方向切换屏幕

        2. 按指定方向切换

            命令 `Ctrl+w h` 用于把光标移到左边的屏幕中

            命令 `Ctrl+w l` 用于把光标移到右边的屏幕中

            命令 `Ctrl+w j` 用于把光标移到下边的屏幕中

            命令 `Ctrl+w l` 用于把光标移到上边的屏幕中

        **设置分屏大小**

        - 命令 `Ctrl+w =` 表示设置所有的分屏幕都有相同的高度

        - 命令 `Ctrl+w +` 用于增加当前屏幕的高度

        - 命令 `Ctrl+w -` 用于减少当前屏幕的高度

        **关闭分屏**

        - 命令 `Ctrl+w c` 用于关闭当前操作的Vim分屏幕
        - `:only`关闭除当前分屏外的其他分屏
        - 命令`Ctrl+w q`关闭当前分屏，可关闭最后一个分屏

### .vimrc文件

.vimrc 是控制 vim 行为的配置文件，位于 ~/.vimrc，不论 vim 窗口外观、显示字体，还是操作方式、快捷键、插件属性均可通过编辑该配置文件将 vim 调教成最适合你的编辑器

一些基础配置：

```sh
" 开启文件类型侦测
filetype on
" 根据侦测到的不同类型加载对应的插件
filetype plugin on
" 让配置变更立即生效
autocmd BufWritePost $MYVIMRC source $MYVIMRC
" 开启实时搜索功能
set incsearch
" 搜索时大小写不敏感
set ignorecase
" 关闭兼容模式
set nocompatible
" vim 自身命令行模式智能补全
set wildmenu
```



### .vim 目录

.vim/ 目录是存放所有插件的地方。vim 有一套自己的脚本语言 vimscript，通过这种脚本语言可以实现与 vim 交互，达到功能扩展的目的。一组 vimscript 就是一个 vim 插件，vim 的很多功能都由各式插件实现。

## 源码安装vim

发行套件的软件源预编译的vim不是全功能的最新版，需要自己进行源码安装

```sh
git clone git@github.com:vim/vim.git
cd vim/
./configure --with-features=huge --enable-pythoninterp --enable-rubyinterp --enable-luainterp --enable-perlinterp --with-python-config-dir=/usr/lib/python2.7/config/ --enable-gui=gtk2 --enable-cscope --prefix=/usr
make
make instal
```
