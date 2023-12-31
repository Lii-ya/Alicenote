# Linux用户和用户组管理

[TOC]







## Linux用户和组的关系

用户和用户组的对应关系有以下4种：

1. 一对一：一个用户可以存在一个组中，是组中的唯一成员；
2. 一对多：一个用户可以存在多个用户组中，此用户具有这多个组的共同权限；
3. 多对一：多个用户可以存在一个组中，这些用户具有和组相同的权限；
4. 多对多：多个用户可以存在多个组中，也就是以上 3 种关系的扩展。

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303221049572.png)



## Linux UID和GID（用户ID和组ID）

登录Linux系统时，虽然输入的是自己的用户名和密码，但其实Linux并不认识你的用户名称，它只认识用户名对应的ID号。**Linux系统将所有用户的名称与ID的对应关系都存储在/etc/password文件中。**`并无实际作用，只是为了方便用户记忆`



Linux 系统中，每个用户的 ID 细分为 2 种，分别是**用户 ID（User ID，简称 UID）和组 ID（Group ID，简称 GID）**，这与文件有拥有者和拥有群组两种属性相对应。

***



#### Linux是如何判别它的拥有者名称和群组名称？

每个文件都有自己的拥有者ID和群组ID，当显示文件属性时，系统会根据 /etc/passwd 和 /etc/group 文件中的内容，分别找到 UID 和 GID 对应的用户名和群组名，然后显示出来。

==etc/passwd 文件不能随意修改。==

***



## /etc/passwd内容解释

> 用户中的绝大多数是系统或服务正常运行所必需的用户，这种用户通常称为系统用户或伪用户。==系统用户无法用来登录系统，但也不能删除，因为一旦删除，依赖这些用户运行的服务或程序就不能正常执行，会导致系统问题。==


**不仅如此，每行用户信息都以 "：" 作为分隔符，划分为 7 个字段**，每个字段所表示的含义如下：

```
用户名：密码：UID（用户ID）：GID（组ID）：描述性信息：主目录：默认Shell
```

***

### UID

UID，就是用户ID。每个用户都有唯一的一个ID，Linux系统通过UID来识别不同的用户。

***

实际上，UID 就是一个 0~65535 之间的数，**不同范围的数字表示不同的用户身份**：


| UID 范围  |                           用户身份                           |
| :-------: | :----------------------------------------------------------: |
|     0     | 超级用户。UID 为 0 就代表这个账号是管理员账号。在 Linux 中，如何把普通用户升级成管理员呢？只需把其他用户的 UID 修改为 0 就可以了，这一点和 Windows 是不同的。不过不建议建立多个管理员账号。 |
|   1~499   | 系统用户（伪用户）。也就是说，此范围的 UID 保留给系统使用。其中，1~99 用于系统自行创建的账号；100~499 分配给有系统账号需求的用户。  其实，除了 0 之外，其他的 UID 并无不同，这里只是默认 500 以下的数字给系统作为保留账户，只是一个公认的习惯而已。 |
| 500~65535 | 普通用户。通常这些 UID 已经足够用户使用了。但不够用也没关系，2.6.x 内核之后的 Linux 系统已经可以支持 232 个 UID 了。 |



### GID

全称“Group ID”，简称“组ID”，表示用户初始组的组 ID 号。

##### **什么是初始组？**

初始组，指用户登陆时就拥有这个用户组的相关权限。每个用户的初始组只能有一个，通常就是将和此用户的用户名相同的组名作为该用户的初始组。

***

##### **什么是附加组？**

附加组，指用户可以加入多个其他的用户组，并拥有这些组的权限。每个用户只能有一个初始组，除初始组外，用户再加入其他的用户组，这些用户组就是这个用户的附加组。**附加组可以有多个，而且用户可以有这些附加组的权限**。

***







### 默认的shell

> shell就是Linux的命令解释器，是用户和Linux内核之间沟通的桥梁。

==Shell 命令解释器的功能就是将用户输入的命令转换成系统可以识别的机器语言。==

***

【例】手工添加了 lamp 用户，它使用的是 bash 命令解释器，那么这个用户就可以使用普通用户的所有权限。

