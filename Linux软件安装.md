# Linux软件安装



## RPM包统一命名规则

RPM 二进制包的命名需遵守统一的命名规则，用户通过名称就可以直接获取这类包的版本、适用平台等信息。

RPM 二进制包命名的一般格式如下：

```
包名-版本号-发布次数-发行商-Linux平台-适合的硬件平台-包扩展名
```

【1】例如，RPM 包的名称是`httpd-2.2.15-15.el6.centos.1.i686.rpm`，其中：

- httped：软件包名。这里需要注意，**httped 是包名**，而 ==httpd-2.2.15-15.el6.centos.1.i686.rpm 通常称为**包全名**==，包名和包全名是不同的，在某些 Linux 命令中，有些命令（如包的安装和升级）使用的是包全名，而有些命令（包的查询和卸载）使用的是包名，一不小心就会弄错。
- 2.2.15：包的版本号，版本号的格式通常为**`主版本号.次版本号.修正号`**。
- 15：二进制包发布的次数，表示此 RPM 包是第几次编程生成的。
- el6：软件发行商，el6 表示此包是由 Red Hat 公司发布，适合在 RHEL 6.x (Red Hat Enterprise Unux) 和 CentOS 6.x 上使用。
- centos：表示此包适用于 CentOS 系统。
- i686：表示此包使用的硬件平台

- rpm：RPM 包的扩展名，表明这是编译好的二进制包，可以使用 rpm 命令直接安装。此外，还有以 src.rpm 作为扩展名的 RPM 包，这表明是源代码包，需要安装生成源码，然后对其编译并生成 rpm 格式的包，最后才能使用 rpm 命令进行安装。



## RPM包默认安装路径

通常情况下，RPM 包采用系统默认的安装路径，所有安装文件会按照类别分散安装到下表所示的目录中。

|    安装路径     |           含 义            |
| :-------------: | :------------------------: |
|      /etc/      |      配置文件安装目录      |
|    /usr/bin/    |    可执行的命令安装目录    |
|    /usr/lib/    | 程序所使用的函数库保存位置 |
| /usr/share/doc/ | 基本的软件使用手册保存位置 |
| /usr/share/man/ |      帮助文件保存位置      |

### RPM包的安装

**安装 RPM 的命令格式为：**

```
[root@localhost ~]# rpm -ivh 包全名
```

==注意一定是包全名。涉及到包全名的命令，一定要注意路径，可能软件包在光盘中，因此需提前做好设备的挂载工作。==

此命令中各选项参数的含义为：

- -i：安装（install）;
- -v：显示更详细的信息（verbose）;
- -h：打印 #，显示安装进度（hash）;

【1】此命令还可以一次性安装多个软件包，仅需将包全名用空格分开即可，如下所示：

```
[root@localhost ~]# rpm -ivh a.rpm b.rpm c.rpm
```

如果还有其他安装要求（比如强制安装某软件而不管它是否有依赖性），可以通过以下选项进行调整：

- -nodeps：不检测依赖性安装。软件安装时会检测依赖性，确定所需的底层软件是否安装，如果没有安装则会报错。如果不管依赖性，想强制安装，则可以使用这个选项。注意，这样不检测依赖性安装的软件基本上是不能使用的，所以不建议这样做。
- -replacefiles：替换文件安装。如果要安装软件包，但是包中的部分文件已经存在，那么在正常安装时会报"某个文件已经存在"的错误，从而导致软件无法安装。使用这个选项可以忽略这个报错而覆盖安装。
- -replacepkgs：替换软件包安装。如果软件包已经安装，那么此选项可以把软件包重复安装一遍。
- -force：强制安装。不管是否已经安装，都重新安装。也就是 -replacefiles 和 -replacepkgs 的综合。
- -test：测试安装。不会实际安装，只是检测一下依赖性。
- -prefix：指定安装路径。为安装软件指定安装路径，而不使用默认安装路径。

***

【2】apache 服务安装完成后，可以尝试启动：

```
[root@localhost ~]# service 服务名 start|stop|restart|status
```

各参数含义：

- start：启动服务；
- stop：停止服务；
- restart：重启服务；
- status: 查看服务状态；

### RPM包的升级

使用如下命令即可实现 RPM 包的升级：

```
[root@localhost ~]# rpm -Uvh 包全名
```

> -U（大写）选项的含义是：如果该软件没安装过则直接安装；若没安装则升级至最新版本。

```
[root@localhost ~]# rpm -Fvh 包全名
```

> -F（大写）选项的含义是：如果该软件没有安装，则不会安装，必须安装有较低版本才能升级。

### RPM包的卸载

**RPM 软件包的卸载要考虑包之间的依赖性。**

RPM 软件包的卸载很简单，使用如下命令即可：

```
[root@localhost ~]# rpm -e 包名
```

> -e 选项表示卸载，也就是 erase 的首字母。
>
> RPM 软件包的卸载命令支持使用“-nocteps”选项，即可以不检测依赖性直接卸载，但此方式不推荐使用，因为此操作很可能导致其他软件也无法征程使用。

# rpm命令查询软件包

