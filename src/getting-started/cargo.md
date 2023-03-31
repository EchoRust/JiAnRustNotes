# 学习和使用 cargo

`Rust` 是一门现代化语言，也为系统编程带来了现代化的开发工具。`Cargo` 便是 `Rust` 内置的依赖管理器和构建工具，它能轻松的增加、编译和管理依赖，并使依赖在 `Rust` 生态体系中保持一致的开发体验。

## 使用 `Cargo` 创建和运行项目

使用 `cargo` 创建上一节的 `hello_world` 项目。在终端中运行以下命令：

```console
$ cargo new new_hello_world
    Created binary (application) `new_hello_world` package
$ cd new_hello_world
```

先查看一下 `cargo` 生成的项目的结构：

```console
$ tree -a
.
├── .git
│   ├── HEAD
│   ├── config
│   ├── description
│   ├── hooks
│   │   └── README.sample
│   ├── info
│   │   └── exclude
│   ├── objects
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       └── tags
├── .gitignore
├── Cargo.toml
└── src
    └── main.rs

11 directories, 8 files
```

可以看到项目创建了一个 `Cargo.toml` 文件、一个 `src` 目录、一个 `.gitignore` 文件和 `git` 仓库。我们主要关注 `Cargo.toml` 文件和 `src` 目录。

查看 `Cargo.toml` 文件，它看起来如示例 1-1 所示：

<span class="filename">文件名 `Cargo.toml`</span>

```toml
[package]
name = "new_hello_world"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

<span class="caption">示例 1-1 `cargo new` 生成的项目 `Cargo.toml` 文件内容</span>

这个文件使用的是 [TOML](https://toml.io/) 文件格式。

`[package]` 片段主要配置当前项目的基本信息：项目名字、项目版本和 `Rust` 的版本 `edition`（许多人推荐使用 `版次` 来表述）。

`[dependencies]` 是我们添加依赖的地方。在 `Rust` 中，依赖包称为 `crates`。

现在查看 `src/main.rs` 文件

<span class="filename">文件名 `src/main.rs`</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<span class="caption">`cargo new` 生成的项目 `src/main.rs` 文件内容</span>

运行当前项目

```console
$ cargo run
   Compiling new_hello_world v0.1.0 (/Users/echo/new_hello_world)
    Finished dev [unoptimized + debuginfo] target(s) in 1.01s
     Running `target/debug/new_hello_world`
Hello, world!
```

该命令会创建一个可执行文件 `target/debug/new_hello_world` （`Windows` 系统上为 `target\debug\new_hello_world.exe`），并且执行当前的可执行文件。

最终在终端上会打印出 `Hello, world!`

默认 `cargo run` 执行的是 `debug` 模式的构建。如果想要编译发布模式构建可以使用如下命令：

```console
$ cargo build --release
   Compiling new_hello_world v0.1.0 (/Users/echo/new_hello_world)
    Finished release [optimized] target(s) in 0.23s
```

该命令会生成 `target/release/new_hello_world` 发布模式的可执行文件。`cargo build --release` 会启用一些优化来编译项目，可以使得编译后的可执行文件运行的更快，因此编译的时间会比较长。

在开发过程中，如果只是想快速检查代码是否通过编译，而不需要实际编译可执行文件，可以使用下面这个命令：

```console
$ cargo check
    Checking new_hello_world v0.1.0 (/Users/echo/new_hello_world)
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
```

该命令只会检查当前的代码能否通过编译，省略了生成可执行文件的步骤，因此速度很快。在实际开发过程中，可以经常使用 `cargo check` 命令来检查代码是否编写正确。

常用的 `cargo` 命令：

* 使用 `cargo new` 创建项目
* 使用 `cargo run` 构建并运行项目
* 使用 `cargo check` 快速检查代码是否可以通过编译
* 使用 `cargo build` 构建项目

从现在起，使用 `cargo` 管理我们的项目吧。

## `Cargo` 命令简介

终端中输入 `cargo` 命令查看

```console
$ cargo
Rust's package manager

Usage: cargo [+toolchain] [OPTIONS] [COMMAND]

Options:
  -V, --version             Print version info and exit
      --list                List installed commands
      --explain <CODE>      Run `rustc --explain CODE`
  -v, --verbose...          Use verbose output (-vv very verbose/build.rs output)
  -q, --quiet               Do not print cargo log messages
      --color <WHEN>        Coloring: auto, always, never
      --frozen              Require Cargo.lock and cache are up to date
      --locked              Require Cargo.lock is up to date
      --offline             Run without accessing the network
      --config <KEY=VALUE>  Override a configuration value
  -Z <FLAG>                 Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
  -h, --help                Print help

Some common cargo commands are (see all commands with --list):
    build, b    Compile the current package
    check, c    Analyze the current package and report errors, but don't build object files
    clean       Remove the target directory
    doc, d      Build this package's and its dependencies' documentation
    new         Create a new cargo package
    init        Create a new cargo package in an existing directory
    add         Add dependencies to a manifest file
    remove      Remove dependencies from a manifest file
    run, r      Run a binary or example of the local package
    test, t     Run the tests
    bench       Run the benchmarks
    update      Update dependencies listed in Cargo.lock
    search      Search registry for crates
    publish     Package and upload this package to the registry
    install     Install a Rust binary. Default location is $HOME/.cargo/bin
    uninstall   Uninstall a Rust binary

See 'cargo help <command>' for more information on a specific command.
```

主要命令详解

```console
build, b    构建当前包
check, c    分析当前包并报告错误，但不构建目标文件
clean       删除构建的目录
doc, d      构建当前包及其依赖项目的文档（会创建 `target/doc` 目录，使用浏览器打开可以查看详细的文档）
new         创建一个新的包
init        在现有目录中创建一个新的包
add         添加依赖项到当前项目中
remove      从当前项目中删除依赖项
run, r      构建并运行项目
test, t     运行测试
bench       运行基准测试
update      更新在 Cargo.lock 注册的依赖项版本
search      搜索 crates
publish     打包并上传当前包 （crates.io）
install     安装 Rust 二进制文件，默认目录在 $HOME/.cargo/bin
uninstall   卸载 Rust 二进制文件
```