- 如果把 lamp 用户的 Shell 命令解释器修改为 /sbin/nologin，那么，这个用户就不能登录了，例如：

  ```
  [root@localhost ~]# vi /etc/passwd
  lamp:x:502:502::/home/lamp:/sbin/nologin
  ```

- 因为 /sbin/nologin 就是禁止登录的 Shell。同样，如果在这里放入的系统命令，如 /usr/bin/passwd，例如：

  ```
  [root@localhost ~]#vi /etc/passwd
  lamp:x:502:502::/home/lamp:/usr/bin/passwd
  ```

***

那么这个用户可以登录，但登录之后就只能修改自己的密码。但是，这里不能随便写入和登陆没有关系的命令（如 ls），系统不会识别这些命令，同时也就意味着这个用户不能登录。



## /etc/shadow（影子文件）

**什么是影子文件？**

/etc/shadow 文件，用于==存储 Linux 系统中用户的密码信息==，又称为“影子文件”。

> 前面介绍了 /etc/passwd 文件，由于该文件允许所有用户读取，易导致用户密码泄露，因此 Linux 系统将用户的密码信息从 /etc/passwd 文件中分离出来，并单独放到了此文件中。

***

**/etc/shadow 文件只有 root 用户==拥有读权限==，其他用户==没有任何权限==，这样就保证了用户密码的安全性。**

***


同 /etc/passwd 文件一样，文件中每行代表一个用户，同样使用 ":" 作为分隔符，不同之处在于，每行用户信息被划分为 9 个字段。每个字段的含义如下：

```
用户名：加密密码：最后一次修改时间：最小修改时间间隔：密码有效期：密码需要变更前的警告天数：密码过期后的宽限时间：账号失效时间：保留字段
```



## /etc/group文件解析

> /ect/group 文件是用户组配置文件，==即用户组的所有信息都存放在此文件中。==


/etc/group 文件的内容可以通过 Vim 看到：

```
[root@localhost ~]#vim /etc/group
root:x:0:
bin:x:1:bin,daemon
daemon:x:2:bin,daemon
…省略部分输出…
lamp:x:502:
```

> 各用户组中，还是以 "：" 作为字段之间的分隔符，分为 4 个字段，每个字段对应的含义为：
>
> 组名：密码：GID：该用户组中的用户列表

***

#### 组中的用户

此字段列出每个群组包含的所有用户。需要注意的是，如果该用户组是这个用户的初始组，则该用户不会写入这个字段，可以这么理解，该字段显示的用户都是这个用户组的附加用户。

***

==每个用户都可以加入多个附加组，但是只能属于一个初始组。==一般情况下，用户的初始组就是在建立用户的同时建立的和用户名相同的组。



## /etc/gshadow文件内容解析

> 本节要将的 /etc/gshadow 文件也是如此，组用户信息存储在 /etc/group 文件中，而将组用户的密码信息存储在 /etc/gshadow 文件中。

***

#### 组密码

对于大多数用户来说，通常不设置组密码，因此该字段常为空，但有时为 "!"，指的是该群组没有组密码，也不设有群组管理员。



## 初始组和附加组

*初始组和附加组的解释前面有介绍。*

【1】举个例子，我们新建一个用户 lamp，并将其加入 users 群组中，执行命令如下：

```
[root@localhost ~]# useradd lamp  <--添加新用户
[root@localhost ~]# groupadd users  <--添加新群组
[root@localhost ~]# usermod -G users lamp  <--将用户lamp加入 users群组
[root@localhost ~]# grep "lamp" /etc/passwd /etc/group /etc/gshadow
/etc/passwd:lamp:x:501:501::/home/lamp:/bin/bash
/etc/group:users:x:100:lamp
/etc/group:lamp:x:501:
/etc/gshadow:users:::lamp
/etc/gshadow:lamp:!::
```

***

- lamp 群组是添加 lamp 用户时默认创建的群组，在 root 管理员使用 useradd 命令创建新用户时，若未明确指定该命令所属的初始组，useradd 命令会**默认创建**一个同用户名相同的群组，作为该用户的初始组。
- 正因为 lamp 群组是 lamp 用户的初始组，**该用户一登陆就会自动获取相应权限**，因此不需要在 /etc/group 的第 4 个字段额外标注。
- 附加组就不一样了，从例子中可以看到，我们将 lamp 用户加入 users 群组中，**由于 users 这个群组并不是 lamp 的初始组**，因此必须要在 /etc/group 这个文件中找到 users 那一行，==将 lamp 这个用户加入第 4 段中（群组包含的所有用户）==，这样 lamp 用户才算是真正加入到 users 这个群组中。

