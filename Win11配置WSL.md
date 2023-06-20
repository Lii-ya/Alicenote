# Win11配置WSL

## 电脑内系统配置

1、控制面板—>>程序—>>启用或关闭window功能   或者直接搜索启用或关闭Windows功能

2、出现弹窗后选择：开启 Windows 虚拟化、 Linux 子系统（WSL2)、Hyper-V

​                                      <img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303101021102.png" alt="img" style="zoom:40%;" />

3、用管理员身份打开powershell，分别输入以下指令：

```
bcdedit /set hypervisorlaunchtype Auto

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform #需要重启系统，请注意按y让他重启

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

如果以上内容不能顺利进行，可以执行以下指令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

#### 下载Linux子操作系统

打开Microsoft Store，搜索Ubuntu，里面有很多不同的操作系统，可以更具自己需要的选择下载。下载完成后，在开始菜单中打开Ubuntu，初始化会让你注册用户名和创建密码。到此安装结束。

## 常见问题

#### 解决Hyper-V没有的问题

如果在启用或关闭Windows功能的弹窗中没有Hyper-V，可以通过如下解决：

1. 新建一个txt文件，将以下内容复制粘贴进去。

   ```
   pushd "%~dp0"
   
   dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
   
   for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
   
   del hyper-v.txt
   
   Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
   ```

   > 保存后，文件的后缀名改成.cmd格式，然后用管理员省份运行。等待文件中的各项都装载完成，重启电脑即可。可以在命令窗口输入Y重启。重启之后，Hyper-v就已经添加到“启用或关闭windows功能”中，并且默认开启。

<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303101032423.png" alt="img" style="zoom: 67%;" />

重启之后，重新在管理员权限下的powershell中运行刚刚的Microsoft-Hyper-V即可

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

#### 解决wsl启动时出现一堆？？？的问题

#### <img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303101034135.png" alt="img" style="zoom:67%;" />

1. 首先下载Windows Subsystem for Linux Update setup 官方版将WSL1升级到WSL2。

​       下载地址1：https://www.xitongzhijia.net/soft/244754.html

​       下载地址2：https://www.aliyundrive.com/s/F297Y6SNpSf

2. 下载完毕执行一下该msi程序。

​        然重新跑一下以下代码：

```
bcdedit /set hypervisorlaunchtype Auto

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform #需要重启系统，请注意按y让他重启

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linu
```

#### wsl子系统初始化报错

启动wsl系统时报错：参考的对象类型不支持尝试的操作，效果如下：<img src="https://raw.githubusercontent.com/Lii-ya/PicGO1/main/202303101037226.png" alt="img" style="zoom:67%;" />

解决方案是使用注册表方式。复制如下代码，新建文件test.reg（文件名可任意取，需以.reg结尾），以管理员身份运行。

```
Windows Registry Editor Version 5.00
 
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinSock2\Parameters\AppId_Catalog\0408F7A3]
"AppFullPath"="C:\\Windows\\System32\\wsl.exe"
"PermittedLspCategories"=dword:80000000
```