使用 rpm 做查询命令的格式如下：

```
[root@localhost ~]# rpm 选项 查询对象
```

## rpm -q：查询软件包是否安装

用 rpm 查询软件包是否安装的命令格式为：

```
[root@localhost ~]# rpm -q 包名
-q 表示查询，是 query 的首字母。
```

## rpm -qa：查询系统中所有安装的软件包

`rpm -q 包名`命令，采用这种方式可以找到含有包名的所有软件包。

## rpm -qi：查询软件包的详细信息

通过 rpm 命令可以查询软件包的详细信息，命令格式如下：

```
[root@localhost ~]# rpm -qi 包名
-i 选项表示查询软件信息，是 information 的首字母。
```

还可以查询未安装软件包的详细信息，命令格式为： 

```
[root@localhost ~]# rpm -qip 包全名
-p 选项表示查询未安装的软件包，是 package 的首字母。
```

> 注意，这里用的是包全名，且未安装的软件包需使用“绝对路径+包全名”的方式才能确定包。

## rpm -ql：命令查询软件包的文件列表

使用rpm命令可以查询到已安装软件包含的所有文件及各自安装路径

```
[root@localhost ~]# rpm -ql 包名
-l 选项表示列出软件包所有文件的安装目录。
```

同时，rpm 命令还可以查询未安装软件包中包含的所有文件以及打算安装的路径，命令格式如下：

```
[root@localhost ~]# rpm -qlp 包全名
```

==注意，由于软件包还未安装，因此需要使用“绝对路径+包全名”的方式才能确定包。==

## rpm -qf：命令查询系统文件属于哪个RPM包

**rpm -ql 命令是通过软件包查询所含文件的安装路径，rpm 还支持反向查询，即查询某系统文件所属哪个 RPM 软件包。**

```
[root@localhost ~]# rpm -qf 系统文件名
-f 选项的含义是查询系统文件所属哪个软件包，是 file 的首字母。
```

==注意，只有使用 RPM 包安装的文件才能使用该命令，手动方式建立的文件无法使用此命令。==

***

## rpm -qR：查询软件包的依赖关系

使用 rpm 命令安装 RPM 包，需考虑与其他 RPM 包的依赖关系。rpm -qR 命令就用来**查询某已安装软件包依赖的其他包**。

```
[root@localhost ~]# rpm -qR 包名
-R（大写）选项的含义是查询软件包的依赖性，是 requires 的首字母。
```

***





# RPM包验证和数字证书

## Linux RPM 包校验

RPM 包校验可用来判断已安装的软件包（或文件）是否被修改，此方式可使用的命令格式分为以下 3 种。

① -Va 选项表示校验系统中已安装的所有软件包。

```
[root@localhost ~]# rpm -Va
```

***

②-V 选项表示校验指定 RPM 包中的文件，是 verity 的首字母。

```
[root@localhost ~]# rpm -V 已安装的包名
```

***

③-Vf 选项表示校验某个系统文件是否被修改。

```
[root@localhost ~]# rpm -Vf 系统文件名
```



## Linux RPM数字证书验证

RPM包检验方法只能用来校验已安装的RPM包及其安装文件，如果RPM包本身就被动过手脚，此方法将无法解决问题，需要使用RPM数字证书验证方法

**数字证书，又称数字签名，由软件开发商直接发布。Linux 系统安装数字证书后，若 RPM 包做了修改，此包携带的数字证书也会改变，将无法与系统成功匹配，软件无法安装。**

使用数字证书验证 RPM 包的方法具有如下 2 个**特点**：

1. 必须找到==原厂的公钥文件==，然后才能进行安装。
2. 安装 RPM 包会提取 RPM 包中的证书信息，然后和本机安装的原厂证书进行验证。如果验证通过，则允许安装；如果验证不通过，则不允许安装并发出警告。

【1】数字证书默认会放到系统中`/etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6`位置处，通过以下命令也可验证：

```
#系统中的数字证书位置
[root@localhost ~]# ll /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
-rw-r--r--.1 root root 1706 6 月 26 17:29 /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
```

==安装数字证书的命令如下：==

```
[root@localhost ~]# rpm --import /efc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
--import表示导入数字证书
```

==卸载数字证书可以使用 -e 选项，命令如下：==

```
[root@localhost ~]# rpm -e gpg-pubkey-c105b9de-4ead3a3
```

***

# Linux提取RPM包文件(cpio命令)详解

cpio命令用于从归档包存入和读取文件，换句话说，cpio 命令可以从归档包中提取文件（或目录），也可以将文件（或目录）复制到归档包中。

> 归档包，也可称为文件库，其实就是 cpio 或 tar 格式的文件，该文件中包含其他文件以及一些相关信息（文件名、访问权限等）。归档包既可以是磁盘中的文件，也可以是磁带或管道。

**cpio 命令可以看做是备份或还原命令，因为它可以将数据（文件）备份到 cpio 归档库，也可以利用 cpio 文档库对数据进行恢复。**

***

