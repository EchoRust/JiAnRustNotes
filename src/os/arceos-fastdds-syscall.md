# ArceOS系统调用 - FastDDS在ArceOS中的系统调用

## 发布者

```shell
/bin/DDSHelloWorldExample publisher
Starting 
terminate called after throwing an instance of 'std::runtime_error'
  what():  random_device::random_device(const std::string&)
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
 31.70    0.004095         102        40           mmap
 16.70    0.002157         308         7           writev
 13.78    0.001781         222         8           munmap
 12.31    0.001591         198         8           recvfrom
  9.54    0.001233         308         4           sendto
  4.45    0.000575         287         2           close
  2.83    0.000366         366         1           set_tid_address
  2.63    0.000340         113         3         3 openat
  1.91    0.000247         123         2           socket
  1.29    0.000167          83         2           brk
  0.98    0.000127          63         2           rt_sigprocmask
  0.53    0.000069          69         1           tkill
  0.52    0.000067          67         1           ioctl
  0.48    0.000062          62         1           sched_getaffinity
  0.33    0.000043          43         1           getpid
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ------------------
100.00    0.012920         153        84         3 total
Aborted
```

## 订阅者

```shell
/bin/DDSHelloWorldExample subscriber
Starting 
terminate called after throwing an instance of 'std::runtime_error'
  what():  random_device::random_device(const std::string&)
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
 23.69    0.004032         100        40           mmap
 18.03    0.003069        3069         1           execve
 14.94    0.002543         363         7           writev
 14.09    0.002399         599         4           sendto
 10.78    0.001835         229         8           recvfrom
  8.58    0.001461         182         8           munmap
  1.84    0.000314         314         1           set_tid_address
  1.79    0.000305         152         2           close
  1.54    0.000263          87         3         3 openat
  1.47    0.000251         250         1           tkill
  0.83    0.000141          70         2           brk
  0.72    0.000122          61         2           rt_sigprocmask
  0.70    0.000119          59         2           socket
  0.39    0.000067          67         1           ioctl
  0.33    0.000057          57         1           sched_getaffinity
  0.26    0.000045          45         1           getpid
------ ----------- ----------- --------- --------- ------------------
100.00    0.017023         202        84         3 total
Aborted
```

## `ArceOS` 在 `RISC-V` 架构下的系统调用整理

