# 优雅的API

Elegant Library APIs in Rust
Created Jul 21, 2016 by Pascal Hertleif and published in Pascal's Scribbles. Labeled as rust, writing and incomplete.

The existence of libraries with nice, user-friendly interfaces is one of the most important factors when choosing a programming language. Here are some tips on how to write libraries with nice APIs in Rust. (Many of the points also apply to other languages.)

You can also watch my talk at Rustfest 2017 about this!

Update 2017-04-27: Since writing that post, @brson of the Rust Libs Team has published a pretty comprehensive Rust API Guidelines document that includes my advice here and a lot more.

Update 2020-06-01: This post is quite a few years old by now! Most of the patterns are still valid and actively used in Rust today, but the language has also evolved quite a bit and enabled new patterns that are not discussed here. I’ve updated some of syntax and crate recommendations but otherwise kept the post as it was in 2016.

Contents
What makes an API elegant?
Techniques
Consistent names
More method name conventions
Doc tests
Don’t “stringly type” your API
A module full of constants
Actually parsing a string with FromStr
Error handling
Public type aliases
Use conversion traits
Cow
Example: std::convert::Into
Into<Option<_>>
A note on possibly long compile times
Laziness
Use Iterators
Iterators as parameters
Returning/implementing iterators
Iterator-like traits
Take closures
Lazy tricks
Convenience traits
Custom traits for input parameters
Example: str::find
Extension traits
Decorating results
Extending traits
Builder pattern
Example: std::fs::OpenOptions
Session types
Use lifetimes well
Put finalizer code in drop
Case Studies
Other design patterns
What makes an API elegant?
Code using the API is easily readable thanks to obvious, self-explanatory method names.
Guessable method names also help when using the API, so there is less need to read documentation.
Everything has at least some documentation and a small code example.
Users of the API need to write little boilerplate code to use it, as
methods accept a wide range of valid input types (where conversions are obvious), and
shortcuts to get the ‘usual’ stuff done quickly are available.
Types are cleverly used to prevent logic errors, but don’t get in your way too much.
Returned errors are useful, and panic cases are clearly documented.
Techniques
Consistent names
There are a few Rust RFCs that describe the naming scheme of the standard library. You should follow them to make your library’s API feel familiar for users.

RFC 199 explains that you should use mut, move, or ref as suffixes to differentiate methods based on the mutability of their parameters.
RFC 344 defines some interesting conventions, like
how to refer to types in method names (e.g., &mut [T] becomes mut_slice, and *mut T becomes mut_ptr),
how to call methods that return iterators,
that getters methods should be called field_name while setter methods should be set_field_name,
and how to name traits: “Prefer (transitive) verbs, nouns, and then adjectives; avoid grammatical suffixes (like able)”, but also “if there is a single method that is the dominant functionality of the trait, consider using the same name for the trait itself”.
RFC 430 describes some general casing conventions (tl;dr CamelCase for type-level stuff, snake_case for value-level stuff).
RFC 445 wants you to add an Ext suffix to extension traits.
More method name conventions
In addition to what RFC 199 and RFC 344 (see above) define, there are a few more conventions around what method names to choose, which seem to not be represented in RFCs (yet). These are mostly documented in the old Rust style guidelines and in @llogiq’s post Rustic Bits as well as clippy’s wrong_self_convention lint. Here’s a summary:

Method name	Parameters	Notes	Examples
new	no self, usually ≥ 11	Constructor, also cf. Default	Box::new, std::net::Ipv4Addr::new
with_...	no self, ≥ 1	Alternative constructors	Vec::with_capacity, regex::Regex::with_size_limit
from_...	1	cf. conversion traits	String::from_utf8_lossy
as_...	&self	Free conversion, gives a view into data	str::as_bytes, uuid::Uuid::as_bytes
to_...	&self	Expensive conversion	str::to_string, std::path::Path::to_str
into_...	self (consumes)	Potentially expensive conversion, cf. conversion traits	std::fs::File::into_raw_fd
is_...	&self (or none)	Should probably return a bool	slice::is_empty, Result::is_ok, std::path::Path::is_file
has_...	&self (or none)	Should probably return a bool	regex_syntax::Expr::has_bytes
Doc tests
Write documentation with example code showing how to use your API and get automatic tests for free – Two birds, one stone. You can read more about in the documentation chapter of the official book.

