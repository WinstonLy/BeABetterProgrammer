### Linux分区设置
分区类型            分区大小            挂载点        新分区位置             新分区用途    
  主分区              10G～20G            /         空间起始位置          Ext4日志文件系统
  逻辑分区        与电脑内存一致（如8G）               空间起始位置             交换空间
  逻辑分区        500M左右，根据情况而定     /boot     空间起始位置          Ext4日志文件系统
  逻辑分区             剩余所有空间         /boot     空间起始位置          Ext4日志文件系统
  注意： 设置完成后要选择挂载到/boot的分区作为启动项。

### Linux切换到root用户
---首先进入到终端界面，`sudo passwd root`，根据提示设置密码
  ---然后输入`su roo`t进入root用户
  ---输入`exit`退出root账户，切换到普通用户；或者直接用Ctrl+D退出。

### Linux下解压命令总结
.tar 
  解包：`tar xvf FileName.tar`
  打包：`tar cvf FileName.tar DirName`

  .gz
  解压1：`gunzip FileName.gz`
  解压2：`gzip -d FileName.gz`
  压缩：`gzip FileName`

  .tar.gz 和 .tgz
  解压：tar `-zxvf FileName.tar.gz`
  压缩：`tar zcvf FileName.tar.gz DirName`

  .zip
  解压：`unzip FileName.zip`
  压缩：`zip FileName.zip DirName`

  .rar
  解压：`rar x FileName.rar`
  压缩：`rar a FileName.rar DirName`

  .rpm
  解包：`rpm2cpio FileName.rpm | cpio -div`

  .deb
  解包：`dpkg -i FileName.deb`

### Linux下删除文件
"rm -r" 将参数中列出的全部目录和子目录进行递归删除。(r为recursive的意思)
 ` rm 文件名   删除一个文件`
 ` rm -r 文件目录  删除一个目

### Linux取出文件或者文件夹下的锁标志
`sudo chown 用户名 文件名/ -R`

### Linux和Windows双系统时间不一致
先输入以下命令更新Linux系统时间
  `sudo apt-get install ntpdate`
  再将时间更新到硬件上
 ` sudo ntpdate time.windows.com`
 ` sudo hwclock --localtime --systohc`
  实现时间同步
  `sudo timedatectl set-local-rtc 1`
  重启之后修改生效

### Linux下解决同时使用有线和无线网络时，网络的优先连接问题（一插上网线，就只能通过有线联网了，也就上不了网)

- 查看当前网关信息`ip route show`

```
default via 192.168.0.1 dev eno1  proto static  metric 100 
default via 192.168.2.1 dev wlxe84e06600d75  proto static  metric 600 
169.254.0.0/16 dev wlxe84e06600d75  scope link  metric 1000 
192.168.0.0/24 dev eno1  proto kernel  scope link  src 192.168.0.100  metric 100 
192.168.2.0/24 dev wlxe84e06600d75  proto kernel  scope link  src 192.168.2.145  metric 600 
```

- 查看ip信息`ifconfig`

```
eno1      Link encap:Ethernet  HWaddr e4:54:e8:9e:50:1d  
          inet addr:192.168.0.100  Bcast:192.168.0.255  Mask:255.255.255.0
          inet6 addr: fe80::fbde:4207:b836:7137/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:44974 errors:0 dropped:0 overruns:0 frame:0
          TX packets:146324 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:4005175 (4.0 MB)  TX bytes:218046134 (218.0 MB)
          Interrupt:16 Memory:9dc80000-9dca0000 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:2022 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2022 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:164064 (164.0 KB)  TX bytes:164064 (164.0 KB)

wlxe84e06600d75 Link encap:Ethernet  HWaddr e8:4e:06:60:0d:75  
          inet addr:192.168.2.145  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::abc4:714c:92aa:8964/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:45072 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28669 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:54415913 (54.4 MB)  TX bytes:4394566 (4.3 MB)

```

- 通过上面的输出可以看出有线网eno1的默认网关优先于无线网，因此需要修改默认网关的优先级。

  `sudo route del default gw 192.168.0.1` 这里的192.168.0.1时有线网的网关地址

  `sudo route add default gw 192.168.2.145` 这里的192.168.2.145是无线网的ip地址

### win10+Ubuntu双系统下，Ubuntu不能访问windows的磁盘分区

```shell
Unable to access "107GB Volume"

Error mounting /dev/sda3 at /media/winston/229AC3E99AC3B795: Command-line `mount -t "ntfs" -o "uhelper=udisks2,nodev,nosuid,uid=1000,gid=1000" "/dev/sda3" "/media/winston/229AC3E99AC3B795"' exited with non-zero exit status 14: Windows is hibernated, refused to mount.
Failed to mount '/dev/sda3': Operation not permitted
The NTFS partition is in an unsafe state. Please resume and shutdown
Windows fully (no hibernation or fast restarting), or mount the volume
read-only with the 'ro' mount option.

```

解决方案如下：

- 安装　ntfs   `sudo apt-get install ntfs-3g`
- 修复挂载，通过 `sudofdisk -l`来查询磁盘号，然后利用`sudo ntfsfix /dev/sda`挂载上相应的磁盘

### Ubuntu 下如何删除软件

- 打开终端，输入`dpkg --list`，查看已安装软件名称
- 输入`sudo apt-get --purge remove softname`,`--purge`是可选项，若将软件及其配置文件一起删除则写上这个属性，若不需要删除配置文件可执行`sudo apt-get remove softname`

- `apt-get purge / apt-get --purge remove`删除已安装包（不保留配置文件)。如软件包a，依赖软件包b，则执行该命令会删除a，而且不保留配置文件

- `apt-get autoremove`删除为了满足依赖而安装的，但现在不再需要的软件包（包括已安装包），保留配置文件。

### 搜狗输入法出现英文输入格式有问题

具体情况如下所示：

ｆｄａｓｆｓａｄｇａｓ

将搜狗输入法的全角改为半角，出现的搜狗输入法的标志有个月牙形的东西，改为月牙即为半角，改为满月则为全角。

### 主机能ping通虚拟机，虚拟机ping不通主机

[解决办法](https://blog.csdn.net/hskw444273663/article/details/81301470?utm_medium=distribute.pc_relevant.none-task-blog-title-4&spm=1001.2101.3001.4242)