> #### **`一个用户可以所属多个附加组，但只能有一个初始组`**

***



#### 如何知道某用户所属哪些群组呢？

【2】例如，我们现在以 lamp 用户的身份登录系统，通过执行如下命令即可知晓当前用户所属的全部群组：

```
[root@localhost ~]# groups
lamp users
```

> ##### **`通过以上输出信息可以得知，lamp 用户同时属于 lamp 群组和 users 群组，而且，第一个出现的为用户的初始组，后面的都是附加组，所以 lamp 用户的初始组为 lamp 群组，附加组为 users 群组。`**

***



## /etc/login.defs：创建用户的默认设置文件

/etc/login.defs文件用于在创建用户时，对用户的一些基本属性做默认设置。

> 注意，该文件的用户默认配置对root用户无效。并且，当此文件中的配置与 `/etc/shadow`文件中的用户信息有冲突时，系统会以`/etc/passwd`和`/etc/shadow`为准。

**可自行使用 `vim /etc/login.defs` 命令查看该文件中的内容**



​                                                                                         **/etc/login.defs文件内容**

|          设置项          |                             含义                             |
| :----------------------: | :----------------------------------------------------------: |
| MAIL_DIR /var/spool/mail | 创建用户时，系统会在目录 /var/spool/mail 中创建一个用户邮箱，比如 lamp 用户的邮箱是 /var/spool/mail/lamp。 |
|   PASS_MAX_DAYS 99999    | 密码有效期，99999 是自 1970 年 1 月 1 日起密码有效的天数，相当于 273 年，可理解为密码始终有效。 |
|     PASS_MIN_DAYS 0      | 表示自上次修改密码以来，最少隔多少天后用户才能再次修改密码，默认值是 0。 |
|      PASS_MIN_LEN 5      | 指定密码的最小长度，默认不小于 5 位，但是现在用户登录时验证已经被 PAM 模块取代，所以这个选项并不生效。 |
|     PASS_WARN_AGE 7      | 指定在密码到期前多少天，系统就开始通过用户密码即将到期，默认为 7 天。 |
|       UID_MIN 500        | 指定最小 UID 为 500，也就是说，添加用户时，默认 UID 从 500 开始。注意，如果手工指定了一个用户的 UID 是 550，那么下一个创建的用户的 UID 就会从 551 开始，哪怕 500~549 之间的 UID 没有使用。 |
|      UID_MAX 60000       |                指定用户最大的 UID 为 60000。                 |
|       GID_MIN 500        | 指定最小 GID 为 500，也就是在添加组时，组的 GID 从 500 开始。 |
|      GID_MAX 60000       |                   用户 GID 最大为 60000。                    |
|     CREATE_HOME yes      | 指定在创建用户时，是否同时创建用户主目录，yes 表示创建，no 则不创建，默认是 yes。 |
|        UMASK 077         |               用户主目录的权限默认设置为 077。               |
|   USERGROUPS_ENAB yes    | 指定删除用户的时候是否同时删除用户组，准备地说，这里指的是删除用户的初始组，此项的默认值为 yes。 |
|  ENCRYPT_METHOD SHA512   | 指定用户密码采用的加密规则，默认采用 SHA512，这是新的密码加密模式，原先的 Linux 只能用 DES 或 MD5 加密。 |

***

## useradd命令：添加新的系统用户

**Linux 系统中，可以使用 useradd 命令新建用户，此命令的基本格式如下：**

```
[root@localhost ~]#useradd [选项] 用户名
```

​                                                                                       

​                                                                                                     **useradd命令常用选项**

