# Vim文本编辑器



[TOC]







## 安装vim

CentOS 系统中，使用如下命令即可安装 Vim：

```
yum install vim
```

如果你想省略手动输入“y”的过程，希望全自动安装，可以使用如下这条命令：

```
yum -y install vim
```

## Vi和Vim之间的关系

Vi 编辑器是 Unix 系统最初的编辑器。它使用控制台图形模式来模拟文本编辑窗口，允许查看文件中的行、在文件中移动、插入、编辑和替换文本。

在 GNU 项目中，程序员在将 Vi 编辑器移植到开源世界的同时，决定对其作一些改进。由于改进后的 Vi 不再是以前 Unix 中的那个原始的 Vi 编辑器了，开发人员也就将它重命名为“Vi improved”，也就是 Vim。

> GNU 项目，英文全称为“GNU is Not Unix”，简单的说，就是一个开发类 Unix 操作系统的项目，GNU 操作系统是由 GNU 软件包及其第三方的免费软件包组成，所以其最大的特点就是免费。

## Vim三种工作模式

vim有三种工作模式，分别是命令模式、输入模式和编辑模式

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021540922.png)

### 命令模式

使用vim编辑文件时，默认处于命令模式。此模式下，可使用方向键（上、下、左、右键）或 k、j、h、i 移动光标的位置，还可以对文件内容进行复制、粘贴、替换、删除等操作。

CentOS 6.x 系统中 Vim 处于命令模式的状态示意图：

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021542081.png)

### 输入模式

在输入模式下，vim可以对文件执行写操作，当编辑文件完成后按 Esc 键即可返回命令模式。

| 快捷键 |                           功能描述                           |
| :----: | :----------------------------------------------------------: |
|   i    | 在当前光标所在位置插入随后输入的文本，光标后的文本相应向右移动 |
|   I    | 在光标所在行的行首插入随后输入的文本，行首是该行的第一个非空白字符，相当于光标移动到行首执行 i 命令 |
|   o    | 在光标所在行的下面插入新的一行。光标停在空行首，等待输入文本 |
|   O    | 在光标所在行的上面插入新的一行。光标停在空行的行首，等待输入文本 |
|   a    |           在当前光标所在位置之后插入随后输入的文本           |
|   A    | 在光标所在行的行尾插入随后输入的文本，相当于光标移动到行尾再执行a命令 |

### 编辑模式

编辑模式用于对文件中的指定内容执行保存，查找或替换操作

使 Vim 切换到编辑模式的方法是在命令模式状态下按“：”键，此时 Vim 窗口的左下方出现一个“：”符号，这是就可以输入相关指令进行操作了。指令执行后 Vim 会自动返回命令模式。如想直接返回命令模式，按 Esc 即可。

## Vim的基本操作

打开文件：

```
[root@itxdl ~]# vim /test/vi.test
```

**Vim 打开文件的快捷方法**

|     Vi 使用的选项      |                       说 明                       |
| :--------------------: | :-----------------------------------------------: |
|      vim filename      |   打开或新建一个文件，并将光标置于第一行的首部    |
|    vim -r filename     |           恢复上次 vim 打开时崩溃的文件           |
|    vim -R filename     |      把指定的文件以只读方式放入 Vim 编辑器中      |
|     vim + filename     |       打开文件，并将光标置于最后一行的首部        |
|     vi +n filename     |        打开文件，并将光标置于第 n 行的首部        |
| vi +/pattern filename  | 打幵文件，并将光标置于第一个与 pattern 匹配的位置 |
| vi -c command filename |       在对文件进行编辑前，先执行指定的命令        |

#### 使用Vim进行编辑

1、vim插入文本

同输入模式表格

2、vim查找文本

| 快捷键 |             功能描述             |
| :----: | :------------------------------: |
|  /abc  | 从光标所在位置向前查找字符串 abc |
| /^abc  |      查找以 abc 为行首的行       |
| /abc$  |      查找以 abc 为行尾的行       |
|  ?abc  | 从光标所在为主向后查找字符串 abc |
|   n    |   向同一方向重复上次的查找指令   |
|   N    |   向相反方向重复上次的查找指定   |

> 如果在文件中并没有找到所要查找的字符串，则在文件底部会出现 "Pattern not found" 提示

==在查找过程中需要注意的是，要查找的字符串是严格区分大小写的，如查找 "shenchao" 和 "ShenChao" 会得到不同的结果。==

