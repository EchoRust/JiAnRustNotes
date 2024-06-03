# 配置国内镜像服务

在安装 `Rust` 的语言或者拉取依赖包的时候，经常会因为网络原因导致安装失败或者无法正常编译程序。这个时候可以使用国内镜像加速。

## `Rustup` 镜像

* `RUSTUP_DIST_SERVER` (默认：`https://static.rust-lang.org`) 设置下载与 `Rust` 相关的静态资源的根 `URL`。
* `RUSTUP_UPDATE_ROOT` (默认：`https://static.rust-lang.org/rustup`) 设置下载自我更新的 `URL`。

设置终端的环境变量为国内镜像站点，可以加速安装 `Rust` 和更新。

```shell
# 设置为字节镜像地址
$ export RUSTUP_DIST_SERVER="https://rsproxy.cn"
$ export RUSTUP_UPDATE_ROOT="https://rsproxy.cn/rustup"
```

重启终端或者执行`source ****`之类的命令即可生效。
```shell
# 执行Rust在线安装方式就可以使用了
$ curl --proto '=https' --tlsv1.2 -sSf https://rsproxy.cn/rustup-init.sh | sh
```

```shell
# 其他镜像地址
# 清华大学
export RUSTUP_DIST_SERVER="https://mirrors.tuna.tsinghua.edu.cn/rustup"
export RUSTUP_UPDATE_ROOT="https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup"
# 中国科学技术大学
export RUSTUP_DIST_SERVER="https://mirrors.ustc.edu.cn/rust-static"
export RUSTUP_UPDATE_ROOT="https://mirrors.ustc.edu.cn/rust-static/rustup"
# 上海交通大学
export RUSTUP_DIST_SERVER="https://mirror.sjtu.edu.cn/rust-static"
export RUSTUP_UPDATE_ROOT="https://mirror.sjtu.edu.cn/rust-static/rustup"
```

## `crates.io` 镜像

<span class="filename">文件名 `~/.cargo/config`</span>, 如果不存在可以新建这个文件.

`windows`在`C`盘当前用户目录下面的`.cargo`目录下. 例如:`C:\Users\echo\.cargo\config`.

* 字节跳动 `crates.io` 源

```toml
# Rust >= 1.68 版本建议使用 sparse-index，速度更快。
# 把下面第二行的 replace-with = 'rsproxy' 改成 replace-with = 'rsproxy-sparse'
[source.crates-io]
replace-with = 'rsproxy-sparse'
[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"
[source.rsproxy-sparse]
registry = "sparse+https://rsproxy.cn/index/"
[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"
[net]
git-fetch-with-cli = true
```

## 其他镜像地址

* 清华大学
```toml
[source.crates-io]
replace-with = 'mirror'

[source.mirror]
registry = "sparse+https://mirrors.tuna.tsinghua.edu.cn/crates.io-index/"
```
* 中国科学技术大学
```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```
* 上海交通大学
```toml
[source.crates-io]
replace-with = 'mirror'

[source.mirror]
registry = "sparse+https://mirrors.sjtug.sjtu.edu.cn/crates.io-index/"
```
