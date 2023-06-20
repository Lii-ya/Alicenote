# Linux基础命令



[toc]













进入root用户

```
[root@localhost ~]$ su
效果：
[root@localhost ~]#
```

> 打开终端快捷键：shift+ctrl+n

## cd命令

cd 命令，是 Change Directory 的缩写，用来切换工作目录


Linux命令按照来源方式，可分为两种，分别时Shell内置命令和外部命令。所谓Shell内置命令，就是Shell自带的命令，这些命令是没有执行文件的；而外部命令就是由程序员单独开发的，所以会有命令的执行文件。*Linux 中的绝大多数命令是外部命令，而 cd 命令是一个典型的 Shell 内置命令，所以 cd 命令没有执行文件所在路径。*

cd命令后面可以跟一些特殊符号，表达固定含义：

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011043476.png)

用法：

1、

```
[root@localhost vbird]# cd ~
#表示回到自己的主目录，对于 root 用户，其主目录为 /root
[root@localhost ~]# cd
#没有加上任何路径，也代表回到当前登录用户的主目录
[root@localhost ~]# cd ~vbird
#代表切换到 vbird 这个用户的主目录，亦即 /home/vbird
```

2、

```
[root@localhost ~]# cd ..
#表示切换到目前的上一级目录，亦即是 /root 的上一级目录的意思；
```

需要注意的是，在 Linux 系统中，根目录确实存在 .（当前目录）以及 ..（当前目录的父目录）两个目录，但由于根目录是最顶级目录，因此根目录的 .. 和 . 的属性和权限完全一致，也就是说，根目录的父目录是自身。

3、

```
[root@localhost /]# cd -
#表示回到刚刚的那个目录
```

## pwd命令

**pwd 命令，是 Print Working Directory （打印工作目录）的缩写，功能是显示用户当前所处的工作目录**。该命令的基本格式为：

```
[root@localhost ~]# pwd
```

## find命令

```
find 起始路径 -name “被查找文件名”
```

被查找文件名，支持使用==通配符*==来做模糊查询

- 符号*表示通配符，即匹配任意内容（包含空）
- test* 表示匹配任何以test开头的内容
- *test，表示匹配任何以test结尾的内容
- *test\*,表示匹配任何包含test的内容

#### 按文件大小查找文件

```
find 起始路径 -size +|-n[K、M、G]
+ - 表示大于和小于
n表示大小数字
KMG表示大小单位，k（小写字母），M表示MB，G表示GB
```

## ls命令

**ls 命令，list 的缩写，是最常见的目录操作命令，其主要功能是显示当前目录下的内容。**此命令的基本格式为：

```
[root@localhost ~]# ls [选项] 目录名称
```

