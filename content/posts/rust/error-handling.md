---
title: "Error handling in Rust"
date: 2021-10-21T18:34:56+02:00
draft: false
author: 'Samuel Martins'
ShowCodeCopyButtons: true
ShowBreadCrumbs: true
tags: [rust]
categories: [programming]
---


Errors are managed by the type:
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```
We can use the operator `match` to manage them or other ways explained below.

### Unrecoverable Errors
```rust
panic!("crash and burn");
```
Ex. access an out of bound index in an array.

### Fast retrive of value
To retrive value from a `Result` we can use the following methods:
```rust
.unwrap()
.expected("error message")
```
if `Result` contains an `Err` the program panics

### Propagation
To propagate an error you can either return an `Err` or use the  `?` operator:
```rust
File::open("hello.txt")?.read_to_string(&mut s)?;
```
If there is an error it returns the error otherwise it gets the value requested.

### Validation
If we need to validate a certain value we can create a custom type and use the constructor as validator:
```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
```

Source
----

https://doc.rust-lang.org/stable/book/ch09-00-error-handling.html