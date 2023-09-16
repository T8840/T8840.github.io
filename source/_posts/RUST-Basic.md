---
title: RUST-Basic
cover: ''
date: 2023-09-15 07:58:27
updated: 2023-09-15 07:58:27
tags:
categories:
---


## Variable

## Data Type

### String

### Vec

```
Vec::join
```

### Values

### Option



## Statement

## Symbol

```
{:?}
{:#?}
```


### Loop Statement

### Condition Statement

## Function Feature

### #[derive(Debug)]
#[derive(Debug)] is an attribute in Rust used to automatically generate code that implements the Debug trait. The Debug trait is a built-in trait in Rust that provides a default way to format and print values for debugging purposes.

By adding #[derive(Debug)] to a struct or an enum, the Rust compiler automatically generates the necessary code to implement the Debug trait. This allows us to use println! macro or other debugging tools to print and inspect the contents of the types.

### #[test]



## Function



### .unwrap()
The .unwrap() method in Rust is used to extract the value from an Option or Result type. It returns the inner value if it exists or panics if it encounters a None or Err variant.

### std::env::args()
std::env::args() is a function provided by the Rust standard library (std::env module) that returns an iterator over the command-line arguments passed to the program.

The args() function returns an iterator of type std::env::Args, which represents the individual command-line arguments as strings. The first element of the iterator (std::env::Args) is always the name of the executable binary itself.

Here's an example of how to use std::env::args():
```
use std::env;

fn main() {
    // Access the command-line arguments
    let args: Vec<String> = env::args().collect();

    // Print the binary name and command-line arguments
    println!("Binary: {}", args[0]);
    println!("Arguments: {:?}", &args[1..]);
}
```

The output:
```
$ ./my_program arg1 arg2 arg3
Binary: ./my_program
Arguments: ["arg1", "arg2", "arg3"]
```

### clap

```
matches.values_of("text").unwrap();
matches.values_of_lossy("text").unwrap();
matches.is_present("omit_newline");

```


- Example: use clap="3"
```
use clap::{App,Arg};

fn main() {
    let args = App::new("MyApp")
        .version("1.0")
        .author("jonhn")
        .help("A simple App")
        .arg(
            Arg::with_name("input")
                .short('i')
                .long("input")
                .value_name("FILE")
                .help("Sets the input file")
                .takes_value(true),
                )
        .arg(
            Arg::with_name("output")
                .short('o')
                .long("output")
                .value_name("FILE")
                .help("Sets the output file")
                .takes_value(true),
                )
        .get_matches();
    println!("{:#?}",args);
    if let Some(input_file) = args.value_of("input") {
        println!("Input file:{}",input_file);
    }
    if let Some(output_file) = args.value_of("output") {
        println!("output file:{}", output_file);
    }
}
```

### std::fs

### std::error::Error