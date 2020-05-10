---
layout: post
title: "Why I started learning Rust and impressions first week"
date: 2020-05-10
excerpt: "In Rust, variables are immutable by default, It doesn't have garbage collection and has first-class support for WebAssembly."
tags: [rust, rustlang, webassembly, wasm, immutability]
published: true
---
I decided to learn one new programming language every year since 2019. I don't plan to become an expert in a particular language. But it introduces a lot of new patterns and broadens our perspective. In 2019, my choice was Flutter. I was advocating about Flutter to our team and did a few side projects (which are never going to see the light of the day). Flutter was a great choice for both joys of learning and the potential of building something for my own.

For 2020, I have been thinking about learning either Golang or Rust that revered by devs working on it. After some days of thought, I decided to go with Rust because of a few things,

1. **In Rust, variables are immutable by default.** Having worked with JS and PHP in complex applications, I can say that this is a great feature. My understanding of the immutable state and unidirectional data-flow is low. I faced a lot of issues due to the misuse of immutable state. Rust makes it hard to make those mistakes.

2. **Rust doesn't have garbage collection**. I never worked with low-level languages that need developers to manage memory. But I know how hard it can be, I spent many hours on V8 blog reading <a href="https://v8.dev/blog/orinoco-parallel-scavenger" target="_blank">how the garbage collector for JS works</a> in chrome. It is really interesting, how a language works, without dealing with memory management and without being GC as well.

3. **Rust has first-class support for WebAssembly.** I first saw the real strength of webAssembly in 
<a href="http://figma.com/" target="_blank">Figma.</a> Their site is one of the most complicated web application and it worked like a charm. This is what web apps have been struggling for decades. I was thinking to use WASM into real projects. But I'm not ready for implementing and supporting it in production yet. In real projects, deadlines and stability matter the most. Learning Rust will help me work with WASM and then start using them in production apps.

Once I decided to learn Rust I didn't start it until recently due to my procrastinating nature. In April, I was scrolling through twitter and decided to start with <a href="https://www.100daysofcode.com/" target="_blank">#100daysofcode</a>. I was not a believer in the social part of learning but it helps to connect with relevant people. Also, It holds you accountable when you commit to it publicly. So give it a try, even if you're an experienced programmer.

## Setup

Rust is easy to set up by using the rustup. run the below command, and let it take care of installing and configuring your environment.

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

After installing, I tried to build a hello world application and got served with this.

```
error: linker cc not found.
```

This means that rust is missing some dependency that it needs for compiling code. I was not sure about why the rustup script missed this dependency. So after some googling, I solved it by installing the build-essentials in Ubuntu.

```bash
sudo apt install build-essentials
```

I am using Webstorm with Rust plugin and it works well enough for me.

## Learning Materials & tools,

Rust has one of the excellent official documentation and tools in my experience. It took decades for PHP to build composer and Javascript to create NPM. Rust has a command-line tool called `rustc` which will let you compile and run Rust programs. The best part is you don't have to use it when you have `cargo.` Cargo is the official package manager for Rust. The packages built by the rust community are  `crates.` You use cargo to orchestrate the crates you want in your codebase.

If you want to check for any crates, head to <a href="https://crates.io/" target="_blank">creates.io</a>, and search with the keyword. Once found, add it to your cargo.toml file.  Every time you run `cargo build` it will install the dependencies automatically (There is no separate install command AFAIK).

## Dependency Management

To manage the dependencies Rust uses two files. cargo.toml, and cargo.lock which are like npm's package.json and package-lock.json. The cargo.toml will keep the list of dependencies required for your project. It is maintained by the developer.

The cargo.lock is a detailed record of your dependencies and their dependencies. It records the exact versions installed in your machine. This is useful to reproduce the exact version of the dependency tree on other machines. You should not edit the cargo.lock file because it will be overwritten.

> You might have noticed the extension of the cargo.toml file.  TOML stands for "Tom's obvious, minimal language" and used for config files in rust. You can learn more about toml <a href="https://github.com/toml-lang/toml" target="_blank">here</a>.

Like the NPM ecosystem, if you're building an end-user application, you should include the cargo.lock to your VCS. If you're building a library that others will be reusing, your cargo.lock must be added to .gitignore.

## Building & Releasing

We're too early in our rust journey to worry about building and releasing. Unlike JS/PHP, Rust is a compiled language. That means to run even the basic hello world example, we need to learn how to compile the code and run it. You can do that by using cargo.

`cargo build` command can be used to build the rust files into binary executable. If there are any issues in your code, the compiler will throw errors. Rust compiler's error messages are really helpful. They offer a possible fix for the error if you read the full message (you should).

So assuming your code compiled, a folder named `target` will be created. The Target folder contains all the build artifacts and intermediate compile stage files. Keeping the intermediate files will make the next compilations faster than the first compilation.

There are two types of builds in rust, one is debugging another one is for release. The debug build is not optimized and contains data required for debugging. The release build is optimized and it won't keep anything for debugging. You can create a release build by running `cargo build —release` command.

Rust includes the dependencies within your final build. This means that your binary should work without any issues in most systems. But this comes with a trade-off. Including all dependencies will increase the binary size. Rust chooses maximum portability over smaller binary executable.

## Language syntax & Features

### Modules
Rust's idea of code reuse/sharing within a project is a module. A module is a set of functions grouped under a common namespace. There are a lot of built-in modules in rust such as `std` which is the standard module.