| #     | syscall                                                                             | prototype                                                                                                                              |
| ----- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `222` | [mmap](https://man7.org/linux/man-pages/man2/mmap.2.html)                           | asmlinkage long sys_old_mmap(struct mmap_arg_struct __user *arg);                                                                      |
| `221` | [execve](https://man7.org/linux/man-pages/man2/execve.2.html)                       | asmlinkage long sys_execve(const char __user *filename, const char __user *const __user *argv, const char __user *const __user *envp); |
| `66`  | [writev](https://man7.org/linux/man-pages/man2/writev.2.html)                       | asmlinkage long sys_writev(unsigned long fd, const struct iovec __user *vec, unsigned long vlen);                                      |
| `206` | [sendto](https://man7.org/linux/man-pages/man2/sendto.2.html)                       | asmlinkage long sys_sendto(int, void __user *, size_t, unsigned, struct sockaddr __user *, int);                                       |
| `207` | [recvfrom](https://man7.org/linux/man-pages/man2/recvfrom.2.html)                   | asmlinkage long sys_recvfrom(int, void __user *, size_t, unsigned, struct sockaddr __user *, int __user *);                            |
| `215` | [munmap](https://man7.org/linux/man-pages/man2/munmap.2.html)                       | asmlinkage long sys_munmap(unsigned long addr, size_t len);                                                                            |
| `96`  | [set_tid_address](https://man7.org/linux/man-pages/man2/set_tid_address.2.html)     | asmlinkage long sys_set_tid_address(int __user *tidptr);                                                                               |
| `57`  | [close](https://man7.org/linux/man-pages/man2/close.2.html)                         | asmlinkage long sys_close(unsigned int fd);                                                                                            |
| `56`  | [openat](https://man7.org/linux/man-pages/man2/openat.2.html)                       | asmlinkage long sys_openat(int dfd, const char __user *filename, int flags, umode_t mode);                                             |
| `130` | [tkill](https://man7.org/linux/man-pages/man2/tkill.2.html)                         | asmlinkage long sys_tkill(pid_t pid, int sig);                                                                                         |
| `214` | [brk](https://man7.org/linux/man-pages/man2/brk.2.html)                             | asmlinkage long sys_brk(unsigned long brk);                                                                                            |
| `135` | [rt_sigprocmask](https://man7.org/linux/man-pages/man2/rt_sigprocmask.2.html)       | asmlinkage long sys_rt_sigprocmask(int how, sigset_t __user *set, sigset_t __user *oset, size_t sigsetsize);                           |
| `198` | [socket](https://man7.org/linux/man-pages/man2/socket.2.html)                       | asmlinkage long sys_socket(int, int, int);                                                                                             |
| `29`  | [ioctl](https://man7.org/linux/man-pages/man2/ioctl.2.html)                         | asmlinkage long sys_ioctl(unsigned int fd, unsigned int cmd, unsigned long arg);                                                       |
| `123` | [sched_getaffinity](https://man7.org/linux/man-pages/man2/sched_getaffinity.2.html) | asmlinkage long sys_sched_getaffinity(pid_t pid, unsigned int len, unsigned long __user *user_mask_ptr);                               |
| `172` | [getpid](https://man7.org/linux/man-pages/man2/getpid.2.html)                       | asmlinkage long sys_getpid(void);                                                                                                      |

### 下面是`LatencyTest`新增的系统调用

| #     | syscall                                                                     | prototype                                                                                                                            |
| ----- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `220` | [clone](https://man7.org/linux/man-pages/man2/clone.2.html)                 | asmlinkage long sys_clone(unsigned long, unsigned long, int __user *, unsigned long, int __user *);                                  |
| `422` | [futex](https://man7.org/linux/man-pages/man2/futex.2.html)                 | asmlinkage long sys_futex(u32 __user *uaddr, int op, u32 val, struct __kernel_timespec __user *utime, u32 __user *uaddr2, u32 val3); |
| `226` | [mprotect](https://man7.org/linux/man-pages/man2/mprotect.2.html)           | asmlinkage long sys_mprotect(unsigned long start, size_t len, unsigned long prot);                                                   |
| `430` | [clock_gettime](https://man7.org/linux/man-pages/man2/clock_gettime.2.html) | asmlinkage long sys_clock_gettime(clockid_t which_clock, struct __kernel_timespec __user *tp);                                       |

## `LatencyTest` 在 `RISC-V` 架构下的系统调用整理

```shell
/bin/LatencyTest both
Performing intraprocess test with 1 subscribers and 10000 samples
terminate called after throwing an instance of 'std::runtime_error'
  what():  random_device::random_device(const std::string&)
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
 17.81    0.002460          51        48           mmap
 17.44    0.002409         344         7           writev
 16.88    0.002331        2331         1           execve
 10.79    0.001491         135        11           munmap
  9.25    0.001278         159         8           recvfrom
  5.97    0.000825         206         4           sendto
  5.68    0.000784         392         2           close
  3.79    0.000524         262         2           socket
  2.54    0.000351         351         1           clone
  2.32    0.000321          80         4         1 futex
  2.19    0.000302          60         5           rt_sigprocmask
  1.01    0.000140          70         2           brk
  0.89    0.000123          41         3         3 openat
  0.76    0.000105         105         1           mprotect
  0.62    0.000085          85         1           ioctl
  0.45    0.000062          62         1           set_tid_address
  0.45    0.000062          62         1           tkill
  0.44    0.000061          61         1           clock_gettime
  0.41    0.000057          57         1           sched_getaffinity
  0.30    0.000041          41         1           getpid
------ ----------- ----------- --------- --------- ------------------
100.00    0.013812         131       105         4 total
```


```shell
/bin/DDSHelloWorldExample publisher
11:27:35.889478 execve("/bin/DDSHelloWorldExample", ["/bin/DDSHelloWorldExample", "publisher"], 0x7fffc74d5d68 /* 5 vars */) = 0
11:27:35.909762 set_tid_address(0x6a8064) = 78
11:27:35.915371 brk(NULL)               = 0x6a9000
11:27:35.917362 brk(0x6ab000)           = 0x6ab000
11:27:35.919784 mmap(0x6a9000, 4096, PROT_NONE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x6a9000
11:27:35.922174 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ff000
11:27:35.925289 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996fe000
11:27:35.928390 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996fd000
11:27:35.930635 sched_getaffinity(0, 128, [0]) = 8
11:27:35.933744 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996fc000
11:27:35.936317 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996fb000
11:27:35.940153 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996fa000
11:27:35.943408 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f9000
11:27:35.946195 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f8000
11:27:35.949598 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f7000
11:27:35.952911 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f6000
11:27:35.955841 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f5000
11:27:35.958930 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f3000
11:27:35.962314 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f1000
11:27:35.965586 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996f0000
11:27:35.967853 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ee000
11:27:35.978082 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ed000
11:27:35.981671 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ec000
11:27:35.983949 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ea000
11:27:35.986637 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e9000
11:27:35.989694 munmap(0x7fff996ec000, 4096) = 0
11:27:35.997639 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ec000
11:27:36.000587 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e7000
11:27:36.003539 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e6000
11:27:36.005790 munmap(0x7fff996ec000, 4096) = 0
11:27:36.008303 munmap(0x7fff996e7000, 8192) = 0
11:27:36.009869 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e7000
11:27:36.012084 munmap(0x7fff996e7000, 8192) = 0
11:27:36.014086 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e7000
11:27:36.016313 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e4000
11:27:36.018485 munmap(0x7fff996e6000, 4096) = 0
11:27:36.020713 munmap(0x7fff996e7000, 8192) = 0
11:27:36.022729 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e7000
11:27:36.025887 munmap(0x7fff996e7000, 8192) = 0
11:27:36.028056 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e7000
11:27:36.032498 mmap(NULL, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e0000
11:27:36.037210 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996ec000
11:27:36.039783 mmap(NULL, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996dc000
11:27:36.044199 mmap(NULL, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996d8000
11:27:36.048757 mmap(NULL, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996d4000
11:27:36.053048 mmap(NULL, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996d0000
11:27:36.057484 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996e6000
11:27:36.060407 mmap(NULL, 73728, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996be000
11:27:36.063257 ioctl(1, TIOCGWINSZ, {ws_row=0, ws_col=0, ws_xpixel=0, ws_ypixel=0}) = 0
11:27:36.067601 writev(1, [{iov_base="Starting ", iov_len=9}, {iov_base="\n", iov_len=1}], 2Starting ) = 10
11:27:36.072236 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996bd000
11:27:36.075311 openat(AT_FDCWD, "DEFAULT_FASTRTPS_PROFILES.xml", O_RDONLY|O_LARGEFILE) = -1 ENOENT (No such file or directory)
11:27:36.079455 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff996bc000
11:27:36.082347 openat(AT_FDCWD, "/etc/localtime", O_RDONLY|O_NONBLOCK|O_LARGEFILE|O_CLOEXEC) = -1 ENOENT (No such file or directory)
11:27:36.084500 socket(AF_NETLINK, SOCK_RAW|SOCK_CLOEXEC, NETLINK_ROUTE) = 3
11:27:36.087662 sendto(3, "\24\0\0\0\22\0\1\3\1\0\0\0\0\0\0\0\0\0\0\0", 20, 0, NULL, 0) = 20
11:27:36.091438 recvfrom(3, "\234\5\0\0\20\0\2\0\1\0\0\0N\0\0\0\0\0\4\3\1\0\0\0\10\0\0\0\0\0\0\0"..., 8192, MSG_DONTWAIT, NULL, NULL) = 2912
11:27:36.094200 recvfrom(3, "\310\5\0\0\20\0\2\0\1\0\0\0N\0\0\0\0\0\1\0\3\0\0\0\2\20\0\0\0\0\0\0"..., 8192, MSG_DONTWAIT, NULL, NULL) = 3044
11:27:36.096445 recvfrom(3, "\24\0\0\0\3\0\2\0\1\0\0\0N\0\0\0\0\0\0\0", 8192, MSG_DONTWAIT, NULL, NULL) = 20
11:27:36.098220 sendto(3, "\24\0\0\0\26\0\1\3\2\0\0\0\0\0\0\0\0\0\0\0", 20, 0, NULL, 0) = 20
11:27:36.100156 recvfrom(3, "\24\0\0\0\3\0\2\0\2\0\0\0N\0\0\0\0\0\0\0", 8192, MSG_DONTWAIT, NULL, NULL) = 20
11:27:36.101887 close(3)                = 0
11:27:36.103466 socket(AF_NETLINK, SOCK_RAW|SOCK_CLOEXEC, NETLINK_ROUTE) = 3
11:27:36.106009 sendto(3, "\24\0\0\0\22\0\1\3\1\0\0\0\0\0\0\0\0\0\0\0", 20, 0, NULL, 0) = 20
11:27:36.108083 recvfrom(3, "\234\5\0\0\20\0\2\0\1\0\0\0N\0\0\0\0\0\4\3\1\0\0\0\10\0\0\0\0\0\0\0"..., 8192, MSG_DONTWAIT, NULL, NULL) = 2912
11:27:36.110863 recvfrom(3, "\310\5\0\0\20\0\2\0\1\0\0\0N\0\0\0\0\0\1\0\3\0\0\0\2\20\0\0\0\0\0\0"..., 8192, MSG_DONTWAIT, NULL, NULL) = 3044
11:27:36.113566 recvfrom(3, "\24\0\0\0\3\0\2\0\1\0\0\0N\0\0\0\0\0\0\0", 8192, MSG_DONTWAIT, NULL, NULL) = 20
11:27:36.115621 sendto(3, "\24\0\0\0\26\0\1\3\2\0\0\0\0\0\0\0\0\0\0\0", 20, 0, NULL, 0) = 20
11:27:36.117941 recvfrom(3, "\24\0\0\0\3\0\2\0\2\0\0\0N\0\0\0\0\0\0\0", 8192, MSG_DONTWAIT, NULL, NULL) = 20
11:27:36.120707 close(3)                = 0
11:27:36.122324 getpid()                = 78
11:27:36.124015 openat(AT_FDCWD, "/dev/urandom", O_RDONLY|O_LARGEFILE) = -1 ENOENT (No such file or directory)
11:27:36.131604 mmap(NULL, 118784, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff9969f000
11:27:36.134045 mmap(NULL, 118784, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fff99682000
11:27:36.207700 munmap(0x7fff99682000, 118784) = 0
11:27:36.210555 writev(2, [{iov_base="", iov_len=0}, {iov_base="terminate called after throwing "..., iov_len=48}], 2terminate called after throwing an instance of ') = 48
11:27:36.213283 writev(2, [{iov_base="", iov_len=0}, {iov_base="std::runtime_error", iov_len=18}], 2std::runtime_error) = 18
11:27:36.215820 writev(2, [{iov_base="", iov_len=0}, {iov_base="'\n", iov_len=2}], 2') = 2
11:27:36.217960 writev(2, [{iov_base="", iov_len=0}, {iov_base="  what():  ", iov_len=11}], 2  what():  ) = 11
11:27:36.220151 writev(2, [{iov_base="", iov_len=0}, {iov_base="random_device::random_device(con"..., iov_len=48}], 2random_device::random_device(const std::string&)) = 48
11:27:36.222858 writev(2, [{iov_base="", iov_len=0}, {iov_base="\n", iov_len=1}], 2) = 1
11:27:36.225051 rt_sigprocmask(SIG_BLOCK, ~[RTMIN RT_1 RT_2], [], 8) = 0
11:27:36.227800 tkill(78, SIGABRT)      = 0
11:27:36.229517 rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
11:27:36.230940 --- SIGABRT {si_signo=SIGABRT, si_code=SI_TKILL, si_pid=78, si_uid=0} ---
11:27:36.235671 +++ killed by SIGABRT +++
```