> 使用 cpio 命令备份或恢复数据，需注意以下几点：
>
> - 使用 cpio 备份数据时如果使用的是绝对路径，那么还原数据时会自动恢复到绝对路径下；同理，如果备份数据使用的是相对路径，那么数据会还原到相对路径下。
> - cpio 命令无法自行指定备份（或还原）的文件，需要目标文件（或目录）的完整路径才能成功读取，因此此命令常与 find 命令配合使用。
> - cpio 命令恢复数据时不会自动覆盖同名文件，也不会创建目录（直接解压到当前文件夹）。

#### cpio 命令主要有以下 3 种基本模式：

**1、"-o" 模式：指的是 copy-out 模式，就是==把数据备份到文件库中==，命令格式如下：**

```
[root@localhost ~]# cpio -o[vcB] > [文件丨设备]
```

> 各选项含义如下：
>
> - -o：copy-out模式，备份；
> - -v：显示备份过程；
> - -c：使用较新的portable format存储方式；
> - -B：设定输入/输出块为 5120Bytes，而不是模式的 512Bytes；

**2、"-i" 模式：指的是 copy-in 模式，就是把数据从文件库中==恢复==，命令格式如下：**

```
[root@localhost ~]# cpio -i[vcdu] < [文件|设备]
```

> 各选项的含义为：
>
> - -i：copy-in 模式，还原；
> - -v：显示还原过程；
> - -c：较新的 portable format 存储方式；
> - -d：还原时自动新建目录；
> - -u：自动使用较新的文件覆盖较旧的文件；

**3、"-p" 模式：指的是复制模式，使用 -p 模式可以从某个目录读取所有文件，但并不将其备份到 cpio 库中，而是直接==复制为其他文件==。**

【1】例如，使用 -p 将 /boot/ 复制到 /test/boot 目录中可以执行如下命令：

```
[root@localhost ~]# cd /tmp/
#进入/tmp/目录
[root@localhost tmp]#rm -rf*
#删除/tmp/目录中的所有数据
[root@localhost tmp]# mkdir test
#建立备份目录
[root@localhost tmp]# find /boot/ -print | cpio -p /tmp/test
#备份/boot/目录到/tmp/test/目录中
[root@localhost tmp]# ls test/boot
#在/tmp/test/目录中备份出了/boot/目录
```

## 使用 cpio 命令提取 RPM 包中指定文件

在服务器使用过程，如果系统文件被误删改或误删除，可以考虑使用cpio命令提取出原RPM包中的所需的系统文件，从而修复被误操作的源文件

RPM 包允许**逐个**提取包中文件，使用的命令格式如下：

```
[root@localhost ~]# rpm2cpio 包全名|cpio -idv .文件绝对路径、
rpm2cpio 就是将 RPM 包转换为 cpio 格式的命令，通过 cpio 命令即可从 cpio 文件库中提取出指定文件。
```

举个例子，假设我们不小心把 /bin/ls 命令删除了，通常有以下 2 种方式修复：

1. 将 coreutils-8.4-19.el6.i686 包（包含 ls 命令的 RPM 包）通过 -force 选项再安装一遍；
2. 使用 cpio 命令从 coreutils-8.4-19.el6.i686 包中提取出 /bin/ls 文件，然后将其复制到相应位置；

***

# Linux SRPM源码包安装

SRPM可直译为“源代码形式的RPM包”。SRPM包是软件以源码发布后直接装成RPM包的产物。

**SRPM包与RPM包的不同点**

| 文件格式 | 文件名格式  | 直接安装与否 |  内含程序类型  | 可否修改参数并编译 |
| :------: | :---------: | :----------: | :------------: | :----------------: |
|   RPM    |   xxx.rpm   |      可      |     已编译     |        不可        |
|   SRPM   | xxx.src.rpm |     不可     | 未编译的源代码 |         可         |

> ==SRPM包是未经编译的源码包，无法直接安装软件==，需要经过以下2步：
>
> - 将SRPM包编译成二进制的RPM包
> - 使用编译完成的RPM包安装软件

***

## rpmbuild命令的安装

rpmbuild 命令也是一个程序，但是这个程序不会默认安装，所以要想使用 rpmbuild 命令就必须提前安装。**使用 rpm 命令来安装 rpmbuild 命令**。

安装过程如下：

```
[root@localhost~]#rpm -ivh /mnt/cdroin/Packages/rpm-build-4.8.0-27.el6.i686.rpm
Preparing...
###################
[100%]
1:rpm-build
###################
[100%]
```

> 出现两个 100% 才证明 rpmbuild 安装成功。

***

## rpmbuild命令安装SRPM包

如果只安装SRPM包，不用修改源代码，直接用rpmbuild命令即可：

```
[root@localhost ~]# rpmbuild [选项] 包全名
```

可使用如下 2 个选项：

- -rebuild：编译 SRPM 包生成 RPM 二进制包；
- -recompile：编译 SRPM 包，同时安装。

> ==需要注意的是，SRPM 本质上仍属于 RPM 包，所以安装时仍需考虑包之间的依赖性，要先安装它的依赖包，才能正确安装。==



通过 ls 命令可以看到，rpmbuild 目录下有几个子目录，其各自保存的文件类别如表所示。

