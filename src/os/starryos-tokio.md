# tokio在StarryOS中的系统调用分析

使用官方的`Echo-Server`实例 [https://tokio.rs/tokio/tutorial/io#echo-server](https://tokio.rs/tokio/tutorial/io#echo-server)

```shell
strace ./tokio-echo
execve("./tokio-echo", ["./tokio-echo"], 0xffffc88f8720 /* 26 vars */) = 0
brk(NULL)                               = 0xaaab0a9b1000
faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=34745, ...}) = 0
mmap(NULL, 34745, PROT_READ, MAP_PRIVATE, 3, 0) = 0xffff9af97000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\320)\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=80200, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff9af95000
mmap(NULL, 144472, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9af4d000
mprotect(0xffff9af60000, 61440, PROT_NONE) = 0
mmap(0xffff9af6f000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x12000) = 0xffff9af6f000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0Ha\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=164304, ...}) = 0
mmap(NULL, 197624, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9af1c000
mprotect(0xffff9af38000, 61440, PROT_NONE) = 0
mmap(0xffff9af47000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0xffff9af47000
mmap(0xffff9af49000, 13304, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xffff9af49000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\272\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=633832, ...}) = 0
mmap(NULL, 696440, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9ae71000
mprotect(0xffff9af0a000, 65536, PROT_NONE) = 0
mmap(0xffff9af1a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x99000) = 0xffff9af1a000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0P\17\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14560, ...}) = 0
mmap(NULL, 78080, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9ae5d000
mprotect(0xffff9ae60000, 61440, PROT_NONE) = 0
mmap(0xffff9ae6f000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0xffff9ae6f000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0`\17\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1450832, ...}) = 0
mmap(NULL, 1519552, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9acea000
mprotect(0xffff9ae45000, 61440, PROT_NONE) = 0
mmap(0xffff9ae54000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15a000) = 0xffff9ae54000
mmap(0xffff9ae5a000, 12224, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xffff9ae5a000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff9af93000
mprotect(0xffff9ae54000, 16384, PROT_READ) = 0
mprotect(0xffff9ae6f000, 4096, PROT_READ) = 0
mprotect(0xffff9af1a000, 4096, PROT_READ) = 0
mprotect(0xffff9af47000, 4096, PROT_READ) = 0
mprotect(0xffff9af6f000, 4096, PROT_READ) = 0
mprotect(0xaaaae43ca000, 24576, PROT_READ) = 0
mprotect(0xffff9afa2000, 4096, PROT_READ) = 0
munmap(0xffff9af97000, 34745)           = 0
set_tid_address(0xffff9af938f0)         = 2378159
set_robust_list(0xffff9af93900, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0xffff9af21bd0, sa_mask=[], sa_flags=SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0xffff9af21c90, sa_mask=[], sa_flags=SA_RESTART|SA_SIGINFO}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
ppoll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, {tv_sec=0, tv_nsec=0}, NULL, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0xaaaae436d758, sa_mask=[], sa_flags=SA_ONSTACK|SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0xaaaae436d758, sa_mask=[], sa_flags=SA_ONSTACK|SA_SIGINFO}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff9af9b000
mprotect(0xffff9af9b000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0xffff9af9c000, ss_flags=0, ss_size=16384}, NULL) = 0
brk(NULL)                               = 0xaaab0a9b1000
brk(0xaaab0a9d2000)                     = 0xaaab0a9d2000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
fstat(3, {st_mode=S_IFREG|0444, st_size=0, ...}) = 0
read(3, "aaaae4309000-aaaae43ba000 r-xp 0"..., 1024) = 1024
read(3, "/lib/aarch64-linux-gnu/libdl-2.3"..., 1024) = 1024
read(3, "usr/lib/aarch64-linux-gnu/libpth"..., 1024) = 1024
read(3, "w-p 00000000 00:00 0 \nffff9afa00"..., 1024) = 485
close(3)                                = 0
sched_getaffinity(2378159, 32, [0, 1, 2, 3]) = 8
getrandom("\x28\xae\x81\x2d\x85\x70\xb9\x51\x9c\x6a\x19\xf4\x6b\x42\x63\xbe", 16, 0x4 /* GRND_??? */) = 16
openat(AT_FDCWD, "/proc/self/cgroup", O_RDONLY|O_CLOEXEC) = 3
read(3, "11:blkio:/user.slice\n10:freezer:"..., 8192) = 319
read(3, "", 8192)                       = 0
close(3)                                = 0
openat(AT_FDCWD, "/proc/self/mountinfo", O_RDONLY|O_CLOEXEC) = 3
read(3, "27 33 0:25 / /sys rw,nosuid,node"..., 8192) = 3728
close(3)                                = 0
openat(AT_FDCWD, "/sys/fs/cgroup/cpu,cpuacct/user.slice/cpu.cfs_quota_us", O_RDONLY|O_CLOEXEC) = 3
statx(3, "", AT_STATX_SYNC_AS_STAT|AT_EMPTY_PATH, STATX_ALL, {stx_mask=STATX_BASIC_STATS, stx_attributes=0, stx_mode=S_IFREG|0644, stx_size=0, ...}) = 0
lseek(3, 0, SEEK_CUR)                   = 0
read(3, "-1\n", 32)                     = 3
read(3, "", 29)                         = 0
close(3)                                = 0
sched_getaffinity(0, 128, [0, 1, 2, 3]) = 8
epoll_create1(EPOLL_CLOEXEC)            = 3
eventfd2(0, EFD_CLOEXEC|EFD_NONBLOCK)   = 4
epoll_ctl(3, EPOLL_CTL_ADD, 4, {EPOLLIN|EPOLLRDHUP|EPOLLET, {u32=0, u64=0}}) = 0
fcntl(3, F_DUPFD_CLOEXEC, 3)            = 5
socketpair(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0, [6, 7]) = 0
fcntl(6, F_DUPFD_CLOEXEC, 3)            = 8
epoll_ctl(5, EPOLL_CTL_ADD, 8, {EPOLLIN|EPOLLRDHUP|EPOLLET, {u32=1, u64=1}}) = 0
futex(0xffff9ae700e8, FUTEX_WAKE_PRIVATE, 2147483647) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff9aae9000
mprotect(0xffff9aaea000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0xffff9ace89d0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[2378160], tls=0xffff9ace97d0, child_tidptr=0xffff9ace91a0) = 2378160
futex(0xffff9ace9898, FUTEX_WAKE_PRIVATE, 1) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff9a8e3000
mprotect(0xffff9a8e4000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0xffff9aae29d0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[2378161], tls=0xffff9aae37d0, child_tidptr=0xffff9aae31a0) = 2378161
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff9a6dd000
mprotect(0xffff9a6de000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0xffff9a8dc9d0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[2378162], tls=0xffff9a8dd7d0, child_tidptr=0xffff9a8dd1a0) = 2378162
futex(0xffff9aae3898, FUTEX_WAKE_PRIVATE, 1) = 1
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff9a4d7000
mprotect(0xffff9a4d8000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0xffff9a6d69d0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[2378163], tls=0xffff9a6d77d0, child_tidptr=0xffff9a6d71a0) = 2378163
futex(0xffff9a6d7898, FUTEX_WAKE_PRIVATE, 1) = 1
socket(AF_INET, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, IPPROTO_IP) = 9
setsockopt(9, SOL_SOCKET, SO_REUSEADDR, [1], 4) = 0
bind(9, {sa_family=AF_INET, sin_port=htons(12345), sin_addr=inet_addr("0.0.0.0")}, 16) = 0
listen(9, 1024)                         = 0
epoll_ctl(5, EPOLL_CTL_ADD, 9, {EPOLLIN|EPOLLOUT|EPOLLRDHUP|EPOLLET, {u32=178000256, u64=187651594129792}}) = 0
futex(0xffff9af93fe8, FUTEX_WAIT_PRIVATE, 1, NULL
```

```shell
strace -c ./tokio-echo
strace: Process 2381013 detached
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 20.30    0.000458          32        14           read
 17.38    0.000392          98         4         1 futex
 14.98    0.000338          84         4           clone
  9.62    0.000217          10        20           mmap
  7.31    0.000165          16        10           openat
  5.27    0.000119           7        17           mprotect
  4.30    0.000097           9        10           close
  3.41    0.000077          25         3           epoll_ctl
  2.13    0.000048          48         1           socketpair
  1.95    0.000044          44         1           socket
  1.91    0.000043          21         2           sched_getaffinity
  1.73    0.000039          19         2           fcntl
  1.68    0.000038          38         1           bind
  1.37    0.000031          31         1           listen
  1.24    0.000028          28         1           setsockopt
  1.20    0.000027          27         1           eventfd2
  1.20    0.000027          27         1           statx
  1.15    0.000026          26         1           epoll_create1
  1.11    0.000025          25         1           getrandom
  0.75    0.000017          17         1           lseek
  0.00    0.000000           0         1         1 faccessat
  0.00    0.000000           0         1           ppoll
  0.00    0.000000           0         7           fstat
  0.00    0.000000           0         1           set_tid_address
  0.00    0.000000           0         1           set_robust_list
  0.00    0.000000           0         2           sigaltstack
  0.00    0.000000           0         7           rt_sigaction
  0.00    0.000000           0         1           rt_sigprocmask
  0.00    0.000000           0         3           brk
  0.00    0.000000           0         1           munmap
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         2           prlimit64
------ ----------- ----------- --------- --------- ----------------
100.00    0.002256                   124         2 total
```