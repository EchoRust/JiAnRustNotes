# Summary

[技安Rust笔记](./index.md)
[前言](./foreword.md)

- [Rust入门](getting-started/index.md)
  - [安装Rust](getting-started/install.md)
    - [Linux系统安装](getting-started/install-linux.md)
    - [macOS系统安装](getting-started/install-mac.md)
    - [Windows系统安装](getting-started/install-windows.md)
    - [Rust更新与卸载](getting-started/update-remove.md)
  - [Rust编辑器](getting-started/editor.md)
  - [Hello, World!](getting-started/hello-world.md)
  - [学习和使用 cargo](getting-started/cargo.md)
  - [配置国内镜像服务](getting-started/mirrors.md)
<!-- - [Rust语言的基本语法](rust/base/index.md)
  - [语句和表达式](rust/base/statement.md) -->
- [宏(Macro)](macro/index.md)
  - [声明宏](macro/macro-rules.md)
- [Rust生态系统非官方指南(翻译)](crates/index.md)
<!-- - [SurrealDB - Rust开发的超现实数据库](surrealdb/index.md)
  - [SurrealDB简介](surrealdb/intro.md) -->
  <!-- - [SurrealDB的安装与配置](surrealdb/install.md)
  - [SurrealDB的查询语言](surrealdb/query.md)
  - [Rust项目使用SurrealDB](surrealdb/in-rust.md) -->
- [RustOS](os/index.md)
- [Linux 系统编程说明]()
  - [文件属性 stat,lstat,fstat系统调用](os/linux/stat.md)
  - [Ubuntu源码编译QEMU](os/linux/ubuntu-qemu.md)
- [ArceOS - 清华大学组件化操作系统]()
  - [ArceOS系统调用 - FastDDS移植](os/arceos-fastdds.md)
  - [FastDDS在ArceOS中的系统调用](os/arceos-fastdds-syscall.md)
  - [macOS开发StarryOS环境部署说明](os/starryos-macos.md)
  - [最新版本StarryOS环境搭建说明](os/new-starry-os.md)
  - [tokio在StarryOS中的系统调用分析](os/starryos-tokio.md)]
  - [gdb移植StarryOS](os/starryos-gdb.md)]
- [Dora - Rust开发的机器人系统]()
  <!-- - [Dora在 x86_64 架构中的 ArceOS 运行](dora/arceos-x86_64.md) -->
  - [Dora在 x86_64 架构中的系统调用分析](dora/linux.md)
  - [Dora在 aarch64 架构中的系统调用分析](dora/aarch64.md)
  - [Dora在 aarch64 架构中的 ArceOS 运行](dora/arceos.md)
  - [Dora在 RISCV64 架构中运行](dora/riscv64.md)
  - [从零开始在StarryOS中运行Dora]()
    - [环境搭建](dora/zero/env.md))
    - [运行 dora -h](dora/zero/dora-h.md)
    - [调试系统调用](dora/zero/debug-syscall.md)
  <!-- - [NetLink内核分析]()
    - [原理解析](os/netlink/principle.md) -->
<!-- - [优秀资源翻译](translate/index.md)
  - [设计优雅的库API](translate/elegant-api.md) -->
- [其它]()
  - [hdiutil工具](other/hdiutil.md)
  - [strace 工具](other/strace.md)
  - [kbuild工具](other/kbuild.md)