|    选项     |                             含义                             |
| :---------: | :----------------------------------------------------------: |
|   -u UID    |    手工指定用户的 UID，注意 UID 的范围（不要小于 500）。     |
|  -d 主目录  | 手工指定用户的主目录。主目录必须写绝对路径，而且如果需要手工指定主目录，则一定要注意权限； |
| -c 用户说明 | 手工指定/etc/passwd文件中各用户信息中第 5 个字段的描述性内容，可随意配置； |
|   -g 组名   | 手工指定用户的初始组。一般以和用户名相同的组作为用户的初始组，在创建用户时会默认建立初始组。一旦手动指定，则系统将不会在创建此默认的初始组目录。 |
|   -G 组名   |  指定用户的附加组。我们把用户加入其他组，一般都使用附加组；  |
|  -s shell   |         手工指定用户的登录 Shell，默认是 /bin/bash；         |
|   -e 曰期   | 指定用户的失效曰期，格式为 "YYYY-MM-DD"。也就是 /etc/shadow 文件的第八个字段； |
|     -o      | 允许创建的用户的 UID 相同。例如，执行 "useradd -u 0 -o usertest" 命令建立用户 usertest，它的 UID 和 root 用户的 UID 相同，都是 0； |
|     -m      | 建立用户时强制建立用户的家目录。在建立系统用户时，该选项是默认的； |
|     -r      | 创建系统用户，也就是 UID 在 1~499 之间，供系统程序使用的用户。由于系统用户主要用于运行系统所需服务的权限配置，因此系统用户的创建默认不会创建主目录。 |

***

其实，系统已经帮我们规定了非常多的默认值，**在没有特殊要求下，无需使用任何选项**即可成功创建用户：

```
[root@localhost ~]# useradd lamp
此行命令就表示创建 lamp 普通用户。
```



### /etc/default/useradd文件

1、首先，使用 Vim 命令查看 /etc/default/useradd 文件中包含哪些内容：

```
[root@localhost ~]#vim /etc/default/useradd
# useradd defaults file
GR0UP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

***

也可以直接通过命令进行查看，结果是一样的：

```
[root@localhost ~]# useradd -D  ->>（-D 选项指的就是查看新建用户的默认值。）
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

***



​                                                                            **/etc/default/useradd 文件内容如表**

|         参数          |                             含义                             |
| :-------------------: | :----------------------------------------------------------: |
|       GR0UP=100       | 这个选项用于建立用户的默认组，也就是说，在添加每个用户时，用户的初始组就是 GID 为 100 的这个用户组。但 CentOS 并不是这样的，而是在添加用户时会自动建立和用户名相同的组作为此用户的初始组。也就是说这个选项并不会生效。  Linux 中默认用户组有两种机制：一种是私有用户组机制，系统会创建一个和用户名相同的用户组作为用户的初始组；另一种是公共用户组机制，系统用 GID 是 100 的用户组作为所有新建用户的初始组。目前我们采用的是私有用户组机制。 |
|      HOME=/home       | 指的是用户主目录的默认位置，所有新建用户的主目录默认都在 /home/下，刚刚新建的 lamp1 用户的主目录就为 /home/lamp1/。 |
|      INACTIVE=-1      | 指的是密码过期后的宽限天数，也就是 /etc/shadow 文件的第七个字段。这里默认值是 -1，代表所有新建立的用户密码永远不会失效。 |
|        EXPIRE=        | 表示密码失效时间，也就是 /etc/shadow 文件的第八个字段。默认值是空，代表所有新建用户没有失效时间，永久有效。 |
|    SHELL=/bin/bash    |       表示所有新建立的用户默认 Shell 都是 /bin/bash。        |
|    SKEL=/etc/skel     | 在创建一个新用户后，你会发现，该用户主目录并不是空目录，而是有 .bash_profile、.bashrc 等文件，这些文件都是从 /etc/skel 目录中自动复制过来的。因此，更改 /etc/skel 目录下的内容就可以改变新建用户默认主目录中的配置文件信息。 |
| CREATE_MAIL_SPOOL=yes | 指的是给新建用户建立邮箱，默认是创建。也就是说，对于所有的新建用户，系统都会新建一个邮箱，放在 /var/spool/mail/ 目录下，和用户名相同。例如，lamp1 的邮箱位于 /var/spool/mail/lamp1。 |

> 注意，此文件中各选项值的修改方式有 2 种，一种是通过 Vim 文本编辑器手动修改，另一种就是使用文章开头介绍的 useradd 命令，不过所用的命令格式发生了改变：
>
> ```
> useradd -D [选项] 参数
> ```

***



**用此命令修改 /etc/default/useradd 文件，可使用的选项如表所示。**

