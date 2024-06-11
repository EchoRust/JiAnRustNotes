# 从零开始在 `StarryOS` 中运行 `Dora`
# 环境搭建

**重要提醒**

> 最新组件化拆分后的 `StarryOS` 具体的命令参数和详解参考[最新版本`StarryOS`环境搭建说明](../../os/new-starry-os.md)
> 
> 推荐使用 `Ubuntu 20.04 LTS` 操作系统进行开发。`Windows` 系统可以使用虚拟机操作。

在 `Ubuntu` 中构建磁盘文件比较方便，只需要一条命令即可

```shell
$ ./build_img.sh -a x86_64                  # 过程中可能需要输入当前登录账号的密码
```

由于 `技安` 使用的是 `macOS` 系统，具体的操作方式可参考[`macOS`开发`StarryOS`环境部署说明](../../os/starryos-macos.md)

开发环境搭建完成后，我们需要做的就是把编译好的 `dora` 可执行程序放在磁盘的根目录下面。    
假设您已经编译完成 `dora`，并且把 `dora` 存放在当前工作目录下。

## 把 `dora` 二进制文件放到磁盘中

### `ubuntu` 系统操作步骤

```shell
$ mkdir rootfs                              # 创建 rootfs 文件夹，用以挂载磁盘文件
$ sudo losetup /dev/loop50 disk.img         # 把磁盘文件设置成块设备
$ sudo mount /dev/loop50 rootfs             # 挂载磁盘文件到 rootfs 文件夹下
$ sudo cp dora rootfs                       # 把 dora 拷贝到磁盘的根目录下
$ sudo umount rootfs                        # 解除挂载
$ sudo losetup -d /dev/loop50               # 卸载块设备
```

知识点说明：

* `losetup` 命令用来设置循环设备。循环设备可把文件虚拟成块设备，籍此来模拟整个文件系统，让用户得以将其视为硬盘驱动器，光驱或软驱等设备，并挂入当作目录来使用。
* `loop` 设备介绍: 在类 `UNIX` 系统里，`loop` 设备是一种伪设备( `pseudo-device` )，或者也可以说是仿真设备，它将一个文件映射成块设备，从而实现对文件的读写操作。 `loop` 设备一般以 `/dev/loop` 开头，如 `/dev/loop0` 、`/dev/loop1` 等。它能使我们像块设备一样访问一个文件。在使用之前，一个 `loop` 设备必须要和一个文件进行连接。这种结合方式给用户提供了一个替代块特殊文件的接口。因此，如果这个文件包含有一个完整的文件系统，那么这个文件就可以像一个磁盘设备一样被 `mount` 起来。
* `loop50` 是我选的在系统中是一个不存在的设备，如果你的系统中存在这个设备，需要更换一下后面的编号，保证唯一性。

### `macOS` 系统下操作步骤

**可视化操作**

* 双击打开 `disk.img` 文件。
* 系统会自动挂在到新的磁盘中，在 `Finder` 中选中磁盘或者桌面双击挂在的磁盘，把 `dora` 复制到磁盘根目录中。
* 弹出磁盘完成文件的复制。

**命令操作**

```shell
$ mkdir mnt                                 # 创建 mnt 文件夹，用以挂载磁盘文件
$ hdiutil attach disk.img -mountpoint mnt   # 附加磁盘镜像挂在在 mnt 文件夹中
$ cp dora mnt                               # 把 dora 复制到磁盘根目录中
$ hdiutil detach mnt                        # 从系统中分离分盘镜像
```

知识点说明:

* `hdiutil`: 磁盘映像工具，用于管理磁盘映像文件。详细说明可查看[`hdiutil`工具](../../other/hdiutil.md)

## 启动 `StarryOS` 系统

执行如下命令
```shell
$ make clean && make A=apps/monolithic_userboot LOG=error ARCH=x86_64 FEATURES=img run
```

先清空`make`缓存，再启动`StarryOS`

启动后将出现如下图所示

![starryos01.png](../../img/starryos01.png)

我们现在已经成功的启动了 `StarryoS` 系统了 下一节。 我们将继续在 `StarryOS` 上成功的运行 `dora -h` 命令。