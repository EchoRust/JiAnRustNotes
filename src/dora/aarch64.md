# Dora在 aarch64 架构中的系统调用分析

## 在 `aarch64` `Linux` 中的系统调用分析

```shell
dora-rs cli client

Usage: dora <COMMAND>

Commands:
  check        Check if the coordinator and the daemon is running
  graph        Generate a visualization of the given graph using mermaid.js. Use --open to
                   open browser
  build        Run build commands provided in the given dataflow
  new          Generate a new project, node or operator. Choose the language between Rust,
                   Python, C or C++
  up           Spawn a coordinator and a daemon
  destroy      Destroy running coordinator and daemon. If some dataflows are still running,
                   they will be stopped first
  start        Start the given dataflow path. Attach a name to the running dataflow by using
                   --name
  stop         Stop the given dataflow UUID. If no id is provided, you will be able to choose
                   between the running dataflows
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
------ ----------- ----------- --------- --------- ----------------
 39.30    0.001780        1780         1           execve
 10.36    0.000469          24        19           mmap
  7.99    0.000362          45         8           openat
  7.95    0.000360          24        15           mprotect
  6.93    0.000314          31        10           read
  6.60    0.000299         299         1           write
  3.62    0.000164          20         8           fstat
  3.29    0.000149          18         8           close
  3.02    0.000137          19         7           rt_sigaction
  2.43    0.000110          22         5         1 ioctl
  1.99    0.000090          45         2           munmap
  1.41    0.000064          21         3           sigaltstack
  1.28    0.000058          19         3           brk
  0.93    0.000042          42         1           ppoll
  0.79    0.000036          18         2           prlimit64
  0.75    0.000034          34         1         1 faccessat
  0.51    0.000023          23         1           sched_getaffinity
  0.31    0.000014          14         1           rt_sigprocmask
  0.26    0.000012          12         1           set_tid_address
  0.26    0.000012          12         1           set_robust_list
------ ----------- ----------- --------- --------- ----------------
100.00    0.004529                    98         2 total
```