| 文件名  |                           文件内容                           |
| :-----: | :----------------------------------------------------------: |
|  BUILD  |                 编译过程中产生的数据保存位置                 |
|  RPMS   |              编译成功后，生成的 RPM 包保存位置               |
| SOURCES |       从 SRPM 包中解压出来的源码包（*.tar.gz）保存位置       |
|  SPECS  | 生成的设置文件的安装位置。第二种安装方法就是利用这个文件进行安装的 |
|  SRPMS  |                      放置 SRPM 包的位置                      |

**使用 rpmbuild命令编译 SRPM 包经历了以下 3 个过程：**

1. 先把 SRPM 包解开，得到源码包；
2. 对源码包进行编译，生成二进制文件；
3. 把二进制文件重新打包生成 RPM 包。

***

## 利用*.spec文件安装

想利用 .spec 文件安装软件，需先将 SRPM 包解开。当然，可以使用 rpmbuild 命令解开 SRPM 包，但这里选择另一种方式，即使用 `rpm -i` 命令，如下所示：

```
[root@localhost ~]# rpm -i httpd-2.2.15-5.el6.src.rpm
```

> -i 选项用于安装 rpm 包时表示安装，但对于 SRPM 包的安装来说，这里**只会将 .src.rpm 包解开后将个文件放置在当前目录下的 rpmbuild 目录中**，==并不涉及安装操作==。



通过此命令，也可以在当前目录下生成rpmbuild目录，这里的 rpmbuild 目录中仅有 SOURCES 和 SPECS 两个子目录。其中，SOURCES 目录中放置的是源码，SPECS 目录中放置的是设置文件。

**接下来使用 SPECS 目录中的设置文件生成 RPM 包，命令如下：**

```
[root@localhost ~]# rpmbuild -ba /root/rpmbuild/SPECS/httpd.spec
```

> 其中，-ba 选项的含义是编译，会同时生成 RPM 二进制包和 SRPM 源码包。这里还可以使用 -bb 选项用来仅生成 RPM 二进制包。

***

# Linux重建RPM数据库（修复）

RPM 包在升级过程被强行退出、RPM 包安装意外中断等误操作，都可能使 RPM 数据库出现故障，后果是当安装、删除、査询软件包时，请求无法执行。

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303210920587.png)

#### 重建RPM数据库

1、删除当前系统中已损坏的RPM 数据库

```
[root@localhost ~]# rm -f /var/lib/rpm/_db.*
```

2、重建RPM数据库

```
[root@localhost -]# rpm -rebuilddb
```

***

另一种情况则是系统被入侵

1、对于要校验的文件或命令，找到它属于哪个软件包，如下命令所示：

```
[root@localhost ~]# rpm -qf/etc/rc.d/init.d/smb
samba-3.0.23c-2
```

2、使用 -dump 选项查看每个文件的信息，使用 grep 命令提取对应文件信息：

```
[root@localhost ~]# rpm -ql -dump samba|grep /etc/rc.d/init.d/smb
/etc/rc.d/init.d/smb 2087 1157165946 b1c26e5292157a83cadabe851bf9b2f9 0100755 root root 1 0 0X
```

> 此信息中，“2087”表示 smb 文件最初的字符数，“b1c26e5292157a83cadabe851bf9b2f9”表示 smb 文件的 MD5 校验值，“0755 root root”表示文件权限及所有者、所属组。

3、查看实际的文件，通过对比文件大小，所有人、所属组、权限、MD5 校验值等数据，判断文件是否被改动过：

```
[root@localhost ~]# ls -l /etc/rc.d/init.d/smb
-rwxr-xr-x 1 root root 2087 Sep 2 2006/etc/rc.d/init.d/smb
[root@localhost ~]# md5sum /etc/rc.d/init.d/smb
b1c26e5292157a83cadabe851bf9b2f9 /etc/rc.d/init.d/smb
```

> 以上校验结果显示，系统的 /etc/rc.d/init.d/smb 文件的信息和通过 rpm-ql-dump Samba 命令获取的信息一致，因此可以断定此文件没有被入侵或更改。

注意，如果确信 RPM 数据库遭到了修改，就要基于从光盘或者其他值得信赖的来源处获得的 Samba RPM 文件进行检査。

```
[root@localhost~]# rpm -ql --dump -p /mnt/cdrom/Fedora/RPMS/samba-3.0.23c-2.i386.rpm | grep /etc/rc.d/init.d/smb
warning: samba-3.0.23c-2.i386.rpm: Header V3 DSA signature: NOKEY, key ID 412a&62
/etc/rc.d/init.d/smb 2087 1157165946 b1c26e5292157a83cadabe851 bf9b2f9 0100755 root root 1 0 0 X

（得到的结果如果和基于 RPM 数据库运行的命令结果不同，说明 RPM 数据库已被更改，就需要修正文件错误和系统漏洞，重建 RPM 数据库。）
```

# RPM包的依赖性及其解决方案

RPM软件包（包含SRPM包）的依赖性主要体现在**RPM包安装与卸载**的过程中。

