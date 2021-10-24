---
title: "Collections in Rust"
date: 2021-10-22T15:15:56+02:00
draft: true
author: 'Samuel Martins'
ShowCodeCopyButtons: true
ShowBreadCrumbs: true
tags: [rust]
categories: [programming]
showtoc: true
---


## Vector

### Creation and isertion
```rust
// create vector
let v: Vec<i32> = Vec::new();
let v = vec![1, 2, 3];

// insert elements
v.push(5);
```
When a Vector goes out of scope all its elements are freed.

### Read elements

To read an element we have two options:
```rust
let third: &i32 = &v[2];
v.get(2) // returns Option 
```
If we try to access an out of bound elment in the first case the program panics. Therefore the second is better for these cases.

### Immutable borrow
In certain cases the first method to read elements can be problematic:
```rust
// !doesn't compile
    let first = &v[0]; //immutable borrow occurs here
    v.push(6); //mutable borrow occurs here
```

### Iterating

```rust
for i in &v {...}
for i in &mut v {...}
```

### Store multiple types
We can use `enum` to store diferent types in the same structure and put them in an array.
```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}
```

## String

### Create string
```rust
let mut s = String::new();
let s = "initial contents".to_string();
let s = String::from("initial contents");
```

### Updating a string
```rust
s.push_str("bar");
let s = s1 + "-" + &s2 + "-" + &s3; // take ownership of s1
let s = format!("{}-{}-{}", s1, s2, s3); // doesn't take ownership
```

### Iterate
A char can be rappresented by multiple bytes:
```rust
for c in s.chars()
for b in s.bytes()
```

## Hash Maps

### Creation and isertion
```rust
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
```

### Insertion
```rust
 map.insert(v, k);
```
after insertion `v` and `k` are invalid for owned type like `String`. In case of types with the `Copy` trait like `i32` the values are copied.


### Access values
```rust
let score = scores.get(&team_name); // returns Option<&v>
```

### Iteration
```rust
for (key, value) in &scores {...}
```

### Update HashMap
```rust
// overwrite value
scores.insert(String::from("Blue"), 25);

// insert if there is no value
scores.entry(String::from("Yellow")).or_insert(50);

// update old value
let count = map.entry(word).or_insert(0);
*count += 1;
```
### Others

Creating a hash map from a list of teams and a list of scores
```rust
use std::collections::HashMap;

let teams = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

let mut scores: HashMap<_, _> =
    teams.into_iter().zip(initial_scores.into_iter()).collect();
```

The Hash Map inplementation in Rust uses the Hash function SipHash. You can use other hash function from crates.io.

Source
----

https://doc.rust-lang.org/stable/book/ch08-00-common-collections.html