```shell
$ strace -f -v dora -h
execve("/home/ubuntu/EchoData/bin/dora", ["dora", "-h"], ["SHELL=/bin/bash", "PWD=/home/ubuntu", "LOGNAME=ubuntu", "XDG_SESSION_TYPE=tty", "MOTD_SHOWN=pam", "HOME=/home/ubuntu", "LANG=C.UTF-8", "LS_COLORS=rs=0:di=01;34:ln=01;36"..., "SSH_CONNECTION=103.233.162.226 6"..., "LESSCLOSE=/usr/bin/lesspipe %s %"..., "XDG_SESSION_CLASS=user", "TERM=xterm-256color", "LESSOPEN=| /usr/bin/lesspipe %s", "USER=ubuntu", "SHLVL=1", "XDG_SESSION_ID=68884", "LC_CTYPE=C.UTF-8", "XDG_RUNTIME_DIR=/run/user/1000", "SSH_CLIENT=103.233.162.226 6781 "..., "XDG_DATA_DIRS=/usr/local/share:/"..., "PATH=/home/ubuntu/.cargo/bin:/us"..., "DBUS_SESSION_BUS_ADDRESS=unix:pa"..., "SSH_TTY=/dev/pts/0", "TERM_PROGRAM=WarpTerminal", "_=/usr/bin/strace"]) = 0
brk(NULL)                               = 0xaaab09806000
faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=3852, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=72, st_size=34745, st_atime=1719469201 /* 2024-06-27T06:20:01.656854826+0000 */, st_atime_nsec=656854826, st_mtime=1718259523 /* 2024-06-13T06:18:43.934348858+0000 */, st_mtime_nsec=934348858, st_ctime=1718259523 /* 2024-06-13T06:18:43.938348891+0000 */, st_ctime_nsec=938348891}) = 0
mmap(NULL, 34745, PROT_READ, MAP_PRIVATE, 3, 0) = 0xffff90469000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0P\17\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=8904, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=32, st_size=14560, st_atime=1719468961 /* 2024-06-27T06:16:01.378693788+0000 */, st_atime_nsec=378693788, st_mtime=1714501218 /* 2024-04-30T18:20:18+0000 */, st_mtime_nsec=0, st_ctime=1717049757 /* 2024-05-30T06:15:57.180178270+0000 */, st_ctime_nsec=180178270}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff90467000
mmap(NULL, 78080, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9042f000
mprotect(0xffff90432000, 61440, PROT_NONE) = 0
mmap(0xffff90441000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0xffff90441000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\320)\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=28456, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=160, st_size=80200, st_atime=1719471001 /* 2024-06-27T06:50:01.387789263+0000 */, st_atime_nsec=387789263, st_mtime=1688885130 /* 2023-07-09T06:45:30+0000 */, st_mtime_nsec=0, st_ctime=1690871450 /* 2023-08-01T06:30:50.652317995+0000 */, st_ctime_nsec=652317995}) = 0
mmap(NULL, 144472, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9040b000
mprotect(0xffff9041e000, 61440, PROT_NONE) = 0
mmap(0xffff9042d000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x12000) = 0xffff9042d000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/librt.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\0\37\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=9385, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=64, st_size=31584, st_atime=1719469222 /* 2024-06-27T06:20:22.484684134+0000 */, st_atime_nsec=484684134, st_mtime=1714501218 /* 2024-04-30T18:20:18+0000 */, st_mtime_nsec=0, st_ctime=1717049757 /* 2024-05-30T06:15:57.180178270+0000 */, st_ctime_nsec=180178270}) = 0
mmap(NULL, 95032, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff903f3000
mprotect(0xffff903fa000, 61440, PROT_NONE) = 0
mmap(0xffff90409000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0xffff90409000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0Ha\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=9149, st_mode=S_IFREG|0755, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=328, st_size=164304, st_atime=1719468961 /* 2024-06-27T06:16:01.378693788+0000 */, st_atime_nsec=378693788, st_mtime=1714501218 /* 2024-04-30T18:20:18+0000 */, st_mtime_nsec=0, st_ctime=1717049757 /* 2024-05-30T06:15:57.180178270+0000 */, st_ctime_nsec=180178270}) = 0
mmap(NULL, 197624, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff903c2000
mprotect(0xffff903de000, 61440, PROT_NONE) = 0
mmap(0xffff903ed000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0xffff903ed000
mmap(0xffff903ef000, 13304, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xffff903ef000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\272\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=8915, st_mode=S_IFREG|0644, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=1240, st_size=633832, st_atime=1719468961 /* 2024-06-27T06:16:01.366693873+0000 */, st_atime_nsec=366693873, st_mtime=1714501218 /* 2024-04-30T18:20:18+0000 */, st_mtime_nsec=0, st_ctime=1717049757 /* 2024-05-30T06:15:57.180178270+0000 */, st_ctime_nsec=180178270}) = 0
mmap(NULL, 696440, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff90317000
mprotect(0xffff903b0000, 65536, PROT_NONE) = 0
mmap(0xffff903c0000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x99000) = 0xffff903c0000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0`\17\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_dev=makedev(0x8, 0x2), st_ino=8899, st_mode=S_IFREG|0755, st_nlink=1, st_uid=0, st_gid=0, st_blksize=4096, st_blocks=2840, st_size=1450832, st_atime=1719468961 /* 2024-06-27T06:16:01.374693817+0000 */, st_atime_nsec=374693817, st_mtime=1714501218 /* 2024-04-30T18:20:18+0000 */, st_mtime_nsec=0, st_ctime=1717049757 /* 2024-05-30T06:15:57.180178270+0000 */, st_ctime_nsec=180178270}) = 0
mmap(NULL, 1519552, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff901a4000
mprotect(0xffff902ff000, 61440, PROT_NONE) = 0
mmap(0xffff9030e000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15a000) = 0xffff9030e000
mmap(0xffff90314000, 12224, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xffff90314000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff90465000
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff901a1000
mprotect(0xffff9030e000, 16384, PROT_READ) = 0
mprotect(0xffff903c0000, 4096, PROT_READ) = 0
mprotect(0xffff903ed000, 4096, PROT_READ) = 0
mprotect(0xffff90409000, 4096, PROT_READ) = 0
mprotect(0xffff9042d000, 4096, PROT_READ) = 0
mprotect(0xffff90441000, 4096, PROT_READ) = 0
mprotect(0xaaaae9122000, 974848, PROT_READ) = 0
mprotect(0xffff90474000, 4096, PROT_READ) = 0
munmap(0xffff90469000, 34745)           = 0
set_tid_address(0xffff901a10e0)         = 1033268
set_robust_list(0xffff901a10f0, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0xffff903c7bd0, sa_mask=[], sa_flags=SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0xffff903c7c90, sa_mask=[], sa_flags=SA_RESTART|SA_SIGINFO}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
ppoll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, {tv_sec=0, tv_nsec=0}, NULL, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0xaaaae8b48400, sa_mask=[], sa_flags=SA_ONSTACK|SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0xaaaae8b48400, sa_mask=[], sa_flags=SA_ONSTACK|SA_SIGINFO}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff9046d000
mprotect(0xffff9046d000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0xffff9046e000, ss_flags=0, ss_size=16384}, NULL) = 0
brk(NULL)                               = 0xaaab09806000
brk(0xaaab09827000)                     = 0xaaab09827000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
fstat(3, {st_dev=makedev(0, 0x5), st_ino=73819201, st_mode=S_IFREG|0444, st_nlink=1, st_uid=1000, st_gid=1000, st_blksize=1024, st_blocks=0, st_size=0, st_atime=1719542835 /* 2024-06-28T02:47:15.193150107+0000 */, st_atime_nsec=193150107, st_mtime=1719542835 /* 2024-06-28T02:47:15.193150107+0000 */, st_mtime_nsec=193150107, st_ctime=1719542835 /* 2024-06-28T02:47:15.193150107+0000 */, st_ctime_nsec=193150107}) = 0
read(3, "aaaae7f4a000-aaaae9113000 r-xp 0"..., 1024) = 1024
read(3, "000000 08:02 8915               "..., 1024) = 1024
read(3, "b/aarch64-linux-gnu/librt-2.31.s"..., 1024) = 1024
read(3, "lib/aarch64-linux-gnu/libdl-2.31"..., 1024) = 985
close(3)                                = 0
sched_getaffinity(1033268, 32, [0, 1, 2, 3]) = 8
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
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=16384}, NULL) = 0
munmap(0xffff9046d000, 20480)           = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```