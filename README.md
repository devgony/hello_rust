# Hello Rust

# install

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

# update

```
rustup update
```

# 1.2. Hello, World!

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

# 1.3. Hello, Cargo!

## cargo expects soure files to live inside the `src` directory.

### top level for: README, license, configuration

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

# 3.1. Variables and Mutability

- immutable variable

```rs
let mut x = 5;
```

## Differences Between Variables and Constants

### const:

- type must be annoated
- can be decalred in any scope
- be set only to a constant expression, can't get after runtime

## Shadowing

- shadow a variable by using the same variable’s name and repeating the use of the let keyword as follows:
- with `let`, perform a few transformations on a value but have the variable be immutable after those transformations have been completed.

### we can change the type of the value but reuse the same name

```rs
    let spaces = "   ";
    let spaces = spaces.len();
```

### However, if we try to use mut for this, as shown here, we’ll get a compile-time error:

```rs
let mut spaces = "   ";
spaces = spaces.len(); // expected `&str`, found `usize`
```

# 3.2. Data Types

- Let rust know type at compile time like TS

## Scalar Types

### Integer Types

- single value like: `integers, floating-point numbers, Booleans, and characters`

```rs
Signed i8: -128 ~ 127
Unsigned u8: 0 ~ 255
```

- default : `i32`

### Floating-Point Types

- default : `f64`

### The Boolean Type

```rs
    let t = true;
    let f: bool = false; // with explicit type annotation
```

### The Character Type

- use `single quotes` (double for string)

```rs
    let c = 'z';
    let z = 'ℤ';
    let heart_eyed_cat = '😻';
```

## Compound Types

- group multiple values into one type

### The Tuple Type

- fixed length: once declared, they cannot grow or shrink in size

```rs
    let tup: (i32, f64, u8) = (500, 6.4, 1);
```

#### Destructing

```rs
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
println!("The value of y is: {}", y);
```

#### Access a tuple element directly by using a period (.) followed by the index

```rs
let x: (i32, f64, u8) = (500, 6.4, 1);
let five_hundred = x.0;
let six_point_four = x.1;
let one = x.2;
```

### The Array Type

- Unlike a tuple, every element of an array must have the `same type`
- fixed length, like tuples.
- Vector is allowed to grow or shrink in size
- unsure whether to use an array or a vector? you should probably use a vector

```rs
let a: [i32; 5] = [1, 2, 3, 4, 5]; // 5 elems of type i32
let a = [3; 5]; // init array with 5 elems of value 3
```

#### Accessing Array Elements

```rs
    let a = [1, 2, 3, 4, 5];
    let first = a[0];
    let second = a[1];
```

#### Invalid Array Element Access => run time error

- Rust protects you against this kind of error by immediately exiting instead of allowing the memory access and continuing.

# 3.3. Functions

- pervasive
- snake case as the conventional style for function and variable names.
- hositing supported

```rs
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

## Function Bodies Contain Statements and Expressions

- Rust is an expression-based language
- Statements are instructions that perform some action and `do not return a value`.
  - assigning value to var
  - defining function

```rs
    let y = 6; // statement
    let x = (let y = 6); // error: expected expression, found statement (`let`)
```

- Expressions evaluate to a resulting value. Let’s look at some examples.
  - evaluating like `5 * 6`, or just `6`
  - calling function
  - calling macro
  - do not include ending semicolons
  - block to create new scopre `{}` can be expression

### If you add a semicolon to the end of an expression, you turn it into a statement, which will then not return a value

```rs
    let x = 5;
    let y = {
        let x = 3;
        x + 1 // expression
    };
```

## Functions with Return Values

- declare their type after an arrow (->)
- final expression as synonym for return

```rs
fn five() -> i32 {
    5
}

fn main() {
    let x = five();
    println!("The value of x is: {}", x);
}
```

# 3.4. Comments

- recommand to use comment on a separate line above the code annotating

```rs
fn main() {
    // I’m feeling lucky today
    let lucky_number = 7;
}
```

# 3.5. Control Flow

## if Expressions

- Blocks of code associated with the conditions in if expressions are sometimes called `arms`

- condition must be a `bool`: Rust will not automatically try to convert non-Boolean types to a Boolean

## Handling Multiple Conditions with else if

- Rust only executes the block for the first true condition, and once it finds one, it doesn’t even check the rest

```rs
    let number = 6;

    if number % 4 == 0 { // false? -> go to else
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 { // wasn't event touched
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
```

## Using if in a let Statement: Ternary

- if is expression -> use in let

```rs
    let number = if condition { 5 } else { 6 };

```

## Should unify types for all arms in if

```rs
    let number = if condition { 5 } else { "six" }; // error[E0308]: `if` and `else` have incompatible types
```

## Repetition with Loops

### Repeating Code with loop

#### Returning Values from Loops with break

```rs
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };
    println!("The result is {}", result);
}
```

## Conditional Loops with while

```rs
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;
    while index < 5 { // but index can be over than length -> use for e in a
        println!("the value is: {}", a[index]);
        index += 1;
    }
```

## Looping Through a Collection with for

```rs
fn main() {
    let a = [10, 20, 30, 40, 50];
    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

```rs
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```
