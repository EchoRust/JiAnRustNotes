# 设计优雅的库`API`

本文章由`Pascal Hertleif`创建于2016年07月21日，并发表在[https://deterministic.space/](https://deterministic.space/)网站上。

在选择编程语言时，具有用户友好的接口的库的存在是最重要的因素之一。这里有一些关于如何在Rust中编写带有漂亮api的库的技巧。(其中许多观点也适用于其他语言。)

[您还可以观看我在 2017 Rustfest 上关于此问题的演讲！](https://www.youtube.com/watch?v=0zOg8_B71gE)

更新2017-04-27：自从写了那篇文章后， Rust Libs 团队的 [@brson](https://github.com/brson)发布了一份非常全面的[Rust API 指南](https://rust-lang.github.io/api-guidelines/) 文档，其中包括我的建议以及更多内容。

更新 2020-06-01：这篇文章已经有好几年了！大多数模式在今天的 Rust 中仍然有效且被积极使用，但该语言也发生了很大变化，并启用了这里未讨论的新模式。我更新了一些语法和板条箱建议，但其他方面仍保留了 2016 年的帖子。

## 什么使得 API 变得优雅？

- 由于方法名称明显且不言自明，因此使用 API 的代码易于阅读。
- 可猜测的方法名称在使用 API 时也有帮助，因此不需要阅读文档。
- 所有内容都至少有一些文档和一个小代码示例。
- API 的用户需要编写一些样板代码才能使用它，因为
  - 方法接受多种有效的输入类型（其中转换是显而易见的），并且
  - 可以使用快捷方式来快速完成“常规”任务。
- 类型被巧妙地用来防止逻辑错误，但不会妨碍你太多。
- 返回的错误很有用，并且恐慌情况都有清晰的记录。

## 一致的名字

有一些 Rust RFC 描述了标准库的命名方案。您应该遵循它们，以使库的 API 对用户来说感觉熟悉。

- [RFC 199](https://github.com/rust-lang/rfcs/blob/1f5d3a9512ba08390a2226aa71a5fe9e277954fb/text/0199-ownership-variants.md)解释说您应该使用`mut`、`move`或`ref`作为后缀来根据方法参数的可变性来区分它们。
- [RFC 344](https://github.com/rust-lang/rfcs/blob/1f5d3a9512ba08390a2226aa71a5fe9e277954fb/text/0344-conventions-galore.md)定义了一些有趣的约定，例如
- 如何在方法名称中引用类型（例如，`&mut [T]` become `mut_slice` 和`*mut T` become `mut_ptr`），
- 如何调用返回迭代器的方法，
- 应该调用 getter 方法，field_name而应该调用 setter 方法set_field_name，
- 以及如何命名特征：“优先使用（及物动词）、名词，然后是形容词；避免使用语法后缀（如 able）”，而且“如果有一种方法是特征的主要功能，则考虑对特征本身使用相同的名称”。
- [RFC 430](https://github.com/rust-lang/rfcs/blob/1f5d3a9512ba08390a2226aa71a5fe9e277954fb/text/0430-finalizing-naming-conventions.md)描述了一些通用的大小写约定（对于类型级别的东西，对于值级别的东西，请参见tl;dr ）。 CamelCasesnake_case
- [RFC 445](https://github.com/rust-lang/rfcs/blob/1f5d3a9512ba08390a2226aa71a5fe9e277954fb/text/0445-extension-trait-conventions.md) `Ext`希望您为扩展特征添加后缀。


> 原文地址：[https://deterministic.space/elegant-apis-in-rust.html](https://deterministic.space/elegant-apis-in-rust.html)