|  选项+参数  |                             含义                             |
| :---------: | :----------------------------------------------------------: |
|   -b HOME   | 设置所创建的主目录所在的默认目录，只需用目录名替换 HOME 即可，例如 useradd -D -b /gargae。 |
|  -e EXPIRE  | 设置密码失效时间，EXPIRE 参数应使用 YYYY-MM-DD 格式，例如 useradd -D -e 2019-10-17。 |
| -f INACTIVE |        设置密码过期的宽限天数，例如 useradd -D -f 7。        |
|  -g GROUP   |      设置新用户所在的初始组，例如 useradd -D -g bear。       |
|  -s SHELL   | 设置新用户的默认 shell，SHELL 必须是完整路径，例如 useradd -D -s /usr/bin/csh。 |



***

> 其实，useradd 命令创建用户的过程是这样的，系统**首先读取 /etc/login.defs 和 /etc/default/useradd**，根据这两个配置文件中定义的规则添加用户，也就是向 /etc/passwd、/etc/group、/etc/shadow、/etc/gshadow 文件中添加用户数据，**接着系统会自动在 /etc/default/useradd 文件设定的目录下建立用户主目录**，最后复制 /etc/skel 目录中的所有文件到此主目录中，由此，一个新的用户就创建完成了。

***



## passwd：修改用户密码

**passwd 命令的基本格式如下：**

```
[root@localhost ~]#passwd [选项] 用户名
```

选项：

- -S：查询用户密码的状态，也就是 /etc/shadow 文件中此用户密码的内容。仅 root 用户可用；
- -l：暂时锁定用户，该选项会在 /etc/shadow 文件中指定用户的加密密码串前添加 "!"，使密码失效。仅 root 用户可用；
- -u：解锁用户，和 -l 选项相对应，也是只能 root 用户使用；
- --stdin：可以将通过管道符输出的数据作为用户的密码。主要在批量添加用户时使用；
- -n 天数：设置该用户修改密码后，多长时间不能再次修改密码，也就是修改 /etc/shadow 文件中各行密码的第 4 个字段；
- -x 天数：设置该用户的密码有效期，对应 /etc/shadow 文件中各行密码的第 5 个字段；
- -w 天数：设置用户密码过期前的警告天数，对于 /etc/shadow 文件中各行密码的第 6 个字段；
- -i 日期：设置用户密码失效日期，对应 /etc/shadow 文件中各行密码的第 7 个字段。

***

==注意，普通用户只能使用 passwd 命令修改自己的密码，而不能修改其他用户的密码。==

***

【例 1】

```
#查看用户密码的状态
[root@localhost ~]# passwd -S lamp
lamp PS 2013-01-06 0 99999 7 -1 (Password set, SHA512 crypt.)
#上面这行代码的意思依次是：用户名 密码 设定时间(2013*01-06) 密码修改间隔时间(0) 密码有效期(99999) 警告时间(7) 密码不失效(-1)，密码已使用
```

> "-S"选项会显示出密码状态，这里的密码修改间隔时间、密码有效期、警告时间、密码宽限时间其实分别是 /etc/shadow 文件的第四、五、六、七个字段的内容。



***

为了方便系统管理，passw命令提供了  ——stdin选项，用于批量给用户设置初始密码。

> 这样设定的密码会把密码明文保存在历史命令中，如果系统被攻破，别人可以在`/root/.bash_history`中找到设置密码的这个命令，存在安全隐患。

***



## 创建新用户并赋予指定目录的相关权限



#### 1、创建用户且指定该用户的根路径和密码

```python
useradd -d /home/mydir -m username
```

==这种方式创建的用户可以使用ssh登录，但只有只读权限可以浏览下载部分文件无法写和修改。建议通过将用户加入一个组来获得指定路径的权限。==

***



#### 2、设置密码

```
passwd username
```



#### 3、用户权限

```
chown -R username: username /home/mydir
chmod 755 /home/mydir
```



***



#### 4、将用户加入到组

**将一个用户添加到用户组中，尽量不要直接用（除非确实时只属于一个组）**

```
usermod -G groupA username
```

==这样做会使你离开其他用户组，仅仅做为这个用户组 groupA 的成员。==
应该用 ==加上 -a 选项：==