|                   选项                    |                             功能                             |
| :---------------------------------------: | :----------------------------------------------------------: |
|                    -a                     | 显示全部的文件，包括隐藏文件（开头为 . 的文件）也一起罗列出来，这是最常用的选项之一。 |
|                    -A                     | 显示全部的文件，连同隐藏文件，但不包括 . 与 .. 这两个目录。  |
|                    -d                     |         仅列出目录本身，而不是列出目录内的文件数据。         |
|                    -f                     | ls 默认会以文件名排序，使用 -f 选项会直接列出结果，而不进行排序。 |
|                    -F                     | 在文件或目录名后加上文件类型的指示符号，例如，* 代表可运行文件，/ 代表目录，= 代表 [socket](http://c.biancheng.net/socket/) 文件，\| 代表 FIFO 文件。 |
|                    -h                     | 以人们易读的方式显示文件或目录大小，如 1KB、234MB、2GB 等。  |
|                    -i                     |                    显示 inode 节点信息。                     |
|                    -l                     |                使用长格式列出文件和目录信息。                |
|                    -n                     |      以 UID 和 GID 分别代替文件用户名和群组名显示出来。      |
|                    -r                     | 将排序结果反向输出，比如，若原本文件名由小到大，反向则为由大到小。 |
|                    -R                     | 连同子目录内容一起列出来，等於将该目录下的所有文件都显示出来。 |
|                    -S                     |           以文件容量大小排序，而不是以文件名排序。           |
|                    -t                     |               以时间排序，而不是以文件名排序。               |
| --color=never --color=always --color=auto | never 表示不依据文件特性给予颜色显示。 always 表示显示颜色，ls 默认采用这种方式。 auto 表示让系统自行依据配置来判断是否给予颜色。 |
|                --full-time                |        以完整时间模式 （包含年、月、日、时、分）输出         |
|           --time={atime,ctime}            | 输出 access 时间或改变权限属性时间（ctime），而不是内容变更时间。 |

## grep命令

可以通过grep命令，从文件中通过关键字过滤文件行

```
grep [-n] 关键字 文件路径
```

- 选项-n ，可选，表示在结果中显示匹配行的行号
- 参数，关键字，必填，表示过滤的关键字，带有空格或其他特殊符号，建议使用“ ”将关键字包围起来
- 参数，文件路径，必填，表示要过滤内容的文件路径，可作为内容输入端口 

## WC命令

可以通过wc命令统计文件的行数、单词数量等

```
wc [-c -m -l -w]文件路径
```

- 选项 -c，统计bytes数量
- 选项 -m，统计字符数量
- 选项 - l ，统计行数
- 选项 -w，统计单词数量
- 参数，文件路径，被统计的文件，可作为内容输入端口

## 管道符|



![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303201343691.png)

嵌套使用

==将管道符左边命令的结果，作为右边命令的输入==

## mkdir命令

**mkdir 命令，是 make directories 的缩写，用于创建新目录，此命令所有用户都可以使用。**

mkdir命令的基本格式:

```
[root@localhost ~]# mkdir [-mp] 目录名
```

- -m 选项用于手动配置所创建目录的权限，而不再使用默认权限。
- -p 选项递归创建所有目录，以创建 /home/test/demo 为例，在默认情况下，你需要一层一层的创建各个目录，而使用 -p 选项，则系统会自动帮你创建 /home、/home/test 以及 /home/test/demo。

【1】建立目录

```
[root@localhost ~]#mkdir cangls
[root@localhost ~]#ls
anaconda-ks.cfg cangls install.log install.log.syslog
```

【2】使用-p选项递归建立目录

```
[root@localhost ~]# mkdir lm/movie/jp/cangls
mkdir:无法创建目录"lm/movie/jp/cangls":没有那个文件或目录
[root@localhost ~]# mkdir -p lm/movie/jp/cangls
[root@localhost ~]# ls
anaconda-ks.cfg cangls install.log install.log.syslog lm
[root@localhost ~]# ls lm/
movie
#这里只查看一级子目录，其实后续的jp目录、cangls目录都已经建立
```

【3】使用-m选项自定义目录权限

```
[root@localhost ~]# mkdir -m 711 test2
[root@localhost ~]# ls -l
drwxr-xr-x  3 root  root 4096 Jul 18 12:50 test
drwxr-xr-x  3 root  root 4096 Jul 18 12:53 test1
drwx--x--x  2 root  root 4096 Jul 18 12:54 test2
```

## rmdir命令

**rmdirremove empty directories 的缩写）命令用于删除目录**

```
[root@localhost ~]# rmdir [-p] 目录名
-p选项用于递归删除空目录
```

> rmdir 命令的作用十分有限，因为只能刪除空目录，所以一旦目录中有内容，就会报错。

## touch命令

**touch命令，创建文件及修改文件时间戳**

**touch 命令不光可以用来创建文件（当指定操作文件不存在时，该命令会在当前位置建立一个空文件），此命令更重要的功能是修改文件的时间参数（但当文件存在时，会修改此文件的时间参数）。**

```
[root@localhost ~]# touch [选项] 文件名
```

选项：

- -a：只修改文件的访问时间；
- -c：仅修改文件的时间参数（3 个时间参数都改变），如果文件不存在，则不建立新文件。
- -d：后面可以跟欲修订的日期，而不用当前的日期，即把文件的 atime 和 mtime 时间改为指定的时间。
- -m：只修改文件的数据修改时间。
- -t：命令后面可以跟欲修订的时间，而不用目前的时间，时间书写格式为 `YYMMDDhhmm`。

## ln命令

### ext文件系统

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011058818.png)