【1】如果采用最基础的方式（基础服务器方式）安装Linux系统，则gcc这个软件是没有安装的，需要自己手工安装。当你使用rpm命令安装gcc软件的RPM包，就会发生依赖性错误，错误提示信息如下：

```
[root@localhost ~]# rpm -ivh /mnt/cdrom/Packages/ gcc-4.4.6-4.el6.i686.rpm
error: Failed dependencies: <―依赖性错误
cloog-ppi >= 0.15 is needed by gcc-4.4.6-4.el6.i686
cpp = 4.4.6-4.el6 is needed by gcc-4.4.6-4.el6.i686
glibc-devel >= 2.2.90-12 is needed by gcc-4.4.6-4.el6.i686
```

==报错信息提示我们，如果要安装 gcc，需要先安装 cloog-ppl、cpp 和 glibc-devel 三个软件，这体现的就是 RPM 包的依赖性。==

> 除此之外，报错信息中还会明确给出各个依赖软件的版本要求：
>
> - ">="：表示版本要大于或等于所显示版本；
> - "<="：表示版本要小于或等于所显示版本；
> - "="：表示版本要等于所显示版本；

***

**Linux系统中，RPM包之间的依赖关系大致可分为以下3种：**

1. 树形依赖（A-B-C-D）：要想安装软件A，必须先安装 B，而安装 B 需要先安装 C…….解决此类型依赖的方法是从后往前安装，即先安装 D，再安装 C，然后安装 B，最后安装软件 A。
2. 环形依赖（A-B-C-D-A）：各个软件安装的依赖关系构成“环状”。解决此类型依赖的方法是用一条命令同时安装所有软件包，即使用 `rpm -ivh 软件包A 软件包B ...`。
3. 模型依赖：软件包的安装需要借助其他软件包的某些文件（比如库文件），解决模块依赖最直接的方式是通过 [http://www.rpmfind.net](http://www.rpmfind.net/) 网站找到包含此文件的软件包，安装即可。

***

# Linux yum源及配置

**yum**，全称“Yellow dog Updater, Modified”，是一个专门**为了解决包的依赖关系而存在的软件包管理器**。就好像 Windows 系统上可以通过 360 软件管家实现软件的一键安装、升级和卸载，Linux 系统也提供有这样的工具，就是 yum。



yum是改进型的RPM软件管理器，它很好的解决了RPM所面临的软件包依赖问题。yum在服务器端存有所有的RPM包，并将各个包之间的依赖关系记录在文件中，当管理员使用yum安装RPM包时，yum 会先从服务器端下载包的依赖性文件，通过分析此文件从服务器端一次性下载所有相关的 RPM 包并进行安装。

***

yum 软件可以用 rpm 命令安装，安装之前可以通过如下命令查看 yum 是否已安装：

```
[root@localhost ~]# rpm -qa | grep yum
yum-metadata-parser-1.1.2-16.el6.i686
yum-3.2.29-30.el6.centos.noarch
yum-utils-1.1.30-14.el6.noarch
yum-plugin-fastestmirror-1.1.30-14.el6.noarch
yum-plugin-security-1.1.30-14.el6.noarch
```

> ==使用 yum 安装软件包之前，需指定好 yum 下载 RPM 包的位置，此位置称为 yum 源。换句话说，yum 源指的就是软件安装包的来源。==

***

## 网络yum源搭建

> 一般情况下，只要你的主机网络正常，可以直接使用网络 yum 源，不需要对配置文件做任何修改



【1】**网络 yum 源配置文件位于 /etc/yum.repos.d/ 目录下，文件扩展名为"*.repo"（只要扩展名为 "*.repo" 的文件都是 yum 源的配置文件）。**

```
[root@localhost ~]# ls /etc/yum.repos.d/
CentOS-Base.repo
CentOS-Media.repo
CentOS-Debuginfo.repo.bak
CentOS-Vault.repo
```

可以看到，该目录下有 4 个 yum 配置文件，通常情况下 CentOS-Base.repo 文件生效。

```
[root@localhost yum.repos.d]# vim /etc/yum.repos.d/ CentOS-Base.repo
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/? release= $releasever&arch=$basearch&repo=os
baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
…省略部分输出…
```

此文件中含有 5 个 yum 源容器，这里只列出了 base 容器，其他容器和 base 容器类似。base 容器中各参数的含义分别为：

- [base]：容器名称，一定要放在[]中。
- name：容器说明，可以自己随便写。
- mirrorlist：镜像站点，这个可以注释掉。
- baseurl：我们的 yum 源服务器的地址。默认是 CentOS 官方的 yum 源服务器，是可以使用的。如果你觉得慢，则可以改成你喜欢的 yum 源地址。
- enabled：此容器是否生效，如果不写或写成 enabled 则表示此容器生效，写成 enable=0 则表示此容器不生效。
- gpgcheck：如果为 1 则表示 RPM 的数字证书生效；如果为 0 则表示 RPM 的数字证书不生效。
- gpgkey：数字证书的公钥文件保存位置。不用修改。

***

## 本地yum源

**在无法联网的情况下，yum可以考虑用本地光盘（或安装映像文件）作为yum源。**

Linux 系统安装映像文件中就含有常用的 RPM 包，我们可以使用压缩文件打开映像文件（iso文件），进入其 Packages 子目录

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303210956178.png)

