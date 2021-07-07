# Hello Rust

# install

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

# update

```
rustup update
```

# 1. ch01-02-hello-world

## `!`: calling a macro instead of normal function

```rs
// main.rs
fn main() {
    println!("Hello, world!");
}
```

```sh
rustc main.rs
.\main.exe
Hello, world!
```

## cargo

### cargo expects soure files to live inside the `src` directory.

#### top level for: README, license, configuration

```rs
cargo new hello_cargo
```

## TOML: Tom's Obvious, Minimal Language

## build: executable file in target/debug/hello_cargo

```rs
cargo build
./target/debug/hello_cargo
or
cargo run
cargo check // check compilable but doesn't product an executable
```

## Run cargo check periodically to speed up

## Building for Release

```rs
cargo build --release
```

# ch02.Guessing game

## A crate is a collection of Rust source code files

```
// Cargo.toml
[dependencies]
rand = "0.8.3"
```

## update crate automatically (to last z version)

```
cargo update
```

## build new docs

```
cargo doc --open
```

## let, mut, associate function with ::

```rs
let mut guess = String::new(); // mutable var, associated function ::new() of empty instance of a String
```

## match

```rs
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"), // enum::
```

## Listing 2-6: Complete guessing game code

```rs
use rand::Rng;
use std::cmp::Ordering;
use std::io; // bring io from standard lib, if there is no types in the prelude

fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1..101);
    println!("The secret number is: {}", secret_number);
    loop {
        println!("Please input your guess.");
        let mut guess = String::new(); // mutable var, associated function ::new() of empty instance of a String
        io::stdin()
            .read_line(&mut guess) // references are immutable by default => need &mut
            .expect("Failed to read line"); // read_line returns io:Result and gets warning if don't expect()
        let guess: u32 = match guess.trim().parse() {
            // shadow the previous value
            Ok(num) => num,
            Err(_) => continue,
        };
        println!("You guessed: {}", guess); // placeholder {}
        match guess.cmp(&secret_number) {
            // arm?
            Ordering::Less => println!("Too small!"), // enum::
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```