```
usermod -a -G groupA username
```

***



#### 5、查看用户所属的组使用命令

```
groups username
```



## usermod命令：修改用户信息


如果不小心添错用户信息，后期如何修改？

- 使用vim文件编辑器手动修改涉及用户信息的相关文件（/etc/passwd、/etc/shadow、/etc/group、/etc/gshadow）
- 使用usermod命令，该命令专门用于修改用户信息

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
这里一定要分清 useradd 命令和 usermod 命令的区别，前者用于添加用户，当然，添加用户时可以对用户信息进行定制；后者针对与已存在的用户，使用该命令可以修改它们的信息。

**usermod 命令的基本格式如下：**

```
[root@localhost ~]#usermod [选项] 用户名
```

> 选项：
>
> - -c 用户说明：修改用户的说明信息，即修改 /etc/passwd 文件目标用户信息的第 5 个字段；
> - -d 主目录：修改用户的主目录，即修改 /etc/passwd 文件中目标用户信息的第 6 个字段，需要注意的是，主目录必须写绝对路径；
> - -e 日期：修改用户的失效曰期，格式为 "YYYY-MM-DD"，即修改 /etc/shadow 文件目标用户密码信息的第 8 个字段；
> - -g 组名：修改用户的初始组，即修改 /etc/passwd 文件目标用户信息的第 4 个字段（GID）；
> - -u UID：修改用户的UID，即修改 /etc/passwd 文件目标用户信息的第 3 个字段（UID）；
> - -G 组名：修改用户的附加组，其实就是把用户加入其他用户组，即修改 /etc/group 文件；
> - -l 用户名：修改用户名称；
> - -L：临时锁定用户（Lock）；
> - -U：解锁用户（Unlock），和 -L 对应；
> - -s shell：修改用户的登录 Shell，默认是 /bin/bash。

##### **`相对useradd命令，usermod命令还多出几个选项，即—L，—U，作用分别与passwd命令和—l和—u相同。需要注意的是，并不是所有的Linux发行版都包含这个命令。`**

==此命令对用户的临时锁定，同 passwd 命令一样，都是在 /etc/passwd 文件目标用户的加密密码字段前添加 "!"，使密码失效；反之，解锁用户就是将添加的 "!" 去掉。==

***



## change：修改用户密码状态

除了`passwd -S`命令可以查看用户的密码信息外，还可以利用chage命令，它可以显示更加详细的用户密码信息，并且和passwd命令一样，提供了修改用户密码信息的功能。

**chage命令的基本格式：**

```
[root@localhost ~]#chage [选项] 用户名
```

> 选项：
>
> - -l：列出用户的详细密码状态;
> - -d 日期：修改 /etc/shadow 文件中指定用户密码信息的第 3 个字段，也就是**最后一次修改密码的日期**，格式为 YYYY-MM-DD；
> - -m 天数：修改密码最短保留的天数，也就是 /etc/shadow 文件中的第 4 个字段；
> - -M 天数：修改密码的有效期，也就是 /etc/shadow 文件中的第 5 个字段；
> - -W 天数：修改密码到期前的警告天数，也就是 /etc/shadow 文件中的第 6 个字段；
> - -i 天数：修改密码过期后的宽限天数，也就是 /etc/shadow 文件中的第 7 个字段；
> - -E 日期：修改账号失效日期，格式为 YYYY-MM-DD，也就是 /etc/shadow 文件中的第 8 个字段。

------

==chage命令除了修改密码信息的功能外，还可以强制用户在第一次登录后，必须修改密码，并利用新密码重新登陆系统，此用户才能正常使用。==

***



## userdel命令：删除用户

userdel 命令功能很简单，就是==删除用户的相关数据==。**此命令只有 root 用户才能使用。**


***

用户的相关数据包含如下几项：

- **用户基本信息：存储在 /etc/passwd 文件中；**
- **用户密码信息：存储在 /etc/shadow 文件中；**
- **用户群组基本信息：存储在 /etc/group 文件中；**
- **用户群组信息信息：存储在 /etc/gshadow 文件中；**
- **用户个人文件：主目录默认位于 /home/用户名，邮箱位于 /var/spool/mail/用户名。**

***



基本命令格式：