***在 /etc/yum.repos.d/ 目录下有一个 CentOS-Media.repo 文件，此文件就是以本地光盘作为 yum 源的模板文件，只需进行简单的修改即可，步骤如下：**

1、放入 CentOS 安装光盘，并挂载光盘到指定位置。命令如下：

```
[root@localhost ~]# mkdir /mnt/cdrom
#创建cdrom目录，作为光盘的挂载点
[root@localhost ~]# mount /dev/cdrom /mnt/cdrom/
mount: block device/dev/srO is write-protected, mounting read-only
#挂载光盘到/mnt/cdrom目录下
```

2、修改其他几个 yum 源配置文件的扩展名，让它们失效，因为只有扩展名是"*.repo"的文件才能作为 yum 源配置文件。当也可以删除其他几个 yum 源配置文件，但是如果删除了，当又想用网络作为 yum 源时，就没有了参考文件，所以最好还是修改扩展名。 命令如下：

```
[root@localhost ~]# cd /etc/yum.repos.d/
[root@localhost yum.repos.d]# mv CentOS-Base, repo CentOS-Base.repo.bak
[root@localhost yum.repos.d]#mv CentOS-Debuginfo.repo CentOS-Debuginfo.repo.bak
[root@localhost yum.repos.d]# mv CentOS-Vault.repo CentOS-Vault.repo.bak
```

3、修改光盘 yum 源配置文件 CentOS-Media.repo，参照以下方修改：

```
[root@localhost yum.repos.d]# vim CentOS-Media.repo
[c6-media]
name=CentOS-$releasever - Media
baseurl=file:///mnt/cdrom
#地址为你自己的光盘挂载地址
#file:///media/cdrom/
#file:///media/cdrecorder/
#注释这两个的不存在地址
gpgcheck=1
enabled=1
#把enabled=0改为enabled=1, 让这个yum源配置文件生效
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
```

***

## yum命令

### yum查询命令

使用 yum 对软件包执行查询操作，常用命令可分为以下几种：

- **yum list：查询所有已安装和可安装的软件包。例如：**

  ```
  [root@localhost yum.repos.d]# yum list
  #查询所有可用软件包列表
  Installed Packages
  #已经安装的软件包
  ConsdeKit.i686 0.4.1-3.el6
  @anaconda-CentOS-201207051201 J386/6.3
  ConsdeKit-libs.i686 0.4.1-3.el6 @anaconda-CentOS-201207051201 J386/6.3
  …省略部分输出…
  Available Packages
  #还可以安装的软件包
  389-ds-base.i686 1.2.10.2-15.el6 c6-media
  389-ds-base-devel.i686 1.2.10.2-15.el6 c6-media
  #软件名 版本 所在位置（光盘）
  …省略部分输出…
  ```

- **yum list 包名：查询执行软件包的安装情况。例如：**

  ```
  [root@localhost yum.repos.d]# yum list samba
  Available Packages samba.i686 3.5.10-125.el6 c6-media
  #查询 samba 软件包的安装情况
  ```

- **yum search 关键字：从 yum 源服务器上查找与关键字相关的所有软件包。例如：**

  ```
  [root@localhost yum.repos.d]# yum search samba
  #搜索服务器上所有和samba相关的软件包
  ========================N/S Matched:
  samba =============================
  samba-client.i686：Samba client programs
  samba-common.i686：Files used by both Samba servers and clients
  samba-doc.i686: Documentation for the Samba suite
  …省略部分输出…
  Name and summary matches only, use"search all" for everything.
  ```

- **yum info 包名：查询执行软件包的详细信息。例如：**

  ```
  [root@localhost yum.repos.d]# yum info samba
  #查询samba软件包的信息
  Available Packages <-没有安装
  Name : samba <-包名
  Arch : i686 <-适合的硬件平台
  Version : 3.5.10 <―版本
  Release : 125.el6 <—发布版本
  Size : 4.9M <—大小
  Repo : c6-media <-在光盘上
  …省略部分输出…
  ```

***

### yum安装命令

yum 安装软件包的命令基本格式为：

```
[root@localhost yum.repos.d]# yum -y install 包名
```

> 其中：
>
> - install：表示安装软件包。
> - -y：自动回答 yes。如果不加 -y，那么每个安装的软件都需要手工回答 yes；

### yum升级命令

使用yum升级软件包，**需要确保yum源服务器中软件包的版本比本机安装的软件包版本高**

**yum 升级软件包常用命令如下：**

- `yum -y update`：升级所有软件包。不过考虑到服务器强调稳定性，因此该命令并不常用。
- `yum -y update 包名`：升级特定的软件包。

***

### yum卸载命令

使用yum卸载软件包时，会同时卸载所有与该包有依赖关系的其他软件包，即便有依赖包属于系统运行必备文件，也会被yum卸载，带来的直接后果就是使系统崩溃。

==除非能确定卸载此包以及它的所有依赖包不会对系统产生影响，否则不要使用yum卸载软件包==

