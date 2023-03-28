# 声明宏

### 分类符

* [`item`](#item) 只匹配`Rust`的`item`的定义, 不会匹配指向`item`的标识符.
* [`block`](#block) 匹配`blok`表达式.
* [`stmt`](#stmt) 匹配语句. 除非`item`语句要求结尾有分号, 否则不会匹配语句最后的分号.
* [`pat_param`](#pat_param) 允许`｜`跟在后面, `2021 editing`开始.
* [`pat`](#pat) 匹配任何形式的模式.
* [`expr`](#expr) 匹配任何形式的表达式.
* [`ty`](#ty) 匹配任何形式的类型表达式.
* [`ident`](#ident) 匹配任何形式的标识符或者关键字.
* [`path`](#path) 匹配类型中的路径
* [`tt`](#tt) 匹配标记树`TokenTree`.
* [`meta`](#meta) 匹配属性. 准确的说是属性里面的内容.
* [`lifetime`](#lifetime) 匹配生命周期注解或者标签. 和`ident`很像, 但是`lifetime`会匹配到前缀`'`.
* [`vis`](#vis) 匹配可能为空的内容.
* [`literal`](#literal) 匹配字面表达式.

### `item`

`item`分类器只匹配`Rust`的`item`的定义, 不匹配指向`item`的标识符.

```rust
macro_rules! items {
    ($($item: item)*) => {};
}
items!{
    struct Foo;
    enum Bar {
        Baz
    }
    impl Foo {}
}
fn main() {}
```

`item`是在编译器完全确定的, 通常在程序执行期间保持固定, 并且可以驻留在支付存储器中.

* `modules`
* `extern crate`
* `use`
* `function`
* `type`
* `struce`
* `enum`
* `union`
* `constant`
* `static`
* `trait`
* `impl`
* `extern`

### `block`

`block`匹配`bolck表达式`

块(block)由`{`开始, 接着是一些语句, 最后以`}`结束. 块的类型要么是最后的值表达式类型, 要么是单元元组`()`类型.

```rust
macro_rules! blocks {
    ($($block: block)*) => {};
}
blocks! {
    {}
    { let abc; }
    {8}
}
fn main() {}
```

### `stmt`

`stmt`匹配语句. 除非`item`语句要求结尾有分号, 否则不会匹配语句最后的分号.

`item`语句要求结尾有分号: 例如 单元结构体是一个例子, 定义中必须要带上结尾的分号.

```rust
macro_rules! statements {
    ($($stmt: stmt)*) => {$($stmt)*};
}
fn main() {
    statements!{
        struct Foo;
        fn foo() {}
        let bar = 8
        let bar = 8
        ;
        8;
        ;
        if true {} else {}
        {}
        ()
    }
}
```

### `pat_param`

从 2021 edition 起，`or-patterns`模式开始应用，这让`pat`分类符不再允许跟随`|`。

为了避免这个问题或者说恢复旧的 pat 分类符行为，你可以使用 `pat_param` 片段，它允许 `|` 跟在它后面，因为 `pat_param` 不允许 `top level` 或 `or-patterns`。

```rust
macro_rules! patterns {
    (pat: $pat: pat) => {
        println!("pat: {}", stringify!($pat));
    };
    (pat_param: $($pat: pat_param)|*) => {
        $( println!("pat_param: {}", stringify!($pat)); )+
    };
}
fn main() {
    patterns!{
        pat: 0 | 1 | 2 | 3
    }
    patterns!{
        pat_param: 0 | 1 | 2 | 3
    }
}
```
```rust
macro_rules! patterns {
    ($($($pat: pat_param)|+)*) => {};
}
patterns!{
    0 | 1 | 2 | 3
}
fn main() {
}
```

### `pat`

`pat` 分类符用于匹配任何形式的模式 (`pattern`)，包括 `2021 edition` 开始的 `or-patterns`。

```rust
macro_rules! patterns {
    ($($pat:pat)*) => ();
}

patterns! {
    "literal"
    _
    0..5
    ref mut PatternsAreNice
    0 | 1 | 2 | 3
}
fn main() {}
```

### `expr`

`expr`匹配任何形式的表达式.

```rust
macro_rules! experssions {
    ($($expr: expr)*) => {};
}
experssions! {
    "abc"
    func()
    func.await
    break 'foo
}
fn main() {}
```

### `ty`

`ty`匹配任何形式的类型表达式, 类型表达式是在`Rust`中表示类型的语法.

```rust
macro_rules! types {
    ($($type: ty)*) => {};
}
types! {
    foo::bar
    bool
    [u8]
    impl IntoIterator<Item = u32>
}
fn main() {}
```

### `ident`

`ident`匹配任何形式的标识符或者关键字.

```rust
macro_rules! idents {
    ($($ident: ident)*) => {};
}
idents! {
    //
    foo
    async
    o_____o
    ___o___
}
fn main() {}
```

### `path`

`path`匹配类型中的路径.

```rust
macro_rules! paths {
    ($($path: path)*) => {};
}
paths! {
    APath
    ::A::B::C::D
    E::<fff>::G
    FnMut(u32) -> ()
}
fn main() {}
```

### `tt`

`tt` 分类符用于匹配标记树 (`TokenTree`)。`tt` 分类符是最有作用的分类符之一，因为它能匹配几乎所有东西， 而且能够让你在使用宏之后检查 (inspect) 匹配的内容。

### `meta`

`meta`匹配属性, 准确的说是属性里面的内容. 通常会在`#[$meta: meta]` 或 `#![$meta: meta]` 模式匹配中看到这个.

```rust
macro_rules! metas {
    ($($meta: meta)*) => {};
}
metas! {
    ASimplePath
    super::man
    path = "home"
    foo(bar)
}
fn main() {}
```

### `lifetime`

`lifetime`匹配生命周期注解或者标签. 和`ident`很像, 但是`lifetime`会匹配到前缀`'`.

```rust
macro_rules! lifetimes {
    ($($lifetime: lifetime)*) => { $( println!("lifetime: {}", stringify!($lifetime)); )*};
}
fn main() {
    lifetimes! {
        'static
        'a
        '_
    }
}
```

### `vis`

`vis`匹配可能为空的内容. 实际上支持例子里的几种方式，因为这里的 `visibility` 指的是可见性，与私有性相对。而涉及这方面的内容只有与 `pub` 的关键字。所以，`vis` 在关心匹配输入的内容是公有还是私有时有用。

```rust
macro_rules! visibilities {
    ($($vis: vis,)*) => {};
}
visibilities! (
    pub,
    ,
);
fn main() {}
```

### `literal`

`literal`匹配字面表达式

```rust
macro_rules! literals {
    ($($literal: literal)*) => {};
}
literals! {
    -1
    "hello world"
    2.3
    b'b'
    'c'
    true
}
fn main() {}
```