`如果想忽略大小写，则输入命令 ":set ic"；调整回来输入":set noic"。`

3、vim替换文本

|     快捷键      |                           功能描述                           |
| :-------------: | :----------------------------------------------------------: |
|        r        |                    替换光标所在位置的字符                    |
|        R        | 从光标所在位置开始替换字符，其输入内容会覆盖掉后面等长的文本内容，按“Esc”可以结束 |
|   :s/a1/a2/g    |            将当前光标所在行中的所有 a1 用 a2 替换            |
| :n1,n2s/a1/a2/g |          将文件中 n1 到 n2 行中所有 a1 都用 a2 替换          |
|   :g/a1/a2/g    |                将文件中所有的 a1 都用 a2 替换                |

例如，要将某文件中所有的 "root" 替换为 "liudehua"，则有两种输入命令，分别为：

```
:1, $s/root/liudehua/g
或
:%s/root/liudehua/g
```

> 如果刚才的命令变成 `:10,20 s/root/liudehua/g`，则只替换从第 10 行到第 20 行的 "root"。

4、vim删除文本

| 快捷键  |                功能描述                |
| :-----: | :------------------------------------: |
|    x    |         删除光标所在位置的字符         |
|   dd    |             删除光标所在行             |
|   ndd   |   删除当前行（包括此行）后 n 行文本    |
|   dG    | 删除光标所在行一直到文件末尾的所有内容 |
|    D    |        删除光标位置到行尾的内容        |
| :a1,a2d |     函数从 a1 行到 a2 行的文本内容     |

> 注意，被删除的内容并没有真正删除，都放在了剪贴板中。将光标移动到指定位置处，按下 "p" 键，就可以将刚才删除的内容又粘贴到此处。

5、其他

- 某些情况下，可能需要把两行进行连接。比如说，下面的文件中有两行文本，现在需要将其合并成一行（实际上就是将两行间的换行符去掉）。可以直接在命令模式中按下 "J" 键，按下前后如图 10 所示。

- 如果不小心误删除了文件内容，则可以通过 "u" 键来撤销刚才执行的命令。如果要撤销刚才的多次操作，可以多按几次 "u" 键。

#### Vim保存退出文本

Vim的保存和退出是在编辑模式中进行的

|    命令     |                      功能描述                      |
| :---------: | :------------------------------------------------: |
|     :wq     |               保存并退出 Vim 编辑器                |
|    :wq!     |             保存并强制退出 Vim 编辑器              |
|     :q      |              不保存就退出 Vim 编辑器               |
|     :q!     |           不保存，且强制退出 Vim 编辑器            |
|     :w      |             保存但是不退出 Vim 编辑器              |
|     :w!     |                    强制保存文本                    |
| :w filename |                另存到 filename 文件                |
|     x！     | 保存文本，并退出 Vim 编辑器，更通用的一个 vim 命令 |
|     ZZ      |                直接退出 Vim 编辑器                 |

**需要注意的是，"w!" 和 "wq!" 等类似的指令，通常用于对文件没有写权限的时候（显示 readonly，如图 所示），但如果你是文件的所有者或者 root 用户，就可以强制执行。**

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021603667.png)

## Vim移动光标快捷键

**需要注意的是，表中所有的快捷键都在命令模式（默认状态）下直接使用。**

#### vim快捷方向键

| 快捷键 |                        功能描述                        |
| :----: | :----------------------------------------------------: |
|   h    |                    光标向左移动一位                    |
|   j    | 光标向下移动一行（以回车为换行符），也就是光标向下移动 |
|   k    |           光标向上移动一行（也就是向上移动）           |
|   l    |                    光标向右移动一位                    |

#### vim光标以单词为单位移动

某些情形下，可能需要光标迅速移动至一行中的某个位置，将光标以单词为单位进行移动就会很方便。

|  快捷键  |              功能描述               |
| :------: | :---------------------------------: |
|  w 或 W  |    光标移动至下一个单词的单词首     |
|  b 或 B  |    光标移动至上一个单词的单词首     |
|  e 或 E  |    光标移动至下一个单词的单词尾     |
| nw 或 nW | n 为数字，表示光标向右移动 n 个单词 |
| nb 或 nB | n 为数字，表示光标向左移动 n 个单词 |

#### vim光标移动至行首或行尾

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021609557.png)

#### vim光标移动至指定字符

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021607371.png)

#### vim光标移动到指定行