ext4文件系统会把分区主要分为两大部分：小部分用于保存文件的inode（i节点）信息；剩余的大部分用于保存block信息。

> inode 的默认大小为 128 Byte，用来记录文件的权限（r、w、x）、文件的所有者和属组、文件的大小、文件的状态改变时间（ctime）、文件的最近一次读取时间（atime）、文件的最近一次修改时间（mtime）、文件的数据真正保存的 block 编号。每个文件需要占用一个 inode。大家如果仔细查看，就会发现 inode 中是不记录文件名的，那是因为文件名记录在文件所在目录的 block 中。

> block 的大小可以是 1KB、2KB、4KB，默认为 4KB。block 用于实际的数据存储，如果一个 block 放不下数据，则可以占用多个 block。例如，有一个 10KB 的文件需要存储，则会占用 3 个 block，虽然最后一个 block 不能占满，但也不能再放入其他文件的数据。这 3 个 block 有可能是连续的，也有可能是分散的。

**1、每个文件都独自占用一个inode，文件内容由inode的记录来指向；**

**2、如果想要读取文件内容，就必须借助目录中记录的文件名找到该文件的inode，才能成功找到文件内容所在的block块；**

#### ln命令用于给文件创建链接

ln 命令用于给文件创建链接，根据 Linux 系统存储文件的特点，链接的方式分为以下 2 种：

- 软链接：类似于 Windows 系统中给文件创建快捷方式，即产生一个特殊的文件，该文件用来指向另一个文件，此链接方式同样适用于目录。
- 硬链接：我们知道，文件的基本信息都存储在 inode 中，而硬链接指的就是给一个文件的 inode 分配多个文件名，通过任何一个文件名，都可以找到此文件的 inode，从而读取该文件的数据信息。

```
[root@localhost ~]# ln [选项] 源文件 目标文件
```

选项：

- -s：建立软链接文件。如果不加 "-s" 选项，则建立硬链接文件；
- -f：强制。如果目标文件已经存在，则删除目标文件后再建立链接文件；

【1】创建硬链接

```
[root@localhost ~]# touch cangls
[root@localhost ~]# ln /root/cangls /tmp
#建立硬链接文件，目标文件没有写文件名，会和原名一致
#也就是/tmp/cangls 是硬链接文件
```

【2】创建软链接

```
[root@localhost ~]# touch bols
[root@localhost ~]# ln -s /root/bols /tmp
#建立软链接文件
```

==这里需要注意的是，软链接文件的源文件必须写成绝对路径，而不能写成相对路径（硬链接没有这样的要求）；否则软链接文件会报错。这是初学者非常容易犯的错误。==

### 对硬件链接的深度剖析

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011112485.png" alt="img" style="zoom: 67%;" />

> \#查看两个文件的详细信息，可以发现这两个文件的 inode 号是一样的，"ll"等同于"ls -l"。

**这里源文件和硬链接文件的 inode 号居然是一样的，那我们在查找文件的时候，到底找到的是哪一个文件呢？**

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011113133.png)

在inode信息中，是不会记录文件名称的，而是把文件名称在上级目录的block中。也就是说，目录的block中记录的是这个目录下所有一级子文件和子目录的文件名及inode的对应；而文件的block中记录的

**硬链接的特点：**

- 不论是修改源文件，还是修改硬件链接文件，另一个文件中的数据都会发生改变

- 不论是删除源文件，还是删除硬件链接文件，只要还有一个文件存在，这个文件（inode号相同的文件）都可以被访问。

- 硬链接不会建立新的 inode 信息，也不会更改 inode 的总数。
- 硬链接不能跨文件系统（分区）建立，因为在不同的文件系统中，inode 号是重新计算的。
- 硬链接不能链接目录，因为如果给目录建立硬链接，那么不仅目录本身需要重新建立，目录下所有的子文件，包括子目录中的所有子文件都需要建立硬链接，这对当前的 Linux 来讲过于复杂。

```
硬链接的限制比较多，既不能跨文件系统，也不能链接目录，而且硬链接文件之间除inode号是一样的之外。没有其他明显的特征，这些特征都使得硬链接并不常用
```