/// Manipulate a number by magic
///
/// # Examples
///
/// ```rust
/// assert_eq!(min( 0,   14),    0);
/// assert_eq!(min( 0, -127), -127);
/// assert_eq!(min(42,  666),   42);
/// ```
fn min(lhs: i32, rhs: i32) -> i32 {
	if lhs < rhs { lhs } else { rhs }
}
To enforce that every public API item is documented, use #![deny(missing_docs)]. You might also be interested in my post suggesting conventions for formatting Rust documentation.

Don’t “stringly type” your API
Coming from dynamically typed languages, you might be tempted to use strings with specific values in various places.

For example: You want a function to print some input text in a color, so you use a parameter color of type &str. That also means you expect your users to write one of the names from a limited number of color names (like ["red", "green", "blue", "light golden rod yellow"]).

This is not what you should do in Rust! If you know all possible variants beforehand, use an enum. This way, you don’t need to parse/pattern match the string – and deal with possible errors – but can be sure that a user of your API can only ever supply valid inputs2.

enum Color { Red, Green, Blue, LightGoldenRodYellow }

fn color_me(input: &str, color: Color) { /* ... */ }

fn main() {
    color_me("surprised", Color::Blue);
}
A module full of constants
Alternatively, if you have a more complex value you want to express you can define a new struct and then define a bunch of constants with common values. If you put the constants into a public module, your users can access them using similar syntax as when using an enum variant.

pub mod output_options {
    pub struct OutputOptions { /* ... */ }
    
    impl OutputOptions { fn new(/* ... */) -> OutputOptions { /* ... */ } }
    
    pub const DEFAULT: OutputOptions = OutputOptions { /* ... */ };
    pub const SLIM: OutputOptions = OutputOptions { /* ... */ };
    pub const PRETTY: OutputOptions = OutputOptions { /* ... */ };
}

fn output(f: &Foo, opts: OutputOptions) { /* ... */ }

fn main() {
    let foo = Foo::new();
    
    output(foo, output_options::PRETTY);
}
Actually parsing a string with FromStr
There may still be cases where users of your API actually have strings, e.g. from reading environment variables or by taking their user input – i.e., they didn’t write (static) strings themselves in their code to give to your API (which is what we try to prevent). In those cases it makes sense to have a look at what the FromStr trait can give you, which abstracts over the concept of parsing a string into a Rust data type.

If all you want to do is map a string with an enum variant name to the right enum variant, you can adapt this macro (from this tweet; there might also be a crate for that).

Depending on your API, you could also decide to have your users deal with parsing the string. If you supply the right types and implementations, it shouldn’t be difficult (but needs to be documented nonetheless).

// Option A: You do the parsing
fn output_a(f: &Foo, color: &str) -> Result<Bar, ParseError> {
    // This shadows the `options` name with the parsed type
    let color: Color = options.parse()?;

    f.to_bar(&color)
}

// Option B: User does the parsing
fn output_b(f: &Foo, color: &Color) -> Bar {
    f.to_bar(color)
}

fn main() {
    let foo = Foo::new();

    // Option A: You do the parsing, user needs to deal with API error
    output_a(foo, "Green").expect("Error :(");

    // Option B: User has correct type, no need to deal with error here
    output_b(foo, Color::Green);

    // Option B: User has string, needs to parse and deal with parse error
    output_b(foo, "Green".parse().except("Parse error!"));
}
Error handling
The official book has an awesome chapter on error handling.

There are a few crates to reduce the boilerplate needed for good error handling, e.g., anyhow (dynamic error type with methods for annotating and chaining errors), and thiserror (makes creating custom error types easy).

Public type aliases
If your internal code uses generic types with the same type parameters over and over again, it makes sense to use a type alias. If you also expose those types to your users, you should expose (and document) the type alias as well.

A common case where this is used is Result<T, E> types, where the error case (E) is fixed. For example, std::io::Result<T> is an alias for Result<T, std::io::Error>, std::fmt::Result is an alias for Result<(), std::fmt::Error>, and serde_json::error::Result<T> is an alias for Result<T, serde_json::error::Error>.

Use conversion traits
It’s good practice to never have &String or &Vec<T> as input parameters and instead use &str and &[T] as they allow more types to be passed in. (Basically, everything that derefs to a (string) slice).

We can apply the same idea at a more abstract level: Instead of using concrete types for input parameters, try to use generics with precise constraints. The downside of this is that the documentation will be less readable as it will be full of generics with complex constraints!