We can create our own modules within the current file or as a separate file. Conventional wisdom is group modules on their own files. For me, the syntax for importing an external module is a little confusing. I have only seen a small part of it and yet to dive deep into defining using modules. My take away here is that Rust supports what is possible in PHP for modules and  importing, `use` statement, and `as` syntax.

### Access Modifiers
Rust supports two types of visibility public and private. The public is defined by `pub` keyword but there is no separate keyword (AFAIK) for private. Wondering why?

Rust chooses private by default. This is different than all the other languages I know. In typescript, PHP public is the default. From experience, I know how hard it is when debugging or refactoring to know the actual usage of a method.

In Rust, if it is not pub it can't be used anywhere, period. This is a great design choice that aligns with the rest of the rust's design principles. It makes it impossible to make mistakes in this regard.

### Variables

Variables are interesting things in rust. They are defined using `let` keyword. Variables are immutable by default. To make them mutable you must use the `mut` keyword before the variable name.

```rust
// default immutable variable
let age = 26;

// mutable variable
let mut age = 26;
```

The immutability as default will make you take a moment and think about the usage for every variable. In effect, you will start thinking about the collective state and data flow.

Rust is strictly typed, but we don't have to define data types for every variable. Rust will infer data types from the value or the usage of the variable. Rust's type inference is different than typescript. When the rust compiler can't guess a variable's type or if there is an ambiguity of types, the code will not compile. In that case, we must define a type for that variable and get rid of the ambiguity.

### Data types

Rust's primitive data types are integers, Float, Character, Boolean. There are compound types such as Tuples, and Array. I had trouble grasping the limits of each integer type such as i8 or u8. I wasn't sure how to decide which type of integer I should use in a given place. This is important because you should not use more memory than you need.

I loved how Rust made me think about the low-level stuff such as how much memory does this variable needs. Usually, in typescript, we just type it as the `number` and call it done. But rust has the following data types for signed and unsigned integers.

<figure>
    	<img src="/assets/img/posts/2020-05-10-rust-learning-quest-1/rust-data types.png">
</figure>

To calculate the limit of each type, the manual gave a formula that is really useful. I must admit, that any programmer with a CS degree or anyone having experience with other low-level languages might find this trivial but it is hard for me.

> A variable can store -(2^n - 1) to (2^n - 1)-1 where n is the number of bits. This formula is for signed integers. So i8 can store -255 to 254.

The `isize` and `usize` are interesting types because their size will be determined based on the architecture of the system that the code will be running. For 32-bit systems they'll be 32 bit and 64 for the 64-bit machines.

When you try to assign a value that is bigger than the range of the integer type, It is an issue. In debug mode rust panics. Panic is rust's name for errors. In release mode, the code runs but reduces the value to the possible value that variable can store. This reducing behavior is called **wrapping**.

### Statement vs Expressions

Rust introduces us to the difference between statement vs expressions. This difference plays a major role in how we create and return values from blocks and functions. Also in How we can assign values to a variable conditionally.

A statement is something that state that a new value is created or a new item is created in the scope. Ex.

```rust
Let number: i8 = 45;
```

In other languages statements can return values, but in rust statements can't return any values.

```rust
let a = b = 10;
```

the above code is valid in Javascript. Both a and b will be assigned with a value of 10. In rust, this will result in a compile error.

An expression is something that can return a value. Rust is an expression based language. Most things you use are expressions. Such as calling a function, calling a macro, evaluating a numerical operation. Since expressions return values, you can use them on the right-hand side of an assignment. That opens a lot of possibilities and syntactic sugar.expression-based

```rust
let age = {
	let current_year = 2020;
	let year_of_birth = 2013;
	current_year - year_of_birth
};
// age = 7;
```

Notice that we created a new scope using `{}` that is an expression. You can return a value from an expression simply by not putting a `;` at the last line of the expression. If you use a semicolon, the expression will turn into a statement and you'll be served with an error. This might be hard to grasp at first. It makes sense once you starts writing functions with expressions.

### Functions

Functions are named using snake_case and start with an `fn` keyword. Unlike variables, it is mandatory to define both the argument and the return types of a function. Argument are typed using the usual `:` symbol, function return types though uses a weird `->` arrow.

Functions are made up of a series of statements and expressions. If you want to return a value you can use the return statement. But, the last expression in the function will be implicitly returned. So we may need to use the `return` statement only when we need an early return based on a condition.

### Control Flow

Rust has `if` and the usual suspects such as the `while` and `for..in`. Conditionals in rust are different. In JS, if you put something in a conditional the interpreter will try to evaluate them to a truthy or a falsy value. That leads to some unexpected edge cases. But rust will not try to convert any non-boolean values to boolean and will throw a compile error instead. Rust if is an expression. so you can do this.

```rust
let number = if condition { 5 } else { 6 };
```

For looping, there is a construct called Loop. It just loops over the statements inside it until we stop using the `break` statement. All the other looping constructs can be written using just the loop and the break.

```rust
fn main() {
    loop {
        println!("All work and no play makes jack a dull boy!");
    }
}
```

I also learned a bit about what is `ownership` and how Rust does its magic without garbage collecting. That's a huge topic and I should learn about `lifetime` in rust before writing about ownership. For now, just assume that your variables will be removed from memory once the code ran past their scope.

### Final words

These are the things I thought important in my first week of learning Rust. It is exciting to see how useful rust can be in a web developer's toolkit. I am hoping to finish 3, 4 projects before the end of the year and to learn rust to an intermediate level.

Are you familiar with Rust or thinking about learning it? Comment here or DM me on <a href="https://twitter.com/ShivEnigma" target="_blank">@ShivEnigma</a>. let's chat about your experience and see if we can work together on any open-source project.
