---
title: RUST
cover: ''
abbrlink: d7a76f14
date: 2023-09-15 07:58:15
updated: 2023-09-15 07:58:15
tags:
categories:
---






## Install





## Example
### Run Demo
1. Use rustc
- hello.rs
```
fn main(){
    println!("hello world")
}
```
- rustc hello.rs
- ./hello
2. Use Cargo
```
cargo new hello
cd hello
cargo run
```
### Test Demo

- mkdir tests
- vim tests/cli.rs
```
use std::process::Command;

#[test]
fn work(){
    let mut cmd = Command::new("ls");
    let res = cmd.output();
    assert!(res.is_ok());
}
```
- cargo test

### Adding a project dependency