std::convert has some goodies for that:

AsMut: A cheap, mutable reference-to-mutable reference conversion.
AsRef: A cheap, reference-to-reference conversion.
From: Construct Self via a conversion.
Into: A conversion that consumes self, which may or may not be expensive.
TryFrom: Attempt to construct Self via a conversion. (Unstable as of Rust 1.10)
TryInto: An attempted conversion that consumes self, which may or may not be expensive. (Unstable as of Rust 1.10)
You might also enjoy this article about Convenient and idiomatic conversions in Rust.

Cow
If you are dealing with a lot of things that may or may not need to be allocated, you should also look into Cow<'a, B> which allows you to abstract over borrowed and owned data.

Example: std::convert::Into
fn foo(p: PathBuf)	fn foo<P: Into<PathBuf>>(p: P)
Users needs to convert their data to a PathBuf	Library can call .into() for them
User does allocation	Less obvious: Library might need to do allocation
User needs to care about what a PathBuf is and how to get one	User can just give a String or an OsString or a PathBuf and be happy
Into<Option<_>>
This pull request, which landed in Rust 1.12, adds an impl<T> From<T> for Option<T>. While only a few lines long this allows you to write APIs that can be called without typing Some(…) all the time.

Before:

// Easy for API author, easy to read documentation
fn foo(lorem: &str, ipsum: Option<i32>, dolor: Option<i32>, sit: Option<i32>) {
    println!("{}", lorem);
}

fn main() {
    foo("bar", None, None, None);               // That looks weird
    foo("bar", Some(42), None, None);           // Okay
    foo("bar", Some(42), Some(1337), Some(-1)); // Halp! Too… much… Some…!
}
After:

// A bit more typing for the API author.
// (Sadly, the parameters need to be specified individually – or Rust would
// infer the concrete type from the first parameter given. This reads not as
// nicely, and documentation might not look as pretty as before.)
fn foo<I, D, S>(lorem: &str, ipsum: I, dolor: D, sit: S) where
    I: Into<Option<i32>>,
    D: Into<Option<i32>>,
    S: Into<Option<i32>>,
{
    println!("{}", lorem);
}

fn main() {
    foo("bar", None, None, None); // Still weird
    foo("bar", 42, None, None);   // Okay
    foo("bar", 42, 1337, -1);     // Wow, that's nice! Gimme more APIs like this!
}
A note on possibly long compile times
If you have:

a lot of type parameters (e.g. for the conversion traits)
on a complex/large function
which is used a lot
then rustc will need to compile a lot of permutations of this function (it monomorphizes generic functions), which will lead to long compile times.

bluss mentioned on Reddit that you can use “de-generization” to circumvent this issue: Your (public) generic function just calls another, (private) non-generic function, which will only be compiled once.

The examples bluss gave was the implementation of std::fs::OpenOptions::open (source from Rust 1.12) and this pull request on the image crate, which changed its open function to this:

pub fn open<P>(path: P) -> ImageResult<DynamicImage> where P: AsRef<Path> {
    // thin wrapper function to strip generics before calling open_impl
    open_impl(path.as_ref())
}
Laziness
While Rust does not have ‘laziness’ in the sense of lazy evaluation of expressions like Haskell implements it, there are several techniques you can use to elegantly omit doing unnecessary computations and allocations.

Use Iterators
One of the most amazing constructs in the standard library is Iterator, a trait that allows generator-like iteration of values where you only need to implement a next method3. Rust’s iterators are lazy in that you explicitly need to call a consumer to start iterating through values. Just writing "hello".chars().filter(char::is_whitespace) won’t do anything until you call something like .collect::<String>() on it.

Iterators as parameters
Using iterators as inputs may make your API harder to read (taking a T: Iterator<Item=Thingy> vs. &[Thingy]), but allows users to skip allocations.

Actually, though, you might not want to take a generic Iterator: Use IntoIterator instead. This way, can give you anything that you can easily turn into an iterator yourself by calling .into_iter() on it. It’s also quite easy to determine which types implement IntoIterator—as the documentation says:

One benefit of implementing IntoIterator is that your type will work with Rust’s for loop syntax.

That is, everything a user can use in a for loop, they can also give to your function.