**yum 卸载命令的基本格式如下：**

```
[root@localhost yum.repos.d]# yum remove 包名
#卸载指定的软件包
```

***

# Linux yum管理软件组方法详解

> yum 命令除了可以对软件包进行查询，安装，升级和卸载外，还可完成对软件包组的查询，安装和卸载操作



## yum查询软件组包含的软件

既然是软件包组，说明包含不只一个软件包，通过 yum 命令可以查询某软件包组中具体包含的软件包，命令格式如下：

```
[root@localhost ~]#yum groupinfo 软件组名
#查询软件组中包含的软件
```

## yum安装软件组

使用 yum 安装软件包组的命令格式如下：

```
[root@localhost ~]#yum groupinstall 软件组名
#安装指定软件组，组名可以由grouplist查询出来
```

## yum命令卸载软件组

yum 卸载软件包组的命令格式如下：

```
[root@localhost ~]# yum groupremove 软件组名
#卸载指定软件组
```

***

# Linux源码安装和卸载

> 注意，本节使用的源码包，指的是软件所有源代码的压缩包，其后缀名为 ".tar.gz" 或 ".tar.bz2"；而 SRPM 源码包本质上属于 RPM 包，也就是源码的RPM包，其文件后缀为 ".src.rpm"。虽然都叫源码包，但不是一码事。

> 软件的源代码，也就是软件的原始数据，任何人都可以通过源代码查看该软件的设计架构和实现方法但软件源代码无法再计算机中直接运行安装，需要将源代码通过编译转换为计算机可以识别的机器语言，然后才可以安装。



- **安装gcc之前，可以使用`rpm -q gcc`查看是否已经安装**
- **如果未安装，考虑到安装gcc所依赖的软件包太多，可以使用yum安装gcc**



- **安装make之前查找是否已经安装**
- **如果未安装，可使用 `yum -y install make` 命令直接安装 make。**

***

## Linux源码包安装软件和 卸载

具体步骤见连接：

http://c.biancheng.net/view/2952.html

***

# Linux源码包快速升级方法

> Linux系统中更新用源码包安装的软件，除了卸载重装这种简单粗暴的方法外，还可以下载补丁文件更新源码包，用新的源码包重新编译安装软件，
> >使用补丁文件更新源码包，省去； 用./configured生产新的Makefile文件，还省去了大量的编译工作，因此效率更高。

## 补丁文件的生产和使用

Linux 系统中可以使用 diff 命令对比出新旧软件的不同，并生成补丁文件。

diff 命令基本格式为：

```
[root@localhost ~]# diff 选项 old new
#比较old和new文件的不同
```

此命令中可使用如下几个选项：

- -a：将任何文档当作文本文档处理；
- -b：忽略空格造成的不同；
- -B：忽略空白行造成的不同；
- -I：忽略大小写造成的不同；
- -N：当比较两个目录时，如果某个文件只在一个目录中，则在另一个目录中视作空文件；
- -r：当比较目录时，递归比较子目录；
- -u：使用同一输出格式；

***



#### 卸载指定的rpm软件

```
rpm -e 完整软件名 {-e|–erase} –》擦除
```



- 辅助选项：
  - –force：强制安装所指定的rpm软件包
  - –nodeps：安装、升级或卸载软件时，忽略依赖关系 no dependencies
  - -h：以“#”号显示安装的进度（50个hash符号）
  - -v：显示安装过程中的详细信息



#### **卸载有依赖关系的多个软件时**

- 依赖其他程序的软件包需要先卸载 ；
- 同时指定多个软件名进行卸载 。



***





# RPM包和源码包的安装方式下选择



> 注意，由于SRPM包本质上依然为RPM包，所以这节将SRPM包安装归属于RPM包安装方式



## 优先选择系统自带的 RPM 包

> 通常情况下，开发商提供的软件都具有一段时间的维护期

`借助yum自动升级，再加上系统持续维护软件（不断进行软件升级），可以保证我们的系统始终保持在最新的状态，安全性也会更好`

> 使用yum安装的RPM软件包具有容易安装，卸载和升级的特点，而且还提供查询和验证的功能，安装时有数字证书的保护。

***

## 选择软件官网提供的 RPM 包（或者存储此包不同版本的网址）

> 某些特殊软件，Linux（及其发行版）系统可能并不会提供其 RPM 软件包，例如 CentOS 不提供 NTFS 的相关模块。这种情况下，就需要我们自行去该软件官网上搜索，看是否提供有和自己 Linux 系统相匹配的 RPM 安装包。除此之外，如果官网上提供有可下载该软件不同版本的网址，可以直接将其作为 yum 源，从而实现软件的自动安装和升级。

***

## 用源码包安装特殊软件

对于有某些特殊用途的软件，开发商根本不提供 RPM 包，这里也不建议你自行制作 SRPM 包从而得到 RPM 包，毕竟多数初学者只使用一台电脑，如果你管理相同的 100 台电脑，则此方式才能凸显它的价值。

***

## 用源码包安装新版软件

