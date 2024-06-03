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
execve("./dora", ["./dora"], 0x7ffe72ffd5f0 /* 28 vars */) = 0
brk(NULL)                               = 0x55e52cb74000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fff585086c0) = -1 EINVAL (无效的参数)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f812ef94000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=59967, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 59967, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f812ef85000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=125488, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 127720, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f812ef65000
mmap(0x7f812ef68000, 94208, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7f812ef68000
mmap(0x7f812ef7f000, 16384, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1a000) = 0x7f812ef7f000
mmap(0x7f812ef83000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d000) = 0x7f812ef83000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=940560, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 942344, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f812ee7e000
mmap(0x7f812ee8c000, 507904, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xe000) = 0x7f812ee8c000
mmap(0x7f812ef08000, 372736, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x8a000) = 0x7f812ef08000
mmap(0x7f812ef63000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xe4000) = 0x7f812ef63000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\237\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0 \0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0"..., 48, 848) = 48
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\226 \25\252\235\23<l\274\3731\3540\5\226\327"..., 68, 896) = 68
newfstatat(3, "", {st_mode=S_IFREG|0755, st_size=2220400, ...}, AT_EMPTY_PATH) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 2264656, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f812ec00000
mprotect(0x7f812ec28000, 2023424, PROT_NONE) = 0
mmap(0x7f812ec28000, 1658880, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x28000) = 0x7f812ec28000
mmap(0x7f812edbd000, 360448, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1bd000) = 0x7f812edbd000
mmap(0x7f812ee16000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x215000) = 0x7f812ee16000
mmap(0x7f812ee1c000, 52816, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f812ee1c000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f812ee7c000
arch_prctl(ARCH_SET_FS, 0x7f812ee7cf40) = 0
set_tid_address(0x7f812ee7d210)         = 108064
set_robust_list(0x7f812ee7d220, 24)     = 0
rseq(0x7f812ee7d8e0, 0x20, 0, 0x53053053) = 0
mprotect(0x7f812ee16000, 16384, PROT_READ) = 0
mprotect(0x7f812ef63000, 4096, PROT_READ) = 0
mprotect(0x7f812ef83000, 4096, PROT_READ) = 0
mprotect(0x55e52beb4000, 987136, PROT_READ) = 0
mprotect(0x7f812efce000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7f812ef85000, 59967)           = 0
poll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f812ec42520}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x55e52b8a95f0, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7f812ec42520}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x55e52b8a95f0, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7f812ec42520}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f812ef91000
mprotect(0x7f812ef91000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0x7f812ef92000, ss_flags=0, ss_size=8192}, NULL) = 0
getrandom("\xe8\xd8\x65\xb7\x01\x5d\xa9\x5f", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x55e52cb74000
brk(0x55e52cb95000)                     = 0x55e52cb95000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
newfstatat(3, "", {st_mode=S_IFREG|0444, st_size=0, ...}, AT_EMPTY_PATH) = 0
read(3, "55e52ab21000-55e52ac80000 r--p 0"..., 1024) = 1024
read(3, "12ee15000-7f812ee16000 ---p 0021"..., 1024) = 1024
read(3, "                /usr/lib/x86_64-"..., 1024) = 1024
read(3, "               /usr/lib/x86_64-l"..., 1024) = 507
close(3)                                = 0
sched_getaffinity(108064, 32, [0, 1, 2, 3]) = 8
ioctl(1, TIOCGWINSZ, 0x7fff58505520)    = -1 ENOTTY (对设备不适当的 ioctl 操作)
ioctl(2, TIOCGWINSZ, {ws_row=39, ws_col=80, ws_xpixel=800, ws_ypixel=819}) = 0
ioctl(2, TIOCGWINSZ, {ws_row=39, ws_col=80, ws_xpixel=800, ws_ypixel=819}) = 0
ioctl(2, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(2, TCGETS, {B38400 opost isig icanon echo ...}) = 0
write(2, "dora-rs cli client\n\n\33[1m\33[4mUsag"..., 1388dora-rs cli client

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
) = 1388
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=8192}, NULL) = 0
munmap(0x7f812ef91000, 12288)           = 0
exit_group(2)                           = ?
+++ exited with 2 +++
```