### 对软链接的深度剖析

**软链接也称作符号链接**

```
[root@localhost ~]# touch check
#建立源文件
[root@localhost ~]# ln -s /root/check /tmp/check-soft
#建立软链接文件
[root@localhost ~]# ll -id /root/check /tmp/check-soft
262154 -rw-r--r-- 1 root root 0 6月 19 11:30 /root/check
917507 lrwxrwxrwx 1 root root 11 6月 19 11:31 /tmp/ check-soft -> /root/check
#软链接和源文件的 inode 号不一致，软链接通过 -> 明显地标识出源文件的位置
#在软链接的权限位 lrwxrwxrwx 中，l 就代表软链接文件
```

==注意：软链接的源文件必须写绝对路径，否则建立的软链接文件就会报错，无法正常使用==

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011514919.png)

> **软链接和硬链接在原理上最主要的不同在于**：硬链接不会建立自己的 inode 索引和 block（数据块），而是直接指向源文件的 inode 信息和 block，所以硬链接和源文件的 inode 号是一致的；而软链接会真正建立自己的 inode 索引和 block，所以软链接和源文件的 inode 号是不一致的，而且在软链接的 block 中，写的不是真正的数据，而仅仅是源文件的文件名及 inode 号。

【1】软链接链接文件

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011519853.png" alt="img" style="zoom: 50%;" />

【2】软链接链接目录

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011522984.png" alt="img" style="zoom:67%;" />

通过上面两个例子我们可以总结出软链接的特点：

- 不论是修改源文件（check），还是修改硬链接文件（check-soft)，另一个文件中的数据都会发生改变。
- 删除软链接文件，源文件不受影响。而删除原文件，软链接文件将找不到实际的数据，从而显示文件不存在。
- 软链接会新建自己的 inode 信息和 block，只是在 block 中不存储实际文件数据，而存储的是源文件的文件名及 inode 号。
- 软链接可以链接目录。
- 软链接可以跨分区。

## cp命令

**cp 命令，主要用来复制文件和目录，同时借助某些选项，还可以实现复制整个目录，以及比对两文件的新旧而予以升级等功能。**

```
[root@localhost ~]# cp [选项] 源文件 目标文件
```

**选项：**

- -a：相当于 -d、-p、-r 选项的集合，这几个选项我们一一介绍；
- -d：如果源文件为软链接（对硬链接无效），则复制出的目标文件也为软链接；
- -i：询问，如果目标文件已经存在，则会询问是否覆盖；
- -l：把目标文件建立为源文件的硬链接文件，而不是复制源文件；
- -s：把目标文件建立为源文件的软链接文件，而不是复制源文件；
- -p：复制后目标文件保留源文件的属性（包括所有者、所属组、权限和时间）；
- -r：递归复制，用于复制目录；
- -u：若目标文件比源文件有差异，则使用该选项可以更新目标文件，此选项可用于对文件的升级和备用。

【1】如果需要改名复制，则命令如下：

```
[root@localhost ~]# cp cangls /tmp/bols
#改名复制
```

【2】复制软链接文件

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011534216.png" alt="img" style="zoom:67%;" />

这个例子说明，如果在复制软链接时不使用“-d”选项，则cp命令复制的是源文件，而不是软链接文件；只有加入了“-d”选项，才会复制软链接文件。==“-d”选项对硬链接是无效的==

## rm命令

**rm 是强大的删除命令，它可以永久性地删除文件系统中指定的文件或目录。在使用 rm 命令删除文件或目录时，系统不会产生任何提示信息。**

```
[root@localhost ~]# rm[选项] 文件或目录
```

选项：

- -f：强制删除（force），和 -i 选项相反，使用 -f，系统将不再询问，而是直接删除目标文件或目录。
- -i：和 -f 正好相反，在删除文件或目录之前，系统会给出提示信息，使用 -i 可以有效防止不小心删除有用的文件或目录。
- -r：递归删除，主要用于删除目录，可删除指定目录及包含的所有内容，包括所有的子目录和文件。