> 可以使用源码包将新软件安装到 /usr/local/ 目录中，因为 Linux 系统允许同一软件的 2 个版本同时存在，且多数软件的不同版本之间不会相互干扰。唯一需要注意的是，你要确定所使用的命令作用于哪一版软件。
>
> 总的来说，使用 RPM 包安装和使用源码包安装软件各有优缺点，不过，如果有 RPM 包的话，还是建议优先选择 RPM 包安装软件，毕竟后期管理起来更方便。当然，如果软件的架构差异太大，或者无法解决软件依赖性的问题，与其花大把的时间和精力解决软件之间的依赖，不如直接使用源码包的方式安装软件。

***

# 静态函数库和动态函数库

**函数库中的函数并不是以源代码的形式存在，而是经过编译后生成的二进制文件，这些文件无法独立运行。只有链接到编写的程序中才能运行**

***

Linux系统中的函数库分为2种，分别是**静态函数库（简称静态库）**和**动态函数库（也称为共享函数库，简称动态库或共享库）**。它们的主要区别在于，==程序调用函数时，将函数整合到程序中的时机不同==：

- 静态函数库在程序编译时就会整合到程序中，换句话说，程序运行前函数库就已经被加载。这样做的好处是程序运行时不再需要调用外部函数库，可直接执行；缺点也很明显，所有内容都整合到程序中，编译文件会比较大，且一旦静态函数库改变，程序就需要重新编译。
- 动态函数库在程序运行时才被加载（如图所示），程序中只保存对函数库的指向（程序编译仅对其做简单的引用）。<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303220928726.png" alt="img" style="zoom:67%;" />

***

**使用动态库的好处：**

程序生成的可执行程序体积比较小，且升级函数库时无需队对整个程序程序编译；

**缺点：**

如果程序执行时函数库出现问题，则程序将不能正确运行


> Linux 系统中，静态函数库文件扩展名是 ".a"，文件通常命令为 libxxx.a（xxx 为文件名）；动态函数库扩展名为 ".so"，文件通常命令为 libxxx.so.major.minor（xxx 为文件名，major 为主版本号，minor 为副版本号）。

目前，Linux 系统中大多数都是动态函数库（主要考虑到软件的升级方便），其中被系统程序调用的函数库主要存放在 "/usr/lib" 和 "/lib" 中；Linux 内核所调用的函数库主要存放在 "/lib/modules" 中。`注意，函数库（尤其是动态函数库）的存放位置非常重要，轻易不要做更改。`

## Linux函数库的安装

【1】例如，安装 curses 函数库命令如下：

```
[root@Linux ~]# yum install ncurses-devel
```

【2】如何查看可执行程序调用了哪些函数库呢？通过以下命令即可：

```
[root@localhost ~]# ldd -v 可执行文件名
-v 选项的含义是显示详细版本信息（不是必须使用）。
```

***

**如果函数库安装后仍无法使用（运行程序时会提示找不到某个函数库），这时就需要对函数库的配置文件进行手动调整，也很简单，只需进行如下操作：**

1. 将函数库文件放入指定位置（通常放在 "/usr/lib" 或 "/lib" 中），然后把函数库所在目录写入 "/etc/ld.so.conf" 文件。例如：

   ```
   [root@localhost ~]# cp *.so /usr/lib/
   #把函数库复制到/usr/lib/目录中
   [root@localhost ~]# vi /etc/ld.so.conf
   #修改函数库配置文件
   include ld.so.conf.d/*.conf
   /usr/lib
   #写入函数库所在目录（其实/usr/lib/目录默认已经被识别）
   ```

   > 注意，这里写入的是函数库所在的目录，而不单单是函数库的文件名。另外，如果自己在其他目录中创建了函数库文件，这里也可以直接在 "/etc/ld.so.conf" 文件中写入函数库文件所在的完整目录。

2. 使用 ldconfig 命令重新读取 /etc/ld.so.conf 文件，把新函数库读入缓存。命令如下：

   ```
   [root@localhost ~]# ldconfig
   #从/etc/ld.so.conf文件中把函数库读入缓存
   [root@localhost ~]# ldconfig -p
   #列出系统缓存中所有识别的函数库
   ```

***

# 脚本程序包及安装方法（以webmin为例）

> Webmin是一个基于Web的系统管理界面，借助任何支持表格和表单的浏览器（和File Manager 模块所需要的Java），就可以设置用户账号，apache，DNS，文件共享等。

> Webmin 包括一个简单的 Web 服务器和许多 CGI 程序，这些程序可以直接修改系统文件，比如 /etc/inetd.conf 和 /etc/passwd。Web 服务器和所有的 CGI 程序都是用 Perl 5 编写的，没有使用任何非标准 Perl 模块。也就是说，Webmin 是一个用 Perl 语言写的、可以通过浏览器管理 Linux 的软件。

## webmin安装步骤

1. 首先下载 Webmin 软件（这里下载的是 webmin-1.610.tar.gz。）

2. 接下来解压缩软件，命令如下：

   ```
   [root@localhost ~]# tar -zxvf webmin-1.610.tar.gz
   ```

3. 进入解压目录，命令如下：

   ```
   [root@localhost ~]# cd webmin-1.610
   ```

   

 