---
title: RUST-Test
cover: ''
date: 2023-09-16 18:01:17
updated: 2023-09-16 18:01:17
tags:
categories:
---


## The Test Library

```
assert_cmd 
```


### assert_cmd库
Uses the assert_cmd crate to test the output of a binary called "hello". The test case verifies that executing the "hello" binary is successful and that the stdout (standard output) matches the expected string "Hello, world!\n".
```
use assert_cmd::Command;

#[test]
fn work(){
    let mut cmd = Command::cargo_bin("hello").unwrap();
    cmd.assert().success();
    cmd.assert().success().stdout("Hello, world!\n");
}
```