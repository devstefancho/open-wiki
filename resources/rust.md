---
published: false
id: rust
slug: rust
title: Rust
description: rust
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-09-13
updatedDate: 2024-01-23
---

# Rust

Rust is installed using the Rustup tool.

You can learn more about this in detail from the below link, although this is not a requirement for the course. Just a useful resource if you want to go deep into using rustup to perform tasks such as:

Managing Multiple Versions
Installing Additional Components
Cross Compiling (for WASM or web-assembly as an example)


## rustup

```bash
rustup --version # Show current version
rustup update # Update rust
rustup toolchain list # List toolchains
rustup toolchain install <version> # Install specific version
rustup default <version> # Set default version
rustup toolchain uninstall <version> # Uninstall a toolchain
rustup target add <target> # Add target for cross-compilation
rustup component list # List installed components
rustup component add <component> # Add a component
rustup component remove <component> # Remove a component
rustup which rustc # Check which rustc binary is currently in use
```


## cargo

```bash
cargo new <project-name> # create new rust project
cargo new --lib <library-name> # create new rust library
cargo add tokio # add package
cargo add tokio --features full
cargo add reqwest --features json # async package
cargo build
cargo run
cargo run --release
cargo test
cargo doc # create doc
cargo doc --open # open html doc
cargo fmt # format file
```


## Data Type Cheatsheet

| category           | data type                      | description                               | copy trait          | memory allocation |
|--------------------|--------------------------------|-------------------------------------------|---------------------|-------------------|
| Scalar Types       | i8, i16, i32, i64, i128, isize | Signed Integer                            | Yes                 | Stack             |
| Scalar Types       | u8 ~ usize                     | Unsigned Integer                          | Yes                 | Stack             |
| Scalar Types       | f32, f64                       | Floating point numbers                    | Yes                 | Stack             |
| Scalar Types       | bool                           |                                           | Yes                 | Stack             |
| Scalar Types       | char                           | Unicode scalar values                     | Yes                 | Stack             |
| Compound Types     | [T; N]                         | Fixed-size arrays                         | Depends on T        | Stack             |
| Compound Types     | (T, U, ..)                     | Tuples                                    | Depends on elements | Stack             |
| Compound Types     | Vec<T>                         | Resizable arrays                          | No                  | Heap              |
| Text Types         | String                         | Grow-able, mutable, owned UTF-8 strings   | No                  | Heap              |
| Text Types         | &str                           | Immutable string references               | No                  | Depends           |
| Pointer Types      | &T, &mut T                     | References                                | No                  | Stack             |
| Pointer Types      | Box<T>                         | Heap-allocated values (smart pointer)     | No                  | Heap              |
| Option Types       | Option<T>                      | Optional values                           | Depends on T        | Depends           |
| Result Types       | Result<T,E>                    | Return type for functions that could fail | Depends on T and E  | Depends           |
| Special Types      | fn(T) -> U                     | Function pointers                         | Yes                 | Stack             |
| User-Defined Types | Structs and Enums              | Custom data types                         | Depends             | Depends           |


## Stack vs Heap

|            | stack                                                | heap                                                                         |
|------------|------------------------------------------------------|------------------------------------------------------------------------------|
| size       | fixed (known at compile time)                        | dynamic                                                                      |
| speed      | faster than heap                                     | slower than stack                                                            |
| usage      | local variables, function call data                  | dynamic data, large object that outlive their scope                          |
| management | Rust(ownership system) and C(compiler) are automatic | Rust is automatic(ownership system), C is manual (new, delete, malloc, free) |
| scope      | within scope where declared                          | anywhere complying with ownership rules                                      |

```rust
const MY_INTEGER: u8 = 10;

fn main() {
    // Stack
    let x: u8 = 50;
    println!("x i {}", x);
    
    // Heap
    let mut arr: Vec<u8> = vec![1,2,3,4,5];
    arr.push(10);
    println!("vec is {:?}", arr);
    
    // A Reference on the Stack pointing to a value on the Heap
    let arr_2 = &arr[0..3];
    println!("arr_is {:?}", arr_2);
    
    // Heap
    let mut s: String = String::from("stefancho");
    s.push(' ');
    s.push('!');
    println!("s is {:?}", s);

    // A Reference on the Stack pointing to a value on the Heap
    let s_2 = &s[0..6];
    println!("s_2 is {:?}", s_2);
    
    println!("Global Variable MY_INTEGER is {:?}", MY_INTEGER);
    
    // string literal is pointing to location of the memory (this is not in heap memory)
    let s_3: &str = "halo"
}
```