Returning/implementing iterators
If you want to return something your users can use as an iterator, the best practice is to define a new type that implements Iterator. This may become easier once impl Trait is stabilized (see the tracking issue). You can find a bit more information about this in the futures tutorial (as returning a Future and an Iterator has similar characteristics).

Iterator-like traits
There are a few libraries that implement traits like Iterator, e.g.:

futures::Stream: As written in the futures tutorial, where Iterator::next returns Option<Self::Item>, Stream::poll returns an async result of Option<Self::Item> (or an error).
Take closures
If a potentially expensive value (let’s say of type Value) is not used in all branches in your control flow, consider taking a closure that returns that value (Fn() -> Value).

If you are designing a trait, you can also have two methods that do the same thing, but where one takes a value and the other a closure that computes the value. A real-life example of this pattern is in Result with unwrap_or and unwrap_or_else:

let res: Result<i32, &str> = Err("oh noes");
res.unwrap_or(42); // just returns `42`

let res: Result<i32, &str> = Err("oh noes");
res.unwrap_or_else(|msg| msg.len() as i32); // will call the closure
Lazy tricks
Letting Deref do all the work: Wrapper type with an implementation of Deref that contains the logic to actually compute a value. The crate lazy implements a macro to do that for you (it requires unstable features, though).
Convenience traits
Here are some traits you should try implement to make using your types easier/more consistent for your users:

Implement or derive the ‘usual’ traits like Debug, Hash, PartialEq, PartialOrd, Eq, Ord
Implement or derive Default instead of writing a new method without arguments
If you find yourself implementing a method on a type to return some of the type’s data as an Iterator, you should also consider implementing IntoIterator on that type. (This only works when there is only one obvious way to iterate over your type’s data. Also see section on iterators above.)
If your custom data type can be thought of in a similar fashion as a primitive data type T from std, consider implementing Deref<Target=T>. But please don’t overdo this – Deref is not meant to emulate inheritance!
Instead of writing a constructor method that takes a string and creates a new instance of your data type, implement FromStr.
Custom traits for input parameters
The Rust way to implement a kind of “function overloading” is by using a generic trait T for one input parameter and implement T for all types the function should accept.

Example: str::find
str::find<P: Pattern>(p: P) accepts a Pattern which is implemented for char, str, FnMut(char) -> bool, etc.

"Lorem ipsum".find('L');
"Lorem ipsum".find("ipsum");
"Lorem ipsum".find(char::is_whitespace);
Extension traits
It’s a good practice to use types and traits defined in the standard library, as those are known by many Rust programmers, well-tested, and nicely documented. And while Rust’s standard library tends to offer types with semantic meaning4, the methods implemented on these types might not be enough for your API. Luckily, Rust’s “orphan rules” allow you implement a trait for a (generic) type if at least one of them is defined in the current crate.

Decorating results
As Florian writes in “Decorating Results”, you can use this to write and implement traits to supply your own methods to built-in types like Result. For example:

pub trait GrandResultExt {
    fn party(self) -> Self;
}

impl GrandResultExt for Result<String, Box<Error>> {
    fn party(self) -> Result<String, Box<Error>> {
        if self.is_ok() {
          println!("Wooohoo! 🎉");
        }
        self
    }
}

// User's code
fn main() {
    let fortune = library_function()
        .method_returning_result()
        .party()
        .unwrap_or("Out of luck.".to_string());
}
Florian’s real-life code in lazers uses the same pattern to decorate the BoxFuture (from the futures crate) to make the code more readable (abbreviated):

let my_database = client
    .find_database("might_not_exist")
    .or_create();
Extending traits
So far, we’ve extended the methods available on a type by defining and implementing our own trait. You can also define traits that extend other traits (trait MyTrait: BufRead + Debug {}). The most prominent example for this is the itertools crate, which adds a long list of methods to std’s Iterators.

FYI: RFC 445 wants you to add an Ext suffix to extension traits.

Builder pattern
You can make it easier to make complex API calls by chaining several smaller methods together. This works nicely with session types (see below). The derive_builder crate can be used to automatically generate (simpler) builders for custom structs.

Example: std::fs::OpenOptions
use std::fs::OpenOptions;
let file = OpenOptions::new().read(true).write(true).open("foo.txt");
Session types
You can encode a state machine in the type system.

Each state is a different type.
Each state type implements different methods.
Some methods consume a state type (by taking ownership of it) and return a different state type.
This works really well in Rust as your methods can move your data into a new type and you can no longer access the old state afterwards.