```
[root@localhost ~]# userdel -r 用户名
-r 选项表示在删除用户的同时删除用户的家目录。
```

> 在删除用户的同时如果不能删除用户的家目录，那么家目录就会编程没有属主和属组的目录，（垃圾文件）



如果要删除的用户已经使用过系统一段时间，那么此用户可能在系统中留有其他文件，因此，如果我们想要从系统中彻底的删除某个用户，最好在使用 userdel 命令之前，先通过 `find -user 用户名` 命令查出系统中属于该用户的文件，然后在加以删除。

***



## id命令：查看用户的UID和GID


**id 命令可以查询用户的UID、GID 和附加组的信息。**

命令比较简单，格式如下：

```
[root@localhost ~]# id 用户名
```



【1】如果不加任何参数，id默认显示当前登录用户的id信息，假设当前登录账号为root,用id查看root的ID：

```
[root@study ~]# id

uid=0(root) gid=0(root) groups=0(root)
```

> ==可以看到超级用户root的uid和gid都是0，root也只有一个群组，那就是root组。==



***





## su命令：用户间切换



su是最简单的用户切换命令，通过该命令可以实现任何身份的切换，包含从普通用户切换为root用户，从root用户切换为普通用户以及普通用户之间的切换。

> 普通用户之间切换以及普通用户切换至 root 用户，都需要知晓对方的密码，只有正确输入密码，才能实现切换；从 root 用户切换至其他用户，无需知晓对方密码，直接可切换成功。



**命令基本格式：**

```
[root@localhost ~]# su [选项] 用户名
```



选项：

- -：当前用户不仅切换为指定用户的身份，同时所用的工作环境也切换为此用户的环境（包括 PATH 变量、MAIL 变量等），使用 - 选项可省略用户名，默认会切换为 root 用户。
- -l：同 - 的使用类似，也就是在切换用户身份的同时，完整切换工作环境，但后面需要添加欲切换的使用者账号。
- -p：表示切换为指定用户的身份，但不改变当前的工作环境（不使用切换用户的配置文件）。
- -m：和 -p 一样；
- -c 命令：仅切换用户执行一次命令，执行后自动切换回来，该选项后通常会带有要执行的命令。

***



### su和 su - 的区别


使用 su 命令时，有 - 和没有 - 是完全不同的，==- 选项表示在切换用户身份的同时，连当前使用的环境变量也切换成指定用户的==。我们知道，环境变量是用来定义操作系统环境的，因此如果系统环境没有随用户身份切换，很多命令无法正确执行。



> 举个例子，普通用户 lamp 通过 su 命令切换成 root 用户，但没有使用 - 选项，这样情况下，虽然看似是 root 用户，但系统中的PATH 环境变量依然是 lamp 的（而不是 root 的），因此当前工作环境中，并不包含 /sbin、/usr/sbin等超级用户命令的保存路径，这就导致很多管理员命令根本无法使用。不仅如此，当 root 用户接受邮件时，会发现收到的是 lamp 用户的邮件，因为环境变量 $MAIL 也没有切换。


***



## whoami和who am i 命令的用法和区别



**whoami 命令和 who am i 命令是不同的 2 个命令，前者用来打印当前==执行操作的用户名==，后者则用来打印==登陆当前 Linux 系统的用户名==。**



【1】为了能够更好地区分这 2 个命令的功能，举个例子，我们首先使用用户名为“Cyuyan”登陆 Linux 系统，然后执行如下命令：

```
[Cyuyan@localhost ~]$ whoami
Cyuyan
[Cyuyan@localhost ~]$ who am i
Cyuyan    pts/0    2017-10-09 15:30 (:0.0)
```

在此基础上，使用 su 命令切换到 root 用户下，再执行一遍上面的命令：

```
[Cyuyan@localhost ~] su - root
[root@localhost ~]$ whoami
root
[root@localhost ~]$ who am i
Cyuyan    pts/0    2017-10-09 15:30 (:0.0)
```



> 执行 whoami 命令，等同于执行 id -un 命令；执行 who am i 命令，等同于执行 who -m 命令。



从上面两个例子可以看出来，使用 su 或者 sudo 命令切换用户身份，骗得过 whoami，但骗不过 who am i。**要解释这背后的运行机制，需要搞清楚什么是实际用户（UID）和有效用户（EUID，即 Effective UID）。**