==注意，rm 命令是一个具有破坏性的命令，因为 rm 命令会永久性地删除文件或目录，这就意味着，如果没有对文件或目录进行备份，一旦使用 rm 命令将其删除，将无法恢复，因此，尤其在使用 rm 命令删除目录时，要慎之又慎。==

## mv命令

**mv 命令（move 的缩写），既可以在不同的目录之间移动文件或目录，也可以对文件和目录进行重命名。**

```
[root@localhost ~]# mv 【选项】 源文件 目标文件
```

选项：

- -f：强制覆盖，如果目标文件已经存在，则不询问，直接强制覆盖；
- -i：交互移动，如果目标文件已经存在，则询问用户是否覆盖（默认选项）；
- -n：如果目标文件已经存在，则不会覆盖移动，而且不询问用户；
- -v：显示文件或目录的移动过程；
- -u：若目标文件已经存在，但两者相比，源文件更新，则会对目标文件进行升级；

==需要注意的是，同 rm 命令类似，mv 命令也是一个具有破坏性的命令，如果使用不当，很可能给系统带来灾难性的后果。==

## echo命令

可以使用echo命令在命令行内输出指定内容

```
echo 输出的内容
```

==无需选项，只有一个参数==，表示要输出的内容，复杂内容可以用“ ”包围

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303201351780.png" alt="img" style="zoom: 80%;" />

### 反引号`

==被`包围的内容，会被作为命令执行，而非普通字符==
![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303201354752.png)

***

### 重定向符

\> 和 >>

\>，将左侧命令的结果，覆盖写入到符号右侧指定的文件中

\>>,将左侧命令的结果，追加写入到符号右侧指定的文件中<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303201404822.png" alt="img" style="zoom:67%;" />

***

## tail命令

使用tail命令，可以查看文件尾部内容，跟踪文件的最新更改

```
tail [-f -num]Linux路径
```

- 参数，Linux路径，表示被跟踪文件路径
- 选项，-f，表示持续跟踪
- 选项-num，表示，查看尾部多少行，不填默认10行



# PATH环境变量

## 什么是path环境变量，有什么用？

首先要了解一下，**which命令 ： 它用于查找某个命令所在的绝对路径。**

```
[root@localhost ~]# which rm
/bin/rm
[root@localhost ~]# which rmdir
/bin/rmdir
[root@localhost ~]# which ls
alias ls='ls --color=auto'
        /bin/ls
```

> 注意，ls 是一个相对特殊的命令，它使用 alias 命令做了别名，也就是说，我们常用的 ls 实际上执行的是 ls --color=auto。



```
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/root/bin
```

这里的echo命令用来输出PATH环境变量的值（$是PATH的前缀符号），PATH环境变量的内容是由一堆目录组成的，各目录之间用冒号“  ：”隔开。当执行某个命令时，Linux会依照PATH中包含的目录依次搜寻该命令的可执行文件，一旦找到，即正常执行，繁殖，则提示无法找到命令。

> 如果在 PATH 包含的目录中，有多个目录都包含某命令的可执行文件，那么会执行先搜索到的可执行文件。



【1】如果将 ls 命令移动到 /root 目录下，由于 PATH 环境变量中没有包含此目录，所有当直接使用 ls 命令名执行时，Linux 将无法找到此命令的可执行文件，并提示 No such file or directory，示例命令如下：

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303011551463.png" alt="img" style="zoom:67%;" />

【2】如果仍想使用 ls 命令，有 2 种方法，一种是直接将 /root 添加到 PATH 环境变量中，例如：

```
[root@localhost ~]# PATH=$PATH:/root
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/sbin:/usr/local/bin:/usr/bin:/bin:/root/bin:/root
[root@localhost ~]# ls
Desktop    Downloads    Music    post-install     Public    Videos
Documents  ls           Pictures post-install.org Templates
```

> 注意，这种方式只是临时有效，一旦退出下次再登陆的时候，$PATH 就恢复成了默认值。

【2.2】另一种方法是以绝对路径的方式使用此命令，例如：

```
[root@localhost ~]# /root/ls
Desktop    Downloads    Music    post-install     Public    Videos
Documents  ls           Pictures post-install.org Templates
```

