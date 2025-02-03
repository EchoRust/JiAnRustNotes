# `Dora`在Linux中的系统调用分析

`Dora`是一个面向数据流的机器人应用框架，使用`Rust`语言开发。让机器人应用变得快速而简单。

[查看官网](https://dora.carsmos.ai)

## 在 `x86_64` `Linux` 中的系统调用分析

```shell
dora-rs cli client

Usage: dora <COMMAND>

Commands:
  check        Check if the coordinator and the daemon is running
  graph        Generate a visualization of the given graph using mermaid.js.
                   Use --open to open browser
  build        Run build commands provided in the given dataflow
  new          Generate a new project, node or operator. Choose the language
                   between Rust, Python, C or C++
  up           Spawn a coordinator and a daemon
  destroy      Destroy running coordinator and daemon. If some dataflows are
                   still running, they will be stopped first
  start        Start the given dataflow path. Attach a name to the running
                   dataflow by using --name
  stop         Stop the given dataflow UUID. If no id is provided, you will
                   be able to choose between the running dataflows
  list         List running dataflows
  logs         Show logs of a given dataflow and node
  daemon       Run daemon
  runtime      Run runtime
  coordinator  Run coordinator
  help         Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
  0.00    0.000000           0         7           read
  0.00    0.000000           0         1           write
  0.00    0.000000           0         5           close
  0.00    0.000000           0         1           poll
  0.00    0.000000           0        17           mmap
  0.00    0.000000           0         7           mprotect
  0.00    0.000000           0         2           munmap
  0.00    0.000000           0         3           brk
  0.00    0.000000           0         5           rt_sigaction
  0.00    0.000000           0         5         1 ioctl
  0.00    0.000000           0         4           pread64
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         3           sigaltstack
  0.00    0.000000           0         2         1 arch_prctl
  0.00    0.000000           0         1           sched_getaffinity
  0.00    0.000000           0         1           set_tid_address
  0.00    0.000000           0         5           openat
  0.00    0.000000           0         5           newfstatat
  0.00    0.000000           0         1           set_robust_list
  0.00    0.000000           0         2           prlimit64
  0.00    0.000000           0         1           getrandom
  0.00    0.000000           0         1           rseq
------ ----------- ----------- --------- --------- ------------------
100.00    0.000000           0        81         3 total
```

```shell
$ strace -f -v dora -h
execve("/home/echo/Dev/dora/target/release/dora", ["/home/echo/Dev/dora/target/relea"..., "-h"], ["SHELL=/bin/bash", "CONDA_EXE=/home/echo/miniconda3/"..., "_CE_M=", "LANGUAGE=zh_CN:zh", "PWD=/home/echo", "LOGNAME=echo", "XDG_SESSION_TYPE=tty", "CONDA_PREFIX=/home/echo/minicond"..., "MOTD_SHOWN=pam", "HOME=/home/echo", "LANG=C.UTF-8", "LS_COLORS=rs=0:di=01;34:ln=01;36"..., "CONDA_PROMPT_MODIFIER=(base) ", "SSH_CONNECTION=172.16.203.10 493"..., "LESSCLOSE=/usr/bin/lesspipe %s %"..., "XDG_SESSION_CLASS=user", "TERM=xterm-256color", "_CE_CONDA=", "LESSOPEN=| /usr/bin/lesspipe %s", "USER=echo", "CONDA_SHLVL=1", "SHLVL=1", "XDG_SESSION_ID=4", "CONDA_PYTHON_EXE=/home/echo/mini"..., "LC_CTYPE=C.UTF-8", "XDG_RUNTIME_DIR=/run/user/1000", "SSH_CLIENT=172.16.203.10 49374 2"..., "CONDA_DEFAULT_ENV=base", "XDG_DATA_DIRS=/usr/share/gnome:/"..., "PATH=/home/echo/miniconda3/bin:/"..., "DBUS_SESSION_BUS_ADDRESS=unix:pa"..., "SSH_TTY=/dev/pts/2", "TERM_PROGRAM=WarpTerminal", "_=/usr/bin/strace"]) = 0
brk(NULL)                               = 0x55b1f9020000
arch_prctl(0x3001 /* ARCH_??? */, 0x7ffee484d3c0) = -1 EINVAL (无效的参数)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7bcf735db000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_dev=makedev(0x8, 0x3), st_ino=19176588, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=128, st_size=62119, st_atime=1719544210 /* 2024-06-28T11:10:10.669757705+0800 */, st_atime_nsec=669757705, st_mtime=1719544210 /* 2024-06-28T11:10:10.645757704+0800 */, st_mtime_nsec=645757704, st_ctime=1719544210 /* 2024-06-28T11:10:10.649757704+0800 */, st_ctime_nsec=649757704}, AT_EMPTY_PATH) = 0
mmap(NULL, 62119, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7bcf735cb000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
newfstatat(3, "", {st_dev=makedev(0x8, 0x3), st_ino=2214, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=248, st_size=125488, st_atime=1719476439 /* 2024-06-27T16:20:39.642014627+0800 */, st_atime_nsec=642014627, st_mtime=1683963205 /* 2023-05-13T15:33:25+0800 */, st_mtime_nsec=0, st_ctime=1718364975 /* 2024-06-14T19:36:15.293760191+0800 */, st_ctime_nsec=293760191}, AT_EMPTY_PATH) = 0
mmap(NULL, 127720, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7bcf735ab000
mmap(0x7bcf735ae000, 94208, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7bcf735ae000
mmap(0x7bcf735c5000, 16384, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1a000) = 0x7bcf735c5000
mmap(0x7bcf735c9000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d000) = 0x7bcf735c9000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
newfstatat(3, "", {st_dev=makedev(0x8, 0x3), st_ino=7202, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=1840, st_size=940560, st_atime=1719469836 /* 2024-06-27T14:30:36.397596395+0800 */, st_atime_nsec=397596395, st_mtime=1715027668 /* 2024-05-07T04:34:28+0800 */, st_mtime_nsec=0, st_ctime=1718364975 /* 2024-06-14T19:36:15.982167719+0800 */, st_ctime_nsec=982167719}, AT_EMPTY_PATH) = 0
mmap(NULL, 942344, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7bcf734c4000
mmap(0x7bcf734d2000, 507904, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xe000) = 0x7bcf734d2000
mmap(0x7bcf7354e000, 372736, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x8a000) = 0x7bcf7354e000
mmap(0x7bcf735a9000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xe4000) = 0x7bcf735a9000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\237\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0 \0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0"..., 48, 848) = 48
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0I\17\357\204\3$\f\221\2039x\324\224\323\236S"..., 68, 896) = 68
newfstatat(3, "", {st_dev=makedev(0x8, 0x3), st_ino=2727, st_mode=S_IFREG|0755, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=4344, st_size=2220400, st_atime=1719461821 /* 2024-06-27T12:17:01.796767217+0800 */, st_atime_nsec=796767217, st_mtime=1715027668 /* 2024-05-07T04:34:28+0800 */, st_mtime_nsec=0, st_ctime=1718364975 /* 2024-06-14T19:36:15.982167719+0800 */, st_ctime_nsec=982167719}, AT_EMPTY_PATH) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 2264656, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7bcf73200000
mprotect(0x7bcf73228000, 2023424, PROT_NONE) = 0
mmap(0x7bcf73228000, 1658880, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x28000) = 0x7bcf73228000
mmap(0x7bcf733bd000, 360448, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1bd000) = 0x7bcf733bd000
mmap(0x7bcf73416000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x215000) = 0x7bcf73416000
mmap(0x7bcf7341c000, 52816, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7bcf7341c000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7bcf734c2000
arch_prctl(ARCH_SET_FS, 0x7bcf734c2f40) = 0
set_tid_address(0x7bcf734c3210)         = 4271
set_robust_list(0x7bcf734c3220, 24)     = 0
rseq(0x7bcf734c38e0, 0x20, 0, 0x53053053) = 0
mprotect(0x7bcf73416000, 16384, PROT_READ) = 0
mprotect(0x7bcf735a9000, 4096, PROT_READ) = 0
mprotect(0x7bcf735c9000, 4096, PROT_READ) = 0
mprotect(0x55b1f821a000, 991232, PROT_READ) = 0
mprotect(0x7bcf73615000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7bcf735cb000, 62119)           = 0
poll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7bcf73242520}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x55b1f7c33f30, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7bcf73242520}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x55b1f7c33f30, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7bcf73242520}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7bcf735d8000
mprotect(0x7bcf735d8000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0x7bcf735d9000, ss_flags=0, ss_size=8192}, NULL) = 0
getrandom("\xd2\x86\xbe\x67\x6f\x87\x31\x6e", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x55b1f9020000
brk(0x55b1f9041000)                     = 0x55b1f9041000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
newfstatat(3, "", {st_dev=makedev(0, 0x16), st_ino=49288, st_mode=S_IFREG|0444, st_nlink=1, st_uid=1000, st_gid=1000, st_blksize=1024, st_blocks=0, st_size=0, st_atime=1719544658 /* 2024-06-28T11:17:38.901251488+0800 */, st_atime_nsec=901251488, st_mtime=1719544658 /* 2024-06-28T11:17:38.901251488+0800 */, st_mtime_nsec=901251488, st_ctime=1719544658 /* 2024-06-28T11:17:38.901251488+0800 */, st_ctime_nsec=901251488}, AT_EMPTY_PATH) = 0
read(3, "55b1f6e22000-55b1f6f82000 r--p 0"..., 1024) = 1024
read(3, "f73415000-7bcf73416000 ---p 0021"..., 1024) = 1024
read(3, "                /usr/lib/x86_64-"..., 1024) = 1024
read(3, "               /usr/lib/x86_64-l"..., 1024) = 507
close(3)                                = 0
sched_getaffinity(4271, 32, [0, 1, 2, 3]) = 8
ioctl(1, TIOCGWINSZ, {ws_row=29, ws_col=86, ws_xpixel=991, ws_ypixel=691}) = 0
ioctl(1, TIOCGWINSZ, {ws_row=29, ws_col=86, ws_xpixel=991, ws_ypixel=691}) = 0
ioctl(1, TCGETS, {c_iflags=0x2102, c_oflags=0x5, c_cflags=0xbd, c_lflags=0xca3b, c_line=0, c_cc="\x03\x1c\x7f\x15\x04\x00\x01\x00\x11\x13\x1a\x00\x12\x0f\x17\x16\x00\x00\x00"}) = 0
ioctl(1, TCGETS, {c_iflags=0x2102, c_oflags=0x5, c_cflags=0xbd, c_lflags=0xca3b, c_line=0, c_cc="\x03\x1c\x7f\x15\x04\x00\x01\x00\x11\x13\x1a\x00\x12\x0f\x17\x16\x00\x00\x00"}) = 0
write(1, "dora-rs cli client\n\n\33[1m\33[4mUsag"..., 1388dora-rs cli client

Usage: dora <COMMAND>

Commands:
  check        Check if the coordinator and the daemon is running
  graph        Generate a visualization of the given graph using mermaid.js. Use
                   --open to open browser
  build        Run build commands provided in the given dataflow
  new          Generate a new project, node or operator. Choose the language
                   between Rust, Python, C or C++
  up           Spawn a coordinator and a daemon
  destroy      Destroy running coordinator and daemon. If some dataflows are still
                   running, they will be stopped first
  start        Start the given dataflow path. Attach a name to the running
                   dataflow by using --name
  stop         Stop the given dataflow UUID. If no id is provided, you will be
                   able to choose between the running dataflows
  list         List running dataflows
  logs         Show logs of a given dataflow and node
  daemon       Run daemon
  runtime      Run runtime
  coordinator  Run coordinator
  help         Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
) = 1388
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=8192}, NULL) = 0
munmap(0x7bcf735d8000, 12288)           = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```