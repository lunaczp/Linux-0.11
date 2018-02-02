# First Run

使用bochs启动Linux 0.11，要准备
- kernel镜像，即编译生成的Image文件
- 根文件系统磁盘镜像。Linux0.11发布的时候搭载了两个磁盘镜像，一个bootimg，即Image内核镜像；一个rootimg，即文件系统镜像。这个镜像是格式化好的minix文件系统，内核可以直接识别并装载，同时文件系统内包含了基本的可执行文件如`mkfs`,`ls`,`cd`等。 没有文件系统，Linux没办法工作（没有文件系统，怎么算是操作系统...) 。

当然，如今的操作系统，都提供安装程序，安装程序负责
- (可选）格式化磁盘
- 选择至少一个分区，格式化为支持的文件系统
- 拷贝系统镜像到该分区，分区标记为可启动
- 修改主分区表，把该分区加入启动列表

而Linux0.11的时候，直接提供了两个磁盘镜像，用户可以在DOS或者Minix下，把磁盘镜像写入floppy。
- 插入boot floopy，启动系统
- 更换root floppy，加载文件系统
- 进入系统。
当然，用户可以选择在自己的硬盘上，划分出一个分区，格式化为minix文件系统，并把rootimg内的可执行文件拷贝过去，这样用户就可以用硬盘来使用Linux了。

### Ref
[linux-0.11 release note and user guild by Linus](https://github.com/lunaczp/archive_linux/commit/7aadf253807264ad5561479e2a8b9de9602f966d)
[user experience install linux-0.11](http://www.hpcf.upr.edu/~humberto/documents/install-log.html)


## 言归正传
回到主题，如何建立一个自己的根文件系统，并在bochs下跑linux呢。

需要：
- Image文件
- 硬盘镜像，分好区，格式化成minix，并拷贝了基本的二进制文件

流程
- 编译，生成Image
- bximage，生成disk镜像c.img
- Ubuntu下fdisk，生成一个分区，并标记为minix系统
- Ubuntu下mkfs.minix，初始化minix文件系统(Centos下没有mkfs.minix)
	- 需要先挂载指定分区，再格式化，[参考](https://unix.stackexchange.com/questions/209566/how-to-format-a-partition-inside-of-an-img-file)
- bochs利用bootimg，rootimg启动linux0.11，并挂载c.img
- 将rootimg内的文件拷贝到c.img内的格式化好的第一个分区
- done
- 重新启动bochs，用bootimg启动，c.img作为跟文件系统。成功

### 说明
linux0.11识别跟文件系统的方法是写死在bootimg的。一般地是floppy，或者是硬盘的第一个分区，当然也可以是其他，规则在[linux-0.11 release note and user guild by Linus](https://github.com/lunaczp/archive_linux/commit/7aadf253807264ad5561479e2a8b9de9602f966d)里有说明。
注意，我们使用的是硬盘的第一个分区，所以代码里写好了是0x301

### 参考：
- Linux 内核完全注释 17.7 制作根文件系统
- test/firstRun