## Ownership && Borrowing

Ownership
1. Each value in Rust has a single owner (i.e. variable) at a time
2. The value is dropped when its owner goes out of scope
Borrowing
1. You can have any number of immutable (read-only) references
2. References must be vali:w

```rust
fn make_string_dangle() -> &String {
    let s: String = String::from("dangle");
    let r: &String = &s; // s is dropped after function end, so r is not exist
    r

fn main() {
    let dangle: &String = make_string_dangle(); // get error because return value is dropped after out of scope
    println!("{}", dangle);
}
}
```


## Pointer
```rust
// how to see pointer value
let s_0 = String::from("test");
let s_3: &str = &s_0;
println!("s_3 is {:p}", s_3);
```


## Dereference
rust is automatically dereference value (dereference coercion)

```rust
// mutable variable에 일반적인 값 할당 예시
let mut name_t: String = String::from("stefan");
name_t = String::from("cho");
println!("{}", name_t);

// 참조값만 할당하여 deref 처리
let mut name: String = String::from("aurora");
let name_s: &mut String = &mut name; // 참조값(주소)만 할당한 것이라 실제 값은 없음
*name_s = String::from("heo"); // name_s는 참조값만 갖고 있으므로 deref를 해줘야 값을 알 수 있음
println!("{}", name_s);
```


## Library

```rust
// block chain library
cargo add ethers@2.0.4
```


## Lifetime
- compare_a와 compare_b가 있다면 return의 Lifetime은 두개중에서 더 짧은 Lifetime을 가진것을 따른다
- [Lifetime annotation](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-annotations-in-function-signatures)으로 명시적으로 lifetime 관계를 표시할 수 있음

```rust
fn get_larger<'a>(compare_a: &'a i32, compare_b: &'a i32) -> &'a i32 {
    if compare_a > compare_b {
        compare_a
    } else {
        compare_b
    }
}

#[cfg(test)]
mod test {
    use super::*;

    #[test]
    fn test_lifetime() {
        let result: &i32;
        let num_1: i32 = 10;
        let num_2: i32 = 20;
        {
            let large_num = get_larger(&num_1, &num_2);
            result = large_num;
        }

        println!("large number is {}", result);
    }
}
```


## Polymorphism with traits

예시를 보면 하나의 EthereumAddress trait에서 두가지(&str와 Address)를 받을 수 있다.
```rust
use ethers::types::Address;
use std::str::FromStr;

trait EthereumAddress {
    fn convert_address(&self) -> Result<Address, &'static str>;
}

impl EthereumAddress for &str {
    fn convert_address(&self) -> Result<Address, &'static str> {
        match Address::from_str(self) {
            Ok(address) => Ok(address),
            Err(_) => Err("invalid address"),
        }
    }
}

impl EthereumAddress for Address {
    fn convert_address(&self) -> Result<Address, &'static str> {
        Ok(*self)
    }
}

fn get_ethereum_data<T: EthereumAddress>(address: T) -> Address {
    let converted_address = address.convert_address().unwrap();
    converted_address
}

#[cfg(test)]
mod test {
    use super::*;

    #[test]
    fn test_poly() {
        let addr = Address::from_str("0x6b333B20fBae3c5c0969dd02176e30802e2fbBdB").unwrap();

        // impl에서 &str과 Address에 대해서 둘다 처리 했으므로 두가지 타입 모두 넣을 수 있음
        let new_addr = get_ethereum_data(addr);
        let new_addr = get_ethereum_data("0x6b333B20fBae3c5c0969dd02176e30802e2fbBdB");
        assert_eq!(addr, new_addr);
    }
}
```


## match guard
match에 조건문을 넣어서 [guard](https://doc.rust-lang.org/reference/expressions/match-expr.html#match-guards)를 할 수 있다
```rust
fn match_guard() {
    struct Location {
        x: i32,
        y: i32,
    }
    let lo = Location { x: 0, y: 0 };

    match lo {
        Location { x, y } if x == 0 && y == 0 => println!("location is on origin"),
        Location { x: _, y: _ } => println!("location is not on origin"),
    };
}
```


## Error Handling
when error handle, recommend to use `std::io::Error` because this is consistent erorr type


## Ref
- [docs](https://doc.rust-lang.org/book/)
- [resource](https://rust-lang.github.io/rustup/index.html) 
- [playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021)
- [rust package registry](https://crates.io/)

