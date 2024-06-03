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
execve("/home/ubuntu/EchoData/bin/dora", ["dora"], 0xfffffc4892a0 /* 25 vars */) = 0
brk(NULL)                               = 0xaaaaf0b90000
faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=34745, ...}) = 0
mmap(NULL, 34745, PROT_READ, MAP_PRIVATE, 3, 0) = 0xffff90803000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0P\17\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14560, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff90801000
mmap(NULL, 78080, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff907c9000
mprotect(0xffff907cc000, 61440, PROT_NONE) = 0
mmap(0xffff907db000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0xffff907db000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\320)\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=80200, ...}) = 0
mmap(NULL, 144472, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff907a5000
mprotect(0xffff907b8000, 61440, PROT_NONE) = 0
mmap(0xffff907c7000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x12000) = 0xffff907c7000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/librt.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\0\37\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=31584, ...}) = 0
mmap(NULL, 95032, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9078d000
mprotect(0xffff90794000, 61440, PROT_NONE) = 0
mmap(0xffff907a3000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0xffff907a3000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0Ha\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=164304, ...}) = 0
mmap(NULL, 197624, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9075c000
mprotect(0xffff90778000, 61440, PROT_NONE) = 0
mmap(0xffff90787000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0xffff90787000
mmap(0xffff90789000, 13304, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xffff90789000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\272\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=633832, ...}) = 0
mmap(NULL, 696440, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff906b1000
mprotect(0xffff9074a000, 65536, PROT_NONE) = 0
mmap(0xffff9075a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x99000) = 0xffff9075a000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0`\17\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1450832, ...}) = 0
mmap(NULL, 1519552, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xffff9053e000
mprotect(0xffff90699000, 61440, PROT_NONE) = 0
mmap(0xffff906a8000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15a000) = 0xffff906a8000
mmap(0xffff906ae000, 12224, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xffff906ae000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff907ff000
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xffff9053b000
mprotect(0xffff906a8000, 16384, PROT_READ) = 0
mprotect(0xffff9075a000, 4096, PROT_READ) = 0
mprotect(0xffff90787000, 4096, PROT_READ) = 0
mprotect(0xffff907a3000, 4096, PROT_READ) = 0
mprotect(0xffff907c7000, 4096, PROT_READ) = 0
mprotect(0xffff907db000, 4096, PROT_READ) = 0
mprotect(0xaaaad93f3000, 974848, PROT_READ) = 0
mprotect(0xffff9080e000, 4096, PROT_READ) = 0
munmap(0xffff90803000, 34745)           = 0
set_tid_address(0xffff9053b0e0)         = 1468778
set_robust_list(0xffff9053b0f0, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0xffff90761bd0, sa_mask=[], sa_flags=SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0xffff90761c90, sa_mask=[], sa_flags=SA_RESTART|SA_SIGINFO}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
ppoll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, {tv_sec=0, tv_nsec=0}, NULL, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0xaaaad8e19400, sa_mask=[], sa_flags=SA_ONSTACK|SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0xaaaad8e19400, sa_mask=[], sa_flags=SA_ONSTACK|SA_SIGINFO}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0xffff90807000
mprotect(0xffff90807000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0xffff90808000, ss_flags=0, ss_size=16384}, NULL) = 0
brk(NULL)                               = 0xaaaaf0b90000
brk(0xaaaaf0bb1000)                     = 0xaaaaf0bb1000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
fstat(3, {st_mode=S_IFREG|0444, st_size=0, ...}) = 0
read(3, "aaaad821b000-aaaad93e4000 r-xp 0"..., 1024) = 1024
read(3, "000000 08:02 9509               "..., 1024) = 1024
read(3, "b/aarch64-linux-gnu/librt-2.31.s"..., 1024) = 1024
read(3, "lib/aarch64-linux-gnu/libdl-2.31"..., 1024) = 985
close(3)                                = 0
sched_getaffinity(1468778, 32, [0, 1, 2, 3]) = 8
ioctl(1, TIOCGWINSZ, 0xffffddd76c28)    = -1 ENOTTY (Inappropriate ioctl for device)
ioctl(2, TIOCGWINSZ, {ws_row=44, ws_col=98, ws_xpixel=1022, ws_ypixel=897}) = 0
ioctl(2, TIOCGWINSZ, {ws_row=44, ws_col=98, ws_xpixel=1022, ws_ypixel=897}) = 0
ioctl(2, TCGETS, {B9600 opost isig icanon echo ...}) = 0
ioctl(2, TCGETS, {B9600 opost isig icanon echo ...}) = 0
write(2, "dora-rs cli client\n\n\33[1m\33[4mUsag"..., 1388dora-rs cli client

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
) = 1388
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=16384}, NULL) = 0
munmap(0xffff90807000, 20480)           = 0
exit_group(2)                           = ?
+++ exited with 2 +++
```