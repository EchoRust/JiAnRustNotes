# Ubuntu源码编译QEMU

```shell
$ sudo apt update -y

$ sudo apt install -y ninja-build pkg-config libglib2.0-dev libpixman-1-dev flex bison
```

> 报错:ERROR: glib-2.56 gthread-2.0 is required to compile QEMU  
> 需要安装 libglib2.0-dev

> 报错:ERROR: Dependency "pixman-1" not found, tried pkgconfig  
> 需要安装 libpixman-1-dev

> 报错:ERROR: Program 'flex' not found or not executable  
> 需要安装 flex

> 报错:ERROR: Program 'bison' not found or not executable  
> 需要安装 bison

```shell
$ ./configure
$ make -j$(nproc)
$ sudo make install
```

`configure`过程

```shell
Using './build' as the directory for build output
The Meson build system
Version: 0.61.5
Source dir: /home/echo/Software/qemu-8.0.5
Build dir: /home/echo/Software/qemu-8.0.5/build
Build type: native build
Project name: qemu
Project version: 8.0.5
C compiler for the host machine: cc -m64 -mcx16 (gcc 11.4.0 "cc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0")
C linker for the host machine: cc -m64 -mcx16 ld.bfd 2.38
Host machine cpu family: x86_64
Host machine cpu: x86_64
Program scripts/symlink-install-tree.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/symlink-install-tree.py)
Program sh found: YES (/usr/bin/sh)
C++ compiler for the host machine: c++ -m64 -mcx16 (gcc 11.4.0 "c++ (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0")
C++ linker for the host machine: c++ -m64 -mcx16 ld.bfd 2.38
Program python3 found: YES (/usr/bin/python3)
Program bzip2 found: YES (/usr/bin/bzip2)
Program iasl found: NO
Compiler for C supports link arguments -Wl,-z,relro: YES
Compiler for C supports link arguments -Wl,-z,now: YES
Compiler for C++ supports link arguments -Wl,--warn-common: YES
Program cgcc found: NO
Library m found: YES
Run-time dependency threads found: YES
Library util found: YES
Run-time dependency appleframeworks found: NO (tried framework)
Found pkg-config: /usr/bin/pkg-config (0.29.2)
Run-time dependency gio-2.0 found: YES 2.72.4
Program /usr/bin/gdbus-codegen found: YES (/usr/bin/gdbus-codegen)
Run-time dependency gio-unix-2.0 found: YES 2.72.4
Run-time dependency pixman-1 found: YES 0.40.0
Run-time dependency zlib found: YES 1.2.11
Has header "libaio.h" : NO
Run-time dependency liburing found: NO (tried pkgconfig)
Run-time dependency libnfs found: NO (tried pkgconfig)
Run-time dependency appleframeworks found: NO (tried framework)
Run-time dependency appleframeworks found: NO (tried framework)
Run-time dependency libseccomp found: NO (tried pkgconfig)
Has header "cap-ng.h" : NO
Run-time dependency xkbcommon found: NO (tried pkgconfig)
Run-time dependency slirp found: NO (tried pkgconfig)
Has header "libvdeplug.h" : NO
Run-time dependency libpulse found: NO (tried pkgconfig)
Run-time dependency alsa found: NO (tried pkgconfig)
Run-time dependency jack found: NO (tried pkgconfig)
Run-time dependency sndio found: NO (tried pkgconfig)
Run-time dependency spice-protocol found: NO (tried pkgconfig)
Run-time dependency spice-server found: NO (tried pkgconfig)
Library rt found: YES
Run-time dependency libiscsi found: NO (tried pkgconfig)
Run-time dependency libzstd found: NO (tried pkgconfig)
Run-time dependency virglrenderer found: NO (tried pkgconfig)
Run-time dependency blkio found: NO (tried pkgconfig)
Run-time dependency libcurl found: NO (tried pkgconfig)
Run-time dependency libudev found: NO (tried pkgconfig)
Library mpathpersist found: NO
Run-time dependency ncursesw found: NO (tried pkgconfig)
Has header "curses.h" : NO
Message: Trying with /usr/include/ncursesw
Has header "curses.h" : NO
Has header "brlapi.h" : NO
sdl2-config found: NO
Run-time dependency sdl2 found: NO (tried pkgconfig and config-tool)
Library rados found: NO
Has header "rbd/librbd.h" : NO
Run-time dependency glusterfs-api found: NO (tried pkgconfig)
Run-time dependency libssh found: NO (tried pkgconfig)
Has header "bzlib.h" : NO
Has header "lzfse.h" : NO
Has header "sys/soundcard.h" : YES
Run-time dependency epoxy found: NO (tried pkgconfig)
Has header "epoxy/egl.h" with dependency epoxy: NO
Run-time dependency gnutls found: NO (tried pkgconfig)
Run-time dependency gnutls found: NO (tried pkgconfig)
libgcrypt-config found: NO need ['>=1.8']
Run-time dependency libgcrypt found: NO (tried config-tool)
Run-time dependency nettle found: NO (tried pkgconfig)
Run-time dependency gmp found: NO (tried pkgconfig)
Run-time dependency gtk+-3.0 found: NO (tried pkgconfig)
Run-time dependency libpng found: NO (tried pkgconfig)
Run-time dependency libjpeg found: NO (tried pkgconfig)
Has header "sasl/sasl.h" : NO
Has header "security/pam_appl.h" : NO
Has header "snappy-c.h" : NO
Has header "lzo/lzo1x.h" : NO
Has header "numa.h" : NO
Library ibumad found: NO
Has header "rdma/rdma_cma.h" : NO
Library ibverbs found: NO
Run-time dependency xencontrol found: NO (tried pkgconfig)
Library xenstore found: NO
Library xenctrl found: NO
Library xendevicemodel found: NO
Library xenforeignmemory found: NO
Library xengnttab found: NO
Library xenevtchn found: NO
Library xentoolcore found: NO
Run-time dependency libcacard found: NO (tried pkgconfig)
Run-time dependency u2f-emu found: NO (tried pkgconfig)
Run-time dependency canokey-qemu found: NO (tried pkgconfig)
Run-time dependency libusbredirparser-0.5 found: NO (tried pkgconfig)
Run-time dependency libusb-1.0 found: NO (tried pkgconfig)
Run-time dependency libpmem found: NO (tried pkgconfig)
Run-time dependency libdaxctl found: NO (tried pkgconfig)
Run-time dependency libkeyutils found: NO (tried pkgconfig)
Checking for function "gettid" : YES
Run-time dependency libselinux found: YES 3.3
Run-time dependency fuse3 found: NO (tried pkgconfig)
Run-time dependency libbpf found: NO (tried pkgconfig)
Run-time dependency libdw found: NO (tried pkgconfig)
Has header "sys/epoll.h" : YES
Has header "linux/magic.h" : YES
Has header "valgrind/valgrind.h" : NO
Has header "linux/btrfs.h" : YES
Has header "libdrm/drm.h" : NO
Has header "pty.h" : YES
Has header "sys/disk.h" : NO
Has header "sys/ioccom.h" : NO
Has header "sys/kcov.h" : NO
Checking for function "close_range" : YES
Checking for function "accept4" : YES
Checking for function "clock_adjtime" : YES
Checking for function "dup3" : YES
Checking for function "fallocate" : YES
Checking for function "posix_fallocate" : YES
Checking for function "posix_memalign" : YES
Checking for function "_aligned_malloc" : NO
Checking for function "valloc" : YES
Checking for function "memalign" : YES
Checking for function "ppoll" : YES
Checking for function "preadv" : YES
Checking for function "pthread_fchdir_np" : NO
Checking for function "sendfile" : YES
Checking for function "setns" : YES
Checking for function "unshare" : YES
Checking for function "syncfs" : YES
Checking for function "sync_file_range" : YES
Checking for function "timerfd_create" : YES
Checking for function "copy_file_range" : YES
Checking for function "getifaddrs" : YES
Checking for function "openpty" with dependency -lutil: YES
Checking for function "strchrnul" : YES
Checking for function "system" : YES
Header <sys/epoll.h> has symbol "epoll_create1" : YES
Header <linux/falloc.h> has symbol "FALLOC_FL_PUNCH_HOLE" : YES
Header <linux/falloc.h> has symbol "FALLOC_FL_KEEP_SIZE" : YES
Header <linux/falloc.h> has symbol "FALLOC_FL_ZERO_RANGE" : YES
Has header "linux/fiemap.h" : YES
Header <linux/fs.h> has symbol "FS_IOC_FIEMAP" : YES
Checking for function "getrandom" : YES
Header <sys/random.h> has symbol "GRND_NONBLOCK" : YES
Header <sys/inotify.h> has symbol "inotify_init" : YES
Header <sys/inotify.h> has symbol "inotify_init1" : YES
Header <sys/prctl.h> has symbol "PR_SET_TIMERSLACK" : YES
Header <linux/rtnetlink.h> has symbol "IFLA_PROTO_DOWN" : YES
Header <sys/sysmacros.h> has symbol "makedev" : YES
Header <getopt.h> has symbol "optreset" : NO
Header <netinet/in.h> has symbol "IPPROTO_MPTCP" : YES
Checking whether type "struct sigevent" has member "sigev_notify_thread_id" : NO
Checking whether type "struct stat" has member "st_atim" : YES
Checking for type "struct iovec" : YES
Checking for type "struct utmpx" : YES
Checking for type "struct mmsghdr" : YES
Header <linux/vm_sockets.h> has symbol "AF_VSOCK" : YES
Program scripts/minikconf.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/minikconf.py)
Configuring aarch64-softmmu-config-target.h using configuration
Configuring aarch64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/aarch64-softmmu-config-devices.mak.d
Configuring aarch64-softmmu-config-devices.h using configuration
Configuring alpha-softmmu-config-target.h using configuration
Configuring alpha-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/alpha-softmmu-config-devices.mak.d
Configuring alpha-softmmu-config-devices.h using configuration
Configuring arm-softmmu-config-target.h using configuration
Configuring arm-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/arm-softmmu-config-devices.mak.d
Configuring arm-softmmu-config-devices.h using configuration
Configuring avr-softmmu-config-target.h using configuration
Configuring avr-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/avr-softmmu-config-devices.mak.d
Configuring avr-softmmu-config-devices.h using configuration
Configuring cris-softmmu-config-target.h using configuration
Configuring cris-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/cris-softmmu-config-devices.mak.d
Configuring cris-softmmu-config-devices.h using configuration
Configuring hppa-softmmu-config-target.h using configuration
Configuring hppa-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/hppa-softmmu-config-devices.mak.d
Configuring hppa-softmmu-config-devices.h using configuration
Configuring i386-softmmu-config-target.h using configuration
Configuring i386-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/i386-softmmu-config-devices.mak.d
Configuring i386-softmmu-config-devices.h using configuration
Configuring loongarch64-softmmu-config-target.h using configuration
Configuring loongarch64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/loongarch64-softmmu-config-devices.mak.d
Configuring loongarch64-softmmu-config-devices.h using configuration
Configuring m68k-softmmu-config-target.h using configuration
Configuring m68k-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/m68k-softmmu-config-devices.mak.d
Configuring m68k-softmmu-config-devices.h using configuration
Configuring microblaze-softmmu-config-target.h using configuration
Configuring microblaze-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/microblaze-softmmu-config-devices.mak.d
Configuring microblaze-softmmu-config-devices.h using configuration
Configuring microblazeel-softmmu-config-target.h using configuration
Configuring microblazeel-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/microblazeel-softmmu-config-devices.mak.d
Configuring microblazeel-softmmu-config-devices.h using configuration
Configuring mips-softmmu-config-target.h using configuration
Configuring mips-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/mips-softmmu-config-devices.mak.d
Configuring mips-softmmu-config-devices.h using configuration
Configuring mips64-softmmu-config-target.h using configuration
Configuring mips64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/mips64-softmmu-config-devices.mak.d
Configuring mips64-softmmu-config-devices.h using configuration
Configuring mips64el-softmmu-config-target.h using configuration
Configuring mips64el-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/mips64el-softmmu-config-devices.mak.d
Configuring mips64el-softmmu-config-devices.h using configuration
Configuring mipsel-softmmu-config-target.h using configuration
Configuring mipsel-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/mipsel-softmmu-config-devices.mak.d
Configuring mipsel-softmmu-config-devices.h using configuration
Configuring nios2-softmmu-config-target.h using configuration
Configuring nios2-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/nios2-softmmu-config-devices.mak.d
Configuring nios2-softmmu-config-devices.h using configuration
Configuring or1k-softmmu-config-target.h using configuration
Configuring or1k-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/or1k-softmmu-config-devices.mak.d
Configuring or1k-softmmu-config-devices.h using configuration
Configuring ppc-softmmu-config-target.h using configuration
Configuring ppc-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/ppc-softmmu-config-devices.mak.d
Configuring ppc-softmmu-config-devices.h using configuration
Configuring ppc64-softmmu-config-target.h using configuration
Configuring ppc64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/ppc64-softmmu-config-devices.mak.d
Configuring ppc64-softmmu-config-devices.h using configuration
Configuring riscv32-softmmu-config-target.h using configuration
Configuring riscv32-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/riscv32-softmmu-config-devices.mak.d
Configuring riscv32-softmmu-config-devices.h using configuration
Configuring riscv64-softmmu-config-target.h using configuration
Configuring riscv64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/riscv64-softmmu-config-devices.mak.d
Configuring riscv64-softmmu-config-devices.h using configuration
Configuring rx-softmmu-config-target.h using configuration
Configuring rx-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/rx-softmmu-config-devices.mak.d
Configuring rx-softmmu-config-devices.h using configuration
Configuring s390x-softmmu-config-target.h using configuration
Configuring s390x-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/s390x-softmmu-config-devices.mak.d
Configuring s390x-softmmu-config-devices.h using configuration
Configuring sh4-softmmu-config-target.h using configuration
Configuring sh4-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/sh4-softmmu-config-devices.mak.d
Configuring sh4-softmmu-config-devices.h using configuration
Configuring sh4eb-softmmu-config-target.h using configuration
Configuring sh4eb-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/sh4eb-softmmu-config-devices.mak.d
Configuring sh4eb-softmmu-config-devices.h using configuration
Configuring sparc-softmmu-config-target.h using configuration
Configuring sparc-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/sparc-softmmu-config-devices.mak.d
Configuring sparc-softmmu-config-devices.h using configuration
Configuring sparc64-softmmu-config-target.h using configuration
Configuring sparc64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/sparc64-softmmu-config-devices.mak.d
Configuring sparc64-softmmu-config-devices.h using configuration
Configuring tricore-softmmu-config-target.h using configuration
Configuring tricore-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/tricore-softmmu-config-devices.mak.d
Configuring tricore-softmmu-config-devices.h using configuration
Configuring x86_64-softmmu-config-target.h using configuration
Configuring x86_64-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/x86_64-softmmu-config-devices.mak.d
Configuring x86_64-softmmu-config-devices.h using configuration
Configuring xtensa-softmmu-config-target.h using configuration
Configuring xtensa-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/xtensa-softmmu-config-devices.mak.d
Configuring xtensa-softmmu-config-devices.h using configuration
Configuring xtensaeb-softmmu-config-target.h using configuration
Configuring xtensaeb-softmmu-config-devices.mak with command
Reading depfile: /home/echo/Software/qemu-8.0.5/build/meson-private/xtensaeb-softmmu-config-devices.mak.d
Configuring xtensaeb-softmmu-config-devices.h using configuration
Configuring aarch64-linux-user-config-target.h using configuration
Configuring aarch64_be-linux-user-config-target.h using configuration
Configuring alpha-linux-user-config-target.h using configuration
Configuring arm-linux-user-config-target.h using configuration
Configuring armeb-linux-user-config-target.h using configuration
Configuring cris-linux-user-config-target.h using configuration
Configuring hexagon-linux-user-config-target.h using configuration
Configuring hppa-linux-user-config-target.h using configuration
Configuring i386-linux-user-config-target.h using configuration
Configuring loongarch64-linux-user-config-target.h using configuration
Configuring m68k-linux-user-config-target.h using configuration
Configuring microblaze-linux-user-config-target.h using configuration
Configuring microblazeel-linux-user-config-target.h using configuration
Configuring mips-linux-user-config-target.h using configuration
Configuring mips64-linux-user-config-target.h using configuration
Configuring mips64el-linux-user-config-target.h using configuration
Configuring mipsel-linux-user-config-target.h using configuration
Configuring mipsn32-linux-user-config-target.h using configuration
Configuring mipsn32el-linux-user-config-target.h using configuration
Configuring nios2-linux-user-config-target.h using configuration
Configuring or1k-linux-user-config-target.h using configuration
Configuring ppc-linux-user-config-target.h using configuration
Configuring ppc64-linux-user-config-target.h using configuration
Configuring ppc64le-linux-user-config-target.h using configuration
Configuring riscv32-linux-user-config-target.h using configuration
Configuring riscv64-linux-user-config-target.h using configuration
Configuring s390x-linux-user-config-target.h using configuration
Configuring sh4-linux-user-config-target.h using configuration
Configuring sh4eb-linux-user-config-target.h using configuration
Configuring sparc-linux-user-config-target.h using configuration
Configuring sparc32plus-linux-user-config-target.h using configuration
Configuring sparc64-linux-user-config-target.h using configuration
Configuring x86_64-linux-user-config-target.h using configuration
Configuring xtensa-linux-user-config-target.h using configuration
Configuring xtensaeb-linux-user-config-target.h using configuration
Program scripts/make-config-poison.sh found: YES (/home/echo/Software/qemu-8.0.5/scripts/make-config-poison.sh)
Run-time dependency capstone found: NO (tried pkgconfig)
Library fdt found: NO
Configuring config-host.h using configuration
Program scripts/hxtool found: YES (/home/echo/Software/qemu-8.0.5/scripts/hxtool)
Program scripts/shaderinclude.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/shaderinclude.py)
Program scripts/qapi-gen.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/qapi-gen.py)
Program scripts/qemu-version.sh found: YES (/home/echo/Software/qemu-8.0.5/scripts/qemu-version.sh)

Executing subproject libvhost-user

libvhost-user| Project name: libvhost-user
libvhost-user| Project version: undefined
libvhost-user| C compiler for the host machine: cc -m64 -mcx16 (gcc 11.4.0 "cc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0")
libvhost-user| C linker for the host machine: cc -m64 -mcx16 ld.bfd 2.38
libvhost-user| Compiler for C supports arguments -Wsign-compare: YES
libvhost-user| Compiler for C supports arguments -Wdeclaration-after-statement: YES
libvhost-user| Compiler for C supports arguments -Wstrict-aliasing: YES
libvhost-user| Dependency threads found: YES unknown (cached)
libvhost-user| Dependency glib-2.0 found: YES 2.72.4 (overridden)
libvhost-user| Build targets in project: 10
libvhost-user| Subproject libvhost-user finished.


Executing subproject libvduse

libvduse| Project name: libvduse
libvduse| Project version: undefined
libvduse| C compiler for the host machine: cc -m64 -mcx16 (gcc 11.4.0 "cc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0")
libvduse| C linker for the host machine: cc -m64 -mcx16 ld.bfd 2.38
libvduse| Compiler for C supports arguments -Wsign-compare: YES (cached)
libvduse| Compiler for C supports arguments -Wdeclaration-after-statement: YES (cached)
libvduse| Compiler for C supports arguments -Wstrict-aliasing: YES (cached)
libvduse| Build targets in project: 11
libvduse| Subproject libvduse finished.

Program scripts/decodetree.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/decodetree.py)
Program flex found: YES (/usr/bin/flex)
Program bison found: YES 3.8.2 (/usr/bin/bison)
Dependency glib-2.0 found: YES 2.72.4 (overridden)
Program indent found: NO
Program ../scripts/modules/module_block.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/block/../scripts/modules/module_block.py)
Program ../scripts/block-coroutine-wrapper.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/block/../scripts/block-coroutine-wrapper.py)
Program scripts/modinfo-collect.py found: YES (/home/echo/Software/qemu-8.0.5/scripts/modinfo-collect.py)
Program scripts/modinfo-generate.py found: YES (/home/echo/Software/qemu-8.0.5/scripts/modinfo-generate.py)
Program nm found: YES
Program scripts/undefsym.py found: YES (/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/undefsym.py)
Program scripts/feature_to_c.sh found: YES (/bin/sh /home/echo/Software/qemu-8.0.5/scripts/feature_to_c.sh)
Configuring 50-edk2-i386-secure.json using configuration
Configuring 50-edk2-x86_64-secure.json using configuration
Configuring 60-edk2-aarch64.json using configuration
Configuring 60-edk2-arm.json using configuration
Configuring 60-edk2-i386.json using configuration
Configuring 60-edk2-x86_64.json using configuration
Program qemu-keymap found: NO
Program sphinx-build found: NO
Program bash found: YES 5.1.16 (/usr/bin/bash)
Program diff found: YES (/usr/bin/diff)
Program dbus-daemon found: YES (/usr/bin/dbus-daemon)
Did not find CMake 'cmake'
Found CMake: NO
Run-time dependency gvnc-1.0 found: NO (tried pkgconfig and cmake)
Run-time dependency sysprof-capture-4 found: NO (tried pkgconfig and cmake)
Program initrd-stress.sh found: YES (/home/echo/Software/qemu-8.0.5/tests/migration/initrd-stress.sh)
Build targets in project: 731

qemu 8.0.5

  Directories
    Install prefix               : /usr/local
    BIOS directory               : share/qemu
    firmware path                : share/qemu-firmware
    binary directory             : /usr/local/bin
    library directory            : /usr/local/lib/x86_64-linux-gnu
    module directory             : lib/x86_64-linux-gnu/qemu
    libexec directory            : /usr/local/libexec
    include directory            : /usr/local/include
    config directory             : /usr/local/etc
    local state directory        : /var/local
    Manual directory             : /usr/local/share/man
    Doc directory                : /usr/local/share/doc
    Build directory              : /home/echo/Software/qemu-8.0.5/build
    Source path                  : /home/echo/Software/qemu-8.0.5
    GIT submodules               : ui/keycodemapdb tests/fp/berkeley-testfloat-3 tests/fp/berkeley-softfloat-3 dtc

  Host binaries
    git                          : git
    make                         : make
    python                       : /usr/bin/python3 (version: 3.10)
    sphinx-build                 : NO
    gdb                          : /usr/bin/gdb
    iasl                         : NO
    genisoimage                  : /usr/bin/genisoimage

  Configurable features
    Documentation                : NO
    system-mode emulation        : YES
    user-mode emulation          : YES
    block layer                  : YES
    Install blobs                : YES
    module support               : NO
    fuzzing support              : NO
    Audio drivers                : oss
    Trace backends               : log
    D-Bus display                : YES
    QOM debugging                : NO
    vhost-kernel support         : YES
    vhost-net support            : YES
    vhost-user support           : YES
    vhost-user-crypto support    : YES
    vhost-user-blk server support: YES
    vhost-vdpa support           : YES
    build guest agent            : YES

  Compilation
    host CPU                     : x86_64
    host endianness              : little
    C compiler                   : cc -m64 -mcx16
    Host C compiler              : cc -m64 -mcx16
    C++ compiler                 : c++ -m64 -mcx16
    CFLAGS                       : -g -O2
    CXXFLAGS                     : -g -O2
    QEMU_CFLAGS                  : -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -fno-strict-aliasing -fno-common -fwrapv -Wundef -Wwrite-strings -Wmissing-prototypes -Wstrict-prototypes -Wredundant-decls -Wold-style-declaration -Wold-style-definition -Wtype-limits -Wformat-security -Wformat-y2k -Winit-self -Wignored-qualifiers -Wempty-body -Wnested-externs -Wendif-labels -Wexpansion-to-defined -Wimplicit-fallthrough=2 -Wmissing-format-attribute -Wno-missing-include-dirs -Wno-shift-negative-value -Wno-psabi -fstack-protector-strong
    QEMU_CXXFLAGS                : -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -fno-strict-aliasing -fno-common -fwrapv -Wundef -Wwrite-strings -Wtype-limits -Wformat-security -Wformat-y2k -Winit-self -Wignored-qualifiers -Wempty-body -Wendif-labels -Wexpansion-to-defined -Wimplicit-fallthrough=2 -Wmissing-format-attribute -Wno-missing-include-dirs -Wno-shift-negative-value -Wno-psabi -fstack-protector-strong
    QEMU_LDFLAGS                 : -fstack-protector-strong -Wl,-z,relro -Wl,-z,now -Wl,--warn-common
    profiler                     : NO
    link-time optimization (LTO) : NO
    PIE                          : YES
    static build                 : NO
    malloc trim support          : YES
    membarrier                   : NO
    debug graph lock             : NO
    debug stack usage            : NO
    mutex debugging              : NO
    memory allocator             : system
    avx2 optimization            : YES
    avx512bw optimization        : YES
    avx512f optimization         : NO
    gprof                        : NO
    gcov                         : NO
    thread sanitizer             : NO
    CFI support                  : NO
    strip binaries               : NO
    sparse                       : NO
    mingw32 support              : NO

  Cross compilers
    aarch64                      : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc aarch64-linux-gnu-gcc-10 -i qemu/debian-arm64-cross -s /home/echo/Software/qemu-8.0.5 --
    alpha                        : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc alpha-linux-gnu-gcc -i qemu/debian-alpha-cross -s /home/echo/Software/qemu-8.0.5 --
    arm                          : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc arm-linux-gnueabihf-gcc -i qemu/debian-armhf-cross -s /home/echo/Software/qemu-8.0.5 --
    i386                         : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc gcc -i qemu/fedora-i386-cross -s /home/echo/Software/qemu-8.0.5 --
    loongarch64                  : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc loongarch64-unknown-linux-gnu-gcc -i qemu/debian-loongarch-cross -s /home/echo/Software/qemu-8.0.5 --
    nios2                        : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc nios2-linux-gnu-gcc -i qemu/debian-nios2-cross -s /home/echo/Software/qemu-8.0.5 --
    riscv64                      : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc riscv64-linux-gnu-gcc -i qemu/debian-riscv64-test-cross -s /home/echo/Software/qemu-8.0.5 --
    s390x                        : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc s390x-linux-gnu-gcc -i qemu/debian-s390x-cross -s /home/echo/Software/qemu-8.0.5 --
    x86_64                       : cc
    xtensa                       : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc /opt/2020.07/xtensa-dc232b-elf/bin/xtensa-dc232b-elf-gcc -i qemu/debian-xtensa-cross -s /home/echo/Software/qemu-8.0.5 --
    xtensaeb                     : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc /opt/2020.07/xtensa-dc232b-elf/bin/xtensa-dc232b-elf-gcc -i qemu/debian-xtensa-cross -s /home/echo/Software/qemu-8.0.5 --
    cris                         : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc cris-linux-gnu-gcc -i qemu/fedora-cris-cross -s /home/echo/Software/qemu-8.0.5 --
    hexagon                      : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc hexagon-unknown-linux-musl-clang -i qemu/debian-hexagon-cross -s /home/echo/Software/qemu-8.0.5 --
    hppa                         : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc hppa-linux-gnu-gcc -i qemu/debian-hppa-cross -s /home/echo/Software/qemu-8.0.5 --
    m68k                         : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc m68k-linux-gnu-gcc -i qemu/debian-m68k-cross -s /home/echo/Software/qemu-8.0.5 --
    microblaze                   : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc microblaze-linux-musl-gcc -i qemu/debian-microblaze-cross -s /home/echo/Software/qemu-8.0.5 --
    mips                         : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc mips-linux-gnu-gcc -i qemu/debian-mips-cross -s /home/echo/Software/qemu-8.0.5 --
    mips64                       : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc mips64-linux-gnuabi64-gcc -i qemu/debian-mips64-cross -s /home/echo/Software/qemu-8.0.5 --
    mips64el                     : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc mips64el-linux-gnuabi64-gcc -i qemu/debian-mips64el-cross -s /home/echo/Software/qemu-8.0.5 --
    mipsel                       : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc mipsel-linux-gnu-gcc -i qemu/debian-mipsel-cross -s /home/echo/Software/qemu-8.0.5 --
    ppc                          : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc powerpc-linux-gnu-gcc-10 -i qemu/debian-powerpc-test-cross -s /home/echo/Software/qemu-8.0.5 --
    ppc64                        : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc powerpc64-linux-gnu-gcc-10 -i qemu/debian-powerpc-test-cross -s /home/echo/Software/qemu-8.0.5 --
    ppc64le                      : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc powerpc64le-linux-gnu-gcc-10 -i qemu/debian-powerpc-test-cross -s /home/echo/Software/qemu-8.0.5 --
    sh4                          : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc sh4-linux-gnu-gcc -i qemu/debian-sh4-cross -s /home/echo/Software/qemu-8.0.5 --
    sparc64                      : /usr/bin/python3 -B /home/echo/Software/qemu-8.0.5/tests/docker/docker.py --engine podman cc --cc sparc64-linux-gnu-gcc -i qemu/debian-sparc64-cross -s /home/echo/Software/qemu-8.0.5 --

  Targets and accelerators
    KVM support                  : YES
    HAX support                  : NO
    HVF support                  : NO
    WHPX support                 : NO
    NVMM support                 : NO
    Xen support                  : NO
    Xen emulation                : YES
    TCG support                  : YES
    TCG backend                  : native (x86_64)
    TCG plugins                  : YES
    TCG debug enabled            : NO
    target list                  : aarch64-softmmu alpha-softmmu arm-softmmu avr-softmmu cris-softmmu hppa-softmmu i386-softmmu loongarch64-softmmu m68k-softmmu microblaze-softmmu microblazeel-softmmu mips-softmmu mips64-softmmu mips64el-softmmu mipsel-softmmu nios2-softmmu or1k-softmmu ppc-softmmu ppc64-softmmu riscv32-softmmu riscv64-softmmu rx-softmmu s390x-softmmu sh4-softmmu sh4eb-softmmu sparc-softmmu sparc64-softmmu tricore-softmmu x86_64-softmmu xtensa-softmmu xtensaeb-softmmu aarch64-linux-user aarch64_be-linux-user alpha-linux-user arm-linux-user armeb-linux-user cris-linux-user hexagon-linux-user hppa-linux-user i386-linux-user loongarch64-linux-user m68k-linux-user microblaze-linux-user microblazeel-linux-user mips-linux-user mips64-linux-user mips64el-linux-user mipsel-linux-user mipsn32-linux-user mipsn32el-linux-user nios2-linux-user or1k-linux-user ppc-linux-user ppc64-linux-user ppc64le-linux-user riscv32-linux-user riscv64-linux-user s390x-linux-user sh4-linux-user sh4eb-linux-user sparc-linux-user sparc32plus-linux-user sparc64-linux-user x86_64-linux-user xtensa-linux-user xtensaeb-linux-user
    default devices              : YES
    out of process emulation     : YES
    vfio-user server             : NO

  Block layer support
    coroutine backend            : ucontext
    coroutine pool               : YES
    Block whitelist (rw)         :
    Block whitelist (ro)         :
    Use block whitelist in tools : NO
    VirtFS support               : NO
    Live block migration         : YES
    replication support          : YES
    bochs support                : YES
    cloop support                : YES
    dmg support                  : YES
    qcow v1 support              : YES
    vdi support                  : YES
    vvfat support                : YES
    qed support                  : YES
    parallels support            : YES
    FUSE exports                 : NO
    VDUSE block exports          : YES

  Crypto
    TLS priority                 : NORMAL
    GNUTLS support               : NO
    libgcrypt                    : NO
    nettle                       : NO
    AF_ALG support               : NO
    rng-none                     : NO
    Linux keyring                : YES

  Dependencies
    SDL support                  : NO
    SDL image support            : NO
    GTK support                  : NO
    pixman                       : YES 0.40.0
    VTE support                  : NO
    slirp support                : NO
    libtasn1                     : NO
    PAM                          : NO
    iconv support                : YES
    curses support               : NO
    virgl support                : NO
    blkio support                : NO
    curl support                 : NO
    Multipath support            : NO
    PNG support                  : NO
    VNC support                  : YES
    VNC SASL support             : NO
    VNC JPEG support             : NO
    OSS support                  : YES
    sndio support                : NO
    ALSA support                 : NO
    PulseAudio support           : NO
    JACK support                 : NO
    brlapi support               : NO
    vde support                  : NO
    netmap support               : NO
    l2tpv3 support               : YES
    Linux AIO support            : NO
    Linux io_uring support       : NO
    ATTR/XATTR support           : YES
    RDMA support                 : NO
    PVRDMA support               : NO
    fdt support                  : internal
    libcap-ng support            : NO
    bpf support                  : NO
    spice protocol support       : NO
    rbd support                  : NO
    smartcard support            : NO
    U2F support                  : NO
    libusb                       : NO
    usb net redir                : NO
    OpenGL support (epoxy)       : NO
    GBM                          : NO
    libiscsi support             : NO
    libnfs support               : NO
    seccomp support              : NO
    GlusterFS support            : NO
    TPM support                  : YES
    libssh support               : NO
    lzo support                  : NO
    snappy support               : NO
    bzip2 support                : NO
    lzfse support                : NO
    zstd support                 : NO
    NUMA host support            : NO
    capstone                     : NO
    libpmem support              : NO
    libdaxctl support            : NO
    libudev                      : NO
    FUSE lseek                   : NO
    selinux                      : YES 3.3
    libdw                        : NO

  Subprojects
    libvduse                     : YES
    libvhost-user                : YES

  User defined options
    Native files                 : config-meson.cross
    prefix                       : /usr/local
    vfio_user_server             : disabled

Found ninja-1.10.1 at /usr/bin/ninja
Running postconf script '/usr/bin/python3 /home/echo/Software/qemu-8.0.5/scripts/symlink-install-tree.py'
```

