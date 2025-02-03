# 从零开始在 `StarryOS` 中运行 `Dora`
# 第三节：调试系统调用

上一节我们已经成功的在 `StarryOS` 中运行了 `dora -h`，本节我们将从内核的角度来调试运行 `dora up`。

我们输入 `/dora up`，可以看到如下输出，输出内容做了一些精简，去除了无关的信息。

```shell
       d8888                            .d88888b.   .d8888b.
      d88888                           d88P" "Y88b d88P  Y88b
     d88P888                           888     888 Y88b.
    d88P 888 888d888  .d8888b  .d88b.  888     888  "Y888b.
   d88P  888 888P"   d88P"    d8P  Y8b 888     888     "Y88b.
  d88P   888 888     888      88888888 888     888       "888
 d8888888888 888     Y88b.    Y8b.     Y88b. .d88P Y88b  d88P
d88P     888 888      "Y8888P  "Y8888   "Y88888P"   "Y8888P"

arch = x86_64
platform = x86_64-qemu-q35
target = x86_64-unknown-none
smp = 1
build_mode = release
log_level = error

/ # /dora up
         7:	file=/dora [0];  generating link map
         7:	  dynamic: 0x00000000014e5600  base: 0x0000000000001000   size: 0x00000000014f9d18
         7:	    entry: 0x00000000001b2570  phdr: 0x0000000000001040  phnum:                 14
         7:	file=libgcc_s.so.1 [0];  needed by /dora [0]
         7:	file=libgcc_s.so.1 [0];  generating link map
         7:	  dynamic: 0x0000000001519de0  base: 0x00000000014fb000   size: 0x000000000001f2e8
         7:	    entry: 0x00000000014fb000  phdr: 0x00000000014fb040  phnum:                 11
         7:	file=libm.so.6 [0];  needed by /dora [0]
         7:	file=libm.so.6 [0];  generating link map
         7:	  dynamic: 0x0000000001602d90  base: 0x000000000151d000   size: 0x00000000000e6108
         7:	    entry: 0x000000000151d000  phdr: 0x000000000151d040  phnum:                 11
         7:	file=libc.so.6 [0];  needed by /dora [0]
         7:	file=libc.so.6 [0];  generating link map
         7:	  dynamic: 0x000000000181cbc0  base: 0x0000000001604000   size: 0x0000000000227e50
         7:	    entry: 0x000000000162df50  phdr: 0x0000000001604040  phnum:                 14
         7:	calling init: /lib64/ld-linux-x86-64.so.2
         7:	calling init: /lib/libc.so.6
         7:	calling init: /lib/libm.so.6
         7:	calling init: /lib/libgcc_s.so.1
         7:	initialize program: /dora
         7:	transferring control: /dora
[  5.210872 0:8 axruntime::lang_items:5] panicked at /Users/echo/.cargo/git/checkouts/linux_syscall_api-6fd063eab50f7cab/811be4c/src/syscall.rs:47:9:
unknown syscall id: 332
```

在输出的结果中我们可以看到上一节我们补全的动态库的加载过程，在加载完所有以来的动态库之后，系统 `panic` 了，并且输出了错误信息。

通过错误信息可以看到程序调用了一个未知的系统调用，这个系统调用号为 `332`。同时根据输出的内容还可以知道，这个错误是在 `linux_syscall_api` 这个 `crate` 中的 `/src/syscall.rs` 文件中的第 `47` 行出现的。

由此我们得到了如下的信息：

1. 错误发生在 `linux_syscall_api` 这个 `crate` 中。
2. 错误发生在 `/src/syscall.rs` 文件中的第 `47` 行。
3. 内核缺少一个系统调用号为 `332` 的系统调用。

上面的三个问题我们一一的解决。

### 组件化 `StarryOS` 如何添加组件包

#### `kbuild`

`kbuild` 是一个构建 `RustOS` 的 `cargo` 工具。仓库地址：[https://github.com/Byte-OS/kbuild](https://github.com/Byte-OS/kbuild)。详细使用查看[`kbuild`工具]



### 如何本地调试 `StarryOS` 内核

### 组件化 `StarryOS` 系统如何添加系统调用