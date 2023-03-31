# Hello, World!

我们现在开始编写第一个 `Rust` 程序.

## 创建项目目录

本书将在用户目录中创建 `rust-projects` 目录, 并将所有的项目存放在这里.

对于 Linux、macOS 和 Windows PowerShell，输入：
```bash
$ mkdir ~/rust-projects
$ cd ~/rust-projects
$ mkdir hello_world
$ cd hello_world
```

接下来使用 `VS Code` 打开当前文件夹
```bash
$ code .
```

## 编写并运行 `Rust` 程序

在 `VS Code` 中新建一个源文件, 命名为 `main.rs`. `Rust` 源文件总是以 `.rs` 扩展名结尾.

打开刚才新建的 `main.rs` 文件, 输入下面代码.

文件名: `main.rs`
```rust
fn main() {
    println!("Hello, world!");
}
```

保存文件, 在终端中进入当前目录. 在 `Linux` 和 `macOS` 系统上, 输入如下命令, 编译并运行文件:
```bash
$ cd ~/rust-projects/hello_world
$ rustc main.rs
$ ./main
Hello, world!
```

在 `Windows` 系统上, 输入命令 `.\main.exe`.
```bash
> cd C:\Users\Echo\rust-projects/hello_world
> rustc main.rs
> .\main.exe
Hello, world!
```

如果没有出错误，终端应该可以打印字符串 `Hello, world!`。如果没有，需要检查一下 `Rust` 是否已安装正确。

如果 `Hello, world!` 出现了，恭喜你！你已经正式编写了一个 `Rust` 程序。现在你成为一名 `Rustacean` 了，欢迎加入！