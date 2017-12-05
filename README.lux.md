# Learning Linux 0.11

## Mac setup
### 运行
- 按照README可以直接编译成功

### 调试
- 按照README安装gdb
- Makefile中的`qemu-system-x86_64`替换为`qemu-system-i386`，不然调试的时候报错`warning: Selected architecture i386 is not compatible with reported target architecture i386:x86-64`
	- 因为内核是i386的，gdb是i386的，qemu也需要使用i386的
	- `qemu-systerm-*`后面代表的是要模拟的架构