| 快捷键 |                           功能描述                           |
| :----: | :----------------------------------------------------------: |
|   gg   |                      光标移动到文件开头                      |
|   G    |                      光标移动至文件末尾                      |
|   nG   |                 光标移动到第 n 行，n 为数字                  |
|   :n   | **编辑模式下使用的快捷键**，可以将光标快速定义到指定行的行首 |

#### vim光标移动到匹配的括号处

程序员在编辑程序时，经常会为将光标移动到与一个 "(" 匹配的 ")" （对于 [] 和 {} 也是一样的）处而感到头疼。Vim 里面提供了一个非常方便地査找匹配括号的命令，这就是 "%"。

比如，在 /etc/init.d/sshd 脚本文件中（最好还是复制后练习，小心驶得万年船），想迅速地将光标定位到与第 49 行的 "{" 相对应的 "}" 处，则可以将光标先定位在 "{" 处，然后再使用 "％" 命令，使之定位在 "}" 处，如图 6 所示。

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021611088.png)

## Vim撤销和恢复撤销快捷键

使用vim编辑文件内容时，经常会有如下两种需求：

- 对文件内容做了修改之后，却发现整个修改过程是错误或者没有必要的，想将文件恢复到修改之前的样子。
- 将文件内容恢复之后，经过仔细考虑，又感觉还是刚才修改过的内容更好，想撤销之前做的恢复操作。

|  快捷键   |                             功能                             |
| :-------: | :----------------------------------------------------------: |
| u（小写） |  undo 的第 1 个字母，功能是撤销最近一次对文本做的修改操作。  |
|  Ctrl+R   |    Redo 的第 1 个字母，功能是恢复最近一次所做的撤销操作。    |
| U（大写） | 第一次会撤销对一行文本（光标所在行）做过的全部操作，第二次使用该命令会恢复对该行文本做过的所有操作。 |

> 注意，以上这 3 种命令都必须在 Vim 编辑器处于命令模式时才能使用。

## Vim可视化模式及其用法

**在vim中，如果想选中目标文本，就需要调整vim进入可视化模式。**

|       命令       |                             功能                             |
| :--------------: | :----------------------------------------------------------: |
|    v（小写）     | 又称字符可视化模式，此模式下目标文本的选择是以字符为单位的，也就是说，该模式下要一个字符一个字符的选中要操作的文本。 |
|    V（大写）     | 又称行可视化模式，此模式化目标文本的选择是以行为单位的，也就是说，该模式化可以一行一行的选中要操作的文本。 |
| Ctrl+v（组合键） | 又称块可视化模式，该模式下可以选中文本中的一个矩形区域作为目标文本，以按下 Ctrl+v 位置作为矩形的一角，光标移动的终点位置作为它的对角。 |

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021618135.gif)

> 需要注意的是，当选中文本并做完相应操作（例如选中文件并按 p 键将其复制到剪贴板中）后，Vim 会自动从可视化模式转换为命令模式。当然，也可以再次按 v（或者 V、Ctrl+v）手动退出可视化模式。

**可视化模式支持使用的命令**

|   命令    |                             功能                             |
| :-------: | :----------------------------------------------------------: |
|     d     |                     删除选中的部分文本。                     |
|    dd     |                          删除当前行                          |
|     D     | 删除选中部分所在的行，和 d 不同之处在于，即使选中文本中有些字符所在的行没有都选中，删除时也会一并删除。 |
|     y     |                  将选中部分复制到剪贴板中。                  |
|    yy     |                          复制当前行                          |
| p（小写） |               将剪贴板中的内容粘贴到光标之后。               |
| P（大写） |               将剪贴板中的内容粘贴到光标之前。               |
| u（小写） |           将选中部分中的大写字符全部改为小写字符。           |
| U（大写） |           将选中部分中的小写字符全部改为大写字符。           |
|     >     | 将选中部分右移（缩进）一个 tab 键规定的长度（CentOS 6.x 中，一个tab键默认相当于 8 个空白字符的长度）。 |
|     <     | 将选中部分左移一个 tab 键规定的长度（CentOS 6.x 中，一个tab键默认相当于 8 个空白字符的长度）。 |

## Vim多窗口编辑模式

例如，在査看 /etc/passwd 时需要参考 /etc/shadow，有两种办法可以实现：

1. 先使用 Vim 打开第一个文件，接着输入命 令 ":sp/etc/shadow" 水平切分窗口，然后按回车键；如果想垂直切分窗口则可以输入 ":vs/etc/shadow";
2. 可以直接执行命令"vim -o 第一个文件名 第二个文件名"，也就是 "vim-o /etc/passwd /etc/shadow"。

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021629072.png)

