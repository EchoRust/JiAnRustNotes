# Linux系统安装

## Ubuntu 系统安装`Rust`

### 自动安装

安装依赖

```bash
$ sudo apt-get update -y
$ sudo apt install -y curl vim build-essential
```

Rustup: Rust安装器和版本管理工具。

安装 Rust 的主要方式是通过 Rustup 这一工具，它既是一个 Rust 安装器又是一个版本管理工具。

```bash
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 手动安装

[下载对应系统的安装文件](https://forge.rust-lang.org/infra/other-installation-methods.html#standalone-installers)

```bash
$ cd ~/Downloads
$ tar -zxvf rust-1.68.2-x86_64-unknown-linux-gnu
$ cd rust-1.68.2-x86_64-unknown-linux-gnu
$ ./install.sh --help

Usage: ./install.sh [options]

Options:

    --uninstall             only uninstall from the installation prefix
    --destdir=[<none>]      set installation root
    --prefix=[/usr/local]   set installation prefix
    --without=[<none>]      comma-separated list of components to not install
    --components=[<none>]   comma-separated list of components to install
    --list-components       list available components
    --sysconfdir=[/etc]     install system configuration files
    --bindir=[/bin]         install binaries
    --libdir=[/lib]         install libraries
    --datadir=[/share]      install data
    --mandir=[/share/man]   install man pages in PATH
    --docdir=[\<default\>]  install documentation in PATH
    --disable-ldconfig      do not run ldconfig after installation (Linux only)
    --disable-verify        do not obsolete
    --verbose               run with verbose output

# 解释说明

    --uninstall             仅从安装的prefix卸载
    --destdir=[<none>]      设置安装根目录
    --prefix=[/usr/local]   设置安装前缀目录
    --without=[<none>]      逗号分隔的不安装组件列表
    --components=[<none>]   以逗号分隔的要安装的组件列表
    --list-components       列出所有的组件
    --sysconfdir=[/etc]     安装配置文件路径, 以prefix为前缀
    --bindir=[/bin]         安装二进制文件路径, 以prefix为前缀
    --libdir=[/lib]         安装库文件路径, 以prefix为前缀
    --datadir=[/share]      安装数据文件路径, 以prefix为前缀
    --mandir=[/share/man]   安装手册文件路径, 以prefix为前缀
    --docdir=[\<default\>]  安装文档文件路径, 默认为/usr/local/share/doc
    --disable-ldconfig      安装后不允许ldconfig(仅限Linux)
    --disable-verify        do not obsolete
    --verbose               输出详细运行信息

$ ./install.sh --destdir=~/rustlang
$ vim ~/.bashrc
# 在最后一行添加环境变量
export PATH="$PATH:/home/echo/rustlang/usr/local/bin"

$ source ~/.bashrc
$ cargo -V
cargo 1.68.2 (6feb7c9cf 2023-03-26)
```

##### 所有可用组件列表

```bash
$ ./install.sh --list-components

# Available components

* rustc
* rust-std-x86_64-unknown-linux-gnu
* rust-docs
* rust-demangler-preview
* cargo
* rustfmt-preview
* rls-preview
* rust-analyzer-preview
* llvm-tools-preview
* clippy-preview
* rust-analysis-x86_64-unknown-linux-gnu
```

### 问题

出现
```
error: linker `cc` not found
```

这是缺少 `compiler toolchain`, 通过命令安装`sudo apt install build-essential` 即可解决.