> 所谓实际用户，指的是登陆 Linux 系统时所使用的用户，因此在整个登陆会话过程中，实际用户是不会发生变化的；而有效用户，指的是当前执行操作的用户，也就是说真正决定权限高低的用户，这个是能够利用 su 或者 sudo 命令进行任意切换的。

> 一般情况下，实际用户和有效用户是相同的，如果出现用户身份切换的情况，它们会出现差异。需要注意的是，实际用户和有效用户出现差异，切换用户并不是唯一的触发机制

***



## group用户组命令



### groupadd命令：添加用户组

**添加用户组的命令是 groupadd**

命令格式如下:

```
[root@localhost ~]# groupadd [选项] 组名
```


选项：

- -g GID：指定组 ID；
- -r：创建系统群组。

***



### groupmod命令：修改用户组

**groupmod 命令用于修改用户组的相关信息**

基本格式：

```
[root@localhost ~]# groupmod [选现] 组名
```


选项：

- -g GID：修改组 ID；
- -n 新组名：修改组名；

***



==用户名不要随意修改，组名和 GID 也不要随意修改，因为非常容易导致管理员逻辑混乱。如果非要修改用户名或组名，则建议先删除旧的，再建立新的。==

***



### groupdel命令：删除用户组

**groupdel 命令用于删除用户组（群组）**


基本格式：

```
[root@localhost ~]#groupdel 组名
```



> 注意，**不能使用 groupdel 命令随意删除群组**。此命令仅适用于删除那些 "不是任何用户初始组" 的群组，换句话说，如果有群组还是某用户的初始群组，则无法使用 groupdel 命令成功删除



### gpasswdml：把用户添加进组或从组删除


为了避免系统管理员（root）太忙碌，无法及时管理群组，**==使用 gpasswd 命令给群组设置一个群组管理员，代替 root 完成将用户加入或移出群组的操作==**


基本格式：

```
[root@localhost ~]# gpasswd 选项 组名
```

***


​                                                                                     **gpasswd命令各选项及其功能**

|     选项     |                             功能                             |
| :----------: | :----------------------------------------------------------: |
|              |      选项为空时，表示给群组设置密码，仅 root 用户可用。      |
| -A user1,... | 将群组的控制权交给 user1,... 等用户管理，也就是说，设置 user1,... 等用户为群组的管理员，仅 root 用户可用。 |
| -M user1,... |       将 user1,... 加入到此群组中，仅 root 用户可用。        |
|      -r      |              移除群组的密码，仅 root 用户可用。              |
|      -R      |             让群组的密码失效，仅 root 用户可用。             |
|   -a user    |                  将 user 用户加入到群组中。                  |
|   -d user    |                  将 user 用户从群组中移除。                  |

> 从表可以看到，除 root 可以管理群组外，可设置多个普通用户作为群组的管理员，但也只能做“将用户加入群组”和“将用户移出群组”的操作。

***



### newgrp命令：切换用户的有效组

每个用户可以属于一个初始组（用户是这个组的初始用户），也可以属于多个附加组（用户是这个组的附加用户）。既然用户可以属于这么多用户组，**那么用户在创建文件后，默认生效的组身份是哪个呢？**



==newgrp 命令可以从用户的附加组中选择一个群组，作为用户新的初始组。==

基本格式：

```
[root@localhost ~]# newgrp 组名
```



#### newgrp命令的底层实现

newgrp 命令每一次切换用户的初始组，该用户都会以另外一个 shell（新进程，也可以说是子进程）登陆，只不过在新 shell 上登陆的该用户，其初始组改变了而已。

> 以上实例中，通过添加 shell 内置命令 "echo $$" 就可以发现，每次使用 newgrp 命令，都会切换到一个新的进程。


**使用 newgrp 命令切换用户初始组的整个过程**

![img](https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303241014183.png)

可以看到，每一次使用 newgrp 切换用户的初始组，用户都会切换到一个新的子 shell 中，如图中，user1 用户的初始组从最初的 group1，切换成了 group2，再切换成 group3。

***

newgrp 命令可以从用户的附加组中选择一个群组，作为用户新的初始组。此命令的基本格式如下：

```
[root@localhost ~]# newgrp 组名
```