Here’s an arbitrary example about mailing a package (the type annotations are not necessary and are only added for clarity here):

let package: OpenPackage = Package::new();
let package: OpenPackage = package.insert([stuff, padding, padding]);

let package: ClosedPackage = package.seal_up();

// let package: OpenPackage = package.insert([more_stuff]);
//~^ ERROR: No method named `insert` on `ClosedPackage`

let package: DeliveryTracking = package.send(address, postage);
A good real-life example was given by /u/ssokolow in this thread on /r/rust:

Hyper uses this to ensure, at compile time, that it’s impossible to get into situations like the “tried to set HTTP headers after request/response body has begun” that we see periodically on PHP sites. (The compiler can catch that because there is no “set header” method on a connection in that state and the invalidating of stale references allows it to be certain that only the correct state is being referenced.)

The hyper::server docs go into a bit of detail on how this is implemented. Another interesting idea can be found in the lazers-replicator crate: It uses std::convert::From to transition between states.

More information:

The article “Beyond Memory Safety With Types” describes how this technique can be used to implement a nice and type safe interface for the IMAP protocol.
The paper “Session types for Rust” (PDF) by Thomas Bracht Laumann Jespersen, Philip Munksgaard, and Ken Friis Larsen (2015). DOI.
Andrew Hobden’s post “Pretty State Machine Patterns in Rust” shows several ways how one can implement state machines in Rust’s type system: Using one enum for all states, explicit structs, a base struct generic over state structs, and transitions using Into.
Use lifetimes well
Specifying type and trait constraints on your API is essential to designing an API in a statically typed language, and, as written above, to help your users prevent logic errors. Rust’s type system can also encode another dimension: You can also describe the lifetimes of your data (and write constraints on lifetimes).

This can allow you (as a developer) to be more relaxed about giving out borrowed resources (instead of more computationally expensive owned data). Using references to data where possible is definitely a good practice in Rust, as high performance and “zero allocation” libraries are one of the languages selling points.

You should try to write good documentation on this, though, as understanding lifetimes and dealing with references can present a challenge to users of your library, especially when they are new to Rust.

For some reason (probably brevity), a lot of lifetimes are called 'a, 'b, or something similarly meaningless. If you know the resource for whose lifetime your references are valid, you can probably find a better name, though. For examples, if you read a file into memory and are working with references to that memory, call those lifetimes 'file. Or if you are processing a TCP request and are parsing its data, you can call its lifetime 'req.

Put finalizer code in drop
Rust’s ownership rules work for more than just memory: If your data type represents an external resource (e.g., a TCP connection), you can use the Drop trait to close/deallocate/clean up the resource when it goes out of scope. You can use this the same way as you would use finalizers (or try … catch … finally) in other languages.

Real-life examples of this are:

The reference count types Rc and Arc use Drop to decrease their reference count (and deallocate the inner data if the count hits zero).
MutexGuard uses Drop to release its lock on a Mutex.
The diesel crate implements Drop to close database connections (e.g. in SQLite).
Case Studies
Possible Rust libraries that use some nice tricks in their APIs:

hyper: session types (see above)
diesel: encodes SQL queries as types, uses traits with complex associated types
futures: very abstract and well documented crate
Other design patterns
What I tried to cover here are design patterns for interfaces, i.e. APIs exposed to the user. While I believe that some of these patterns are only applicable to writing libraries, many also apply to writing generic application code.

You can find more information on this topic in the Rust Design Patterns repository.

If you can construct your type without any parameters, you should implement Default on it, and use that instead of new. An exception to this is new on “container” types, like Vec or HashMap, where it makes sense to initialize an empty container. ↩

There is a slogan of “making illegal states unrepresentable” in other strongly typed languages. While I first heard this when talking to people about Haskell, it is also the title of this article by F# for fun and profit, and this talk by Richard Feldman presented at elm-conf 2016. ↩

In that regard, Rust’s Iterators are very similar to the Iterator interface in Java or the Iteration protocol in Python (as well as many others). ↩

For examples, std has an Result type (with Ok and Err variants) which should be used to handle errors, instead of an Either type (with Left and Right variants) which does not imply that meaning. ↩

Thanks for reading. Comment on this post! Revision history.

This post has been discussed on Twitter (1), Twitter (2) and /r/rust.

Back to index.