==切换到另一个文件窗口，可以按 "Ctrl+WW" 快捷键。==

==如果想将一个文件的内容全部复制到另一个文件中，则可以输入命令 ":r 被复制的文件名"，即可将导入文件的全部内容复制到当前光标所在行下面。==

## Vim批量注释和自定义注释快捷方式

1. 连续行的注释其实可以用替换命令来完成。换句话说，在指定范围行加"#"注释，可以使用 ":起始行，终止行 s/^/#/g"，例如：

```
:1,10s/^/#/g
```

**表示在第 1~10 行行首加"#"注释。"^"意为行首；"g"表示执行替换时不询问确认。如果希望每行交互询问是否执行，则可将 "g" 改为 "c"。**

2. 取消连续行注释，则可以使用 ":起始行，终止行s/^#//g"，例如：

```
:1,10s/^#//g
意为将行首的"#"替换为空，即删除。
```

3. 添加"//"注释要稍微麻烦一些，命令格式为 ":起始行，终止行 s/^/\/\//g"。例如：

```
:1,5s/^/\/\//g
```

> 表示在第 1~5 行行首加"//"注释，因为 "/" 前面需要加转义字符 "\"，所以写出来比较奇特。

**:map 快捷键 执行命令**

> 如定义快捷键 "Ctrl+P" 为在行首添加 "#" 注释，可以执行 ":map^P l#<Esc>"。其中 "^P" 为定义快捷键 "Ctrl+P"。注意：必须同时按 "Ctrl+V+P" 快捷键生成 "^P" 方可有效，或先按 "Ctrl+V" 再按 "Ctrl+P" 也可以，直接输入 "^P" 是无效的。

> "l#<Esc>" 就是此快捷键要触发的动作，"l" 为在光标所在行行首插入，"#" 为要输入的字符，"<Esc>" 表示退回命令模式。"<Esc>" 要逐个字符输入，不可直接按键盘上的 Esc 键。
>
> 置成功后，直接在任意需要注释的行上按 "Ctrl+P" 快捷键，就会自动在行首加上 "#" 注释。取消此快捷键定义，输入 ":unmap^P" 即可。

> 需要在末尾注释中加入自己的邮箱，则可以直接定义每次按快捷键 "Ctrl+E" 实现插入邮箱，定义方法为 ":map^E asamlee@itxdl.net<Esc>"。其中 "a" 表示在当前字符后插入，"samlee@itxdl.net" 为插入的邮箱，"<Esc>" 表示插入后返回命令模式。

## 显示行号

**在命令模式下输入":set nu"即可显示每一行的行号**

**如果想要取消行 号，则再次输入":set nonu"即可。**

## Vim配置文件

Vim 配置文件分为系统配置文件和用户配置文件：

- 系统配置文件位于 Vim 的安装目录（默认路径为 /etc/.vimrc）；
- 用户配置文件位于主目录 ~/.vimrc，即通过执行 `vim ~/.vimrc` 命令即可对此配置文件进行合理修改。通常情况下，Vim 用户配置文件需要自己手动创建。

> 注意，Vim 用户配置文件比系统配置文件的优先级高，换句话说，Vim 启动时，会优先读取 Vim 用户配置文件（位于主目录中的），所以我们只需要修改用户配置文件即可（不建议直接修改系统配置文件）。

|                           设置参数                           |                           功能描述                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936 set termencoding=utf-8 set encoding=utf-8 | 设置编码格式，encoding 选项用于缓存的文本、寄存器、Vim 脚本文件等；fileencoding 选项是 Vim 写入文件时采用的编码类型；termencoding 选项表示输出到终端时采用的编码类型。 |
|                      set nu set number                       | nu 是 number 的缩写，所以上面两个配置命令是完全等效的，二选一即可。取消行号可使用 set nonu。 |
|                        set cursorline                        |                       突出显示当前行。                       |
| set mouse=a set selection=exclusive set selectmode=mouse,key |   Vim 编辑器里默认是不启用鼠标的，通过此设置即可启动鼠标。   |
|                        set autoindent                        |           设置自动缩进，即每行的缩进同上一节相同。           |
|                        set tabstop=4                         |                 设置 Tab 键宽度为 4 个空格。                 |

注意，表中各配置参数前面可以添加冒号（：），也可以省略，两种写法都可以。

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303021649184.png)