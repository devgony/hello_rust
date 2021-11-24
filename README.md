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

# 2.1. Guessing game

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

# 4.1. What Is Ownership?

- Some languages have garbage collection that constantly looks for no longer used memory as the program runs;
- in other languages, the programmer must explicitly allocate and free the memory.
- Rust uses a third approach: `memory is managed through a system of ownership with a set of rules that the compiler checks at compile time.`

## The Stack and the Heap

### STACK (index book of the staff)

- The stack stores values in the order it gets them and removes the values in the opposite order.: LIFO
- All data stored on the stack must have a known, fixed size
- pointer is a known, fixed size, you can store the pointer on the stack

### HEAP (Real Table seat)

- Data with an unknown size at compile time or a size that might change must be stored on the heap
- you request a certain amount of space
- marks it as being in use, and returns a pointer which is the address of that location (allocating on the heap)

### Ownership does..

- Keeping track of what parts of code are using what data on the heap
- Minimizing the amount of duplicate data on the heap
- Cleaning up unused data on the heap so you don’t run out of space

## Ownership Rules

- Each value in Rust has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

## Variable Scope

### string literal -> stored in stack

```rs
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward

    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

## The String Type -> stored in heap

- is allocated on the heap and as such is able to store an amount of text that is unknown to us at compile time

```rs
let mut s = String::from("hello");
s.push_str(", world!"); // push_str() appends a literal to a String
println!("{}", s); // This will print `hello, world!`
```

## Memory and Allocation

- The memory must be requested from the memory allocator at runtime.
- We need a way of returning this memory to the allocator when we’re done with our String.

### Rust's memory is automatically returned once the variable that owns it goes out of scope

- Rust calls `drop()` automatically at the closing curly bracket.

```rs
{
    let s = String::from("hello"); // s is valid from this point forward

    // do stuff with s
}                                  // this scope is now over, and s is no
                                    // longer valid
```

## Ways Variables and Data Interact: Move

- Stack: pointer, length, capacity
- Heap: content

### Rust considers s1 to no longer be valid and, therefore, Rust doesn’t need to free anything when s1 goes out of scope

```rs
let s1 = String::from("hello");
let s2 = s1;
println!("{}, world!", s1); // value borrowed here after move
```

- Only Stack data is copied
- Previous Stack data `s1` has been invalidated
- Rust will never automatically create “deep” copies of your data

## Ways Variables and Data Interact: Clone

```rs
let s1 = String::from("hello");
let s2 = s1.clone();
println!("s1 = {}, s2 = {}", s1, s2); // stack and heap data is also cloned
```

- Is it shallow copy? or head copy?

## Stack-Only Data: Copy

### known size at compile time -> Stack-Only -> Copy

```rs
let x = 5;
let y = x;
println!("x = {}, y = {}", x, y);
```

### can be with Copy type annotation only for...

- All the integer types, such as `u32`.
- The `Boolean` type, bool, with values true and false.
- All the floating point types, such as `f64`.
- The character type, `char`.
- `Tuples`, if they only contain types that also implement Copy. For example, `(i32, i32)` implements Copy, but (i32, String) does not.

## Ownership and Functions

- Passing a variable to a function will move or copy

```rs
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
    println!("{}", s);                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
    println!("{}", x);              // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

## Return Values and Scope

```rs
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("hello"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// takes_and_gives_back will take a String and return one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

## It’s possible to return multiple values using a tuple

```rs
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

### But too verbose -> use references

# 4.2. References and Borrowing

## pass and take reference using `&` without taking `ownership`

### s -> s1 -> heap_data

```rs
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // Here, s goes out of scope. But because it does not have ownership of what
  // it refers to, nothing happens.
```

## Call having references as function parameters borrowing

## Can't modify variables of borrowing `by default`

```rs
fn main() {
    let s = String::from("hello");
    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world"); // cannot borrow `*some_string` as mutable, as it is behind a `&` reference
}
```

## Mutable References with `&mut`

```rs
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

### Can have `only one` mutable reference to a particular piece of data in a particular scope

```rs
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s;
println!("{}, {}", r1, r2); // error[E0499]: cannot borrow `s` as mutable more than once at a time
```

### Can prevent data races at compile time

#### 3 reason of data race

- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data.

## `Multiple mutable` reference with `different scope`

```rs
   let mut s = String::from("hello");
    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.
    let r2 = &mut s;
```

## Cannot have a mutable reference while we have an immutable one

```rs
let mut s = String::from("hello");
let r1 = &s; // no problem
let r2 = &s; // no problem
let r3 = &mut s; // BIG PROBLEM
println!("{}, {}, and {}", r1, r2, r3);
```

## Can use immutable before &mut

```rs
let mut s = String::from("hello");
let r1 = &s; // no problem
let r2 = &s; // no problem
println!("{} and {}", r1, r2);
// r1 and r2 are no longer used after this point
let r3 = &mut s; // no problem
println!("{}", r3);
```

## Dangling References

### References must always be valid.

```rs
fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger!
```

### solution: return String directly

```rs
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```

# 4.3. The Slice Type

## Unsafe code returing index

```rs
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }
    s.len()
}

fn main() {
    let mut s = String::from("hello world");
    let word = first_word(&s); // word will get the value 5
    s.clear(); // this empties the String, making it equal to ""
    // word still has the value 5 here, but there's no more string that
    // we could meaningfully use the value 5 with. word is now totally invalid!
}
```

## String Slices

- Rather than a reference to the entire String, it’s a reference to a portion of the String

```rs
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..11];
```

### Rust’s `..` range syntax

```rs
let slice = &s[0..2]; // equal to below
let slice = &s[..2];
```

```rs
let len = s.len();
let slice = &s[3..len]; // equal to below
let slice = &s[3..];
```

```rs
let slice = &s[0..len]; // equal to below
let slice = &s[..];
```

### Right answer for first_work

- `&str`: string slice type

```rs
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

## But got immutable borrow error -> can eliminate an entire class of errors at compile time!

```rs
fn main() {
    let mut s = String::from("hello world");
    let word = first_word(&s);
    s.clear(); // error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
    println!("the first word is: {}", word); // immutable borrow?
}
```

## String Literals Are Slices

### For more general API

```rs
fn first_word(s: &str) -> &str {
```

### slices of `String = slices of string literals = string literals

```rs
fn main() {
    let my_string = String::from("hello world");

    // first_word works on slices of `String`s
    let word = first_word(&my_string[..]);

    let my_string_literal = "hello world";

    // first_word works on slices of string literals
    let word = first_word(&my_string_literal[..]);

    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}
```

# 5.1. Defining and Instantiating Structs

- object + class

```rs
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

## Create an instance of that struct + mutation (entire instance must be mutable)

```rs
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
user1.email = String::from("anotheremail@example.com"); // mutate
```

## Using the Field Init Shorthand having same name + return struct

```rs
fn build_user(email: String, username: String) -> User {
    User {
        email, // init with param
        username, // init with param
        active: true,
        sign_in_count: 1,
    }
}
```

## Update Syntax (like spread operator)

```rs
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1 // Struct Update Syntax
};
```

## Using Tuple Structs (without Named Fields to Create Different Types)

```rs
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

## Unit-Like Structs `struct User()` => Ch.10 traits

```rs
struct AlwaysEqual;
let subject = AlwaysEqual;
```

- when don't have any data that you want to sotre in the type itself.
- empty struct -> for trait

## Ownership of Struct Data

### To use reference(&) in struct, we need to specify lifetime => Ch. 10

```rs
struct User {
    username: &str,
    email: &str,
    sign_in_count: u64,
    active: bool,
}
```

### Let us use owned type like `String::` for now.

# 5.2. An Example Program Using Structs

## Refactoring with Variables

```rs
fn main() {
    let width1 = 30;
    let height1 = 50;

    println!(
        "The area of the rectangle is {} square pixels.",
        area(width1, height1)
    );
}

fn area(width: u32, height: u32) -> u32 {
    width * height
}
```

## Refactoring with Tuples

```rs
fn main() {
    let rect1 = (30, 50);

    println!(
        "The area of the rectangle is {} square pixels.",
        area(rect1)
    );
}

fn area(dimensions: (u32, u32)) -> u32 {
    dimensions.0 * dimensions.1
}

```

## Refactoring with Structs: Adding More Meaning

### to use struct, pass immutable borrow(&)

```rs
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 { // immutable borrow
    rectangle.width * rectangle.height
}
```

## Adding Useful Functionality with Derived Traits

- `#[derive(Debug)]`: to use derive annotation, explicate it
- `{:?}` is derive annotation
- `{:#?}` shows struct with pretty format

```rs
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:?}", rect1); // { width: 30, height: 50 }
    println!("rect1 is {:#?}", rect1);
    // {
    //     width: 30,
    //     height: 50,
    // }
}

```

# 5.3. Method Syntax

- similar to fuinctions
- but defined within the context of a struct (or an enum or a trait)
- first param is always `self`: the instance of the struct the method is being called on

## Defining Methods

```rs
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 { // borrow ownership of immutable Rectangle
    // &mut self to mutate
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area() // method syntax but rect1 as arg
    );
}
```

## automatic referencing and dereferencing

```rs
p1.distance(&p2);
(&p1).distance(&p2);
```

## Methods with More Parameters

```rs
fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
...
println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
```

- immutable borrow to rect2 => keep ownership in main

## Associated Functions

- doesn't take `self` as a param
- call by `::`
- often used for constructors

```rs
impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}
let sq = Rectangle::square(3);
```

## Multiple impl Blocks

- able to separate impl block

```rs
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

- struct is not the only way to create custom type => enum!

# 6.1. Defining an Enum

- enum values can only be one of its variants
- also custom data type
- each variant can have different types

```rs
enum IpAddrKind { // define enum
    V4,
    V6,
}

fn main() {
    let four = IpAddrKind::V4; // call enum
    let six = IpAddrKind::V6;

    route(IpAddrKind::V4);
    route(IpAddrKind::V6);
}

fn route(ip_kind: IpAddrKind) {}

// use different variants
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}
let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```

## Define methods on enum with `impl` like struct

```rs
enum Message {
    Quit,
    Move { x: i32, y: i32 }, // anonymous struct
    Write(String),
    ChangeColor(i32, i32, i32),
}
impl Message {
    fn call(&self) {
        // method body would be defined here
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

## The Option Enum and Its Advantages Over Null Values

- A value could be something or it could be nothing
- Rust does not have nulls, but it does have an enum that can encode the concept of a value being present or absent
- included in the prelude; you don’t need to bring it into scope explicitly

```rs
enum Option<T> {
    None,
    Some(T),
}
```

### Generic <T>

- `Some` variant of the `Option` enum can hold one piece of data of any type
- If we use None rather than Some, we need to tell Rust what type of Option<T>

```rs
let some_number = Some(5);
let some_string = Some("a string");
let absent_number: Option<i32> = None;
```

## How do you get the T value out of a Some variant?

1. match
2. unwrap_or, unwrap_or_else, or unwrap_or_default

# 6.2 The match Control Flow Operator

## Match?

- A coin-sorting machine
- `if expression` needs to return a Boolean but `match expression` can be any type.
- single line: just return `=>`, multi line: curly bracket

```rs
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        // arms
        // pattern => {code block} or return value
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

## Patterns that Bind to Values

- can bind to the parts of the values for enum variant

```rs
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}
```

## Matching with Option<T>

```rs
  fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            None => None,
            Some(i) => Some(i + 1),
        }
    }

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
```

## Matches Are Exhaustive

- we must exhaust every last possibility in order for the code to be valid

## The \_ Placeholder

- the `_` will match all the possible cases that aren’t specified before it.

```rs
let some_u8_value = 0u8;
    match some_u8_value {
        1 => println!("one"),
        3 => println!("three"),
        5 => println!("five"),
        7 => println!("seven"),
        _ => (), // unit value, nothing happen here
    }
```

# 6.3 Concise Control Flow with if let

- the match expression can be a bit wordy in a situation in which we care about `only one of the cases`

## match-exhaustive

```rs
 let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {}", max),
        _ => (),
    }
```

## if let-conciseness

```rs
    let config_max = Some(3u8);
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }
```

# 7.1. Packages and Crates

- Packages: A Cargo feature that lets you build, test, and share crates
- Crates: A tree of modules that produces a library or executable
  - Crate roots: `src/main.rs` and `src/lib.rs`
- Modules and use: Let you control the organization, scope, and privacy of paths
- Paths: A way of naming an item, such as a struct, function, or module

## Create new project

```sh
cargo new my-project
     Created binary (application) `my-project` package
ls my-project
    Cargo.toml
    src
ls my-project/src
    main.rs
```

# 7.2 Defining Modules to Control Scope and Privacy

## Create a new library: named restaurant industry

```sh
cargo new --lib restaurant
```

```rs
// restaurant/src/lib.rs
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
        fn seat_at_table() {}
    }
    mod serving {
        fn take_order() {}
        fn serve_order() {}
        fn take_payment() {}
    }
}
```

### Module tree

```
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

# 7.3. Paths for Referring to an Item in the Module Tree

## Two Forms separated by double colons (::)

1. An absolute path starts from a crate root by using a crate name or a literal `crate`.
2. A relative path starts from the current module and uses `self`, `super`, or an `identifier` in the current module.

```rs
mod front_of_house {
    mod hosting { // error[E0603]: module `hosting` is private
        fn add_to_waitlist() {}
    }
}
pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();
    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```

## Exposing Paths with the `pub` Keyword

- All functions, methods, structs, enums, modules, and constants are private by default
- Items in a parent module can't use the private items inside child modules.
- Child can use ancestors'

```rs
mod front_of_house {
    pub mod hosting { // make module public
        pub fn add_to_waitlist() {} // make function public
    }
}
pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();
    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```

- `front_of_house`?: cuz `eat_at_restaurant` is defined in the `same module`, we can refer to it

## Starting Relative Paths with `super`

```rs
fn serve_order() {} // don't need pub?

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::serve_order(); // super = crate root
    }
    fn cook_order() {}
}
```

## Making Structs and Enums Public

### pub struct

- `pub` sturct + each fields should be with explicit `pub`
- To set or construct private field, we can use public associated function

```rs
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        // if struct has private filed, we need public associated function as a setter
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}

pub fn eat_at_restaurant() {
    // Order a breakfast in the summer with Rye toast
    let mut meal = back_of_house::Breakfast::summer("Rye");
    // Change our mind about what bread we'd like
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);

    // The next line won't compile if we uncomment it; we're not allowed
    // to see or modify the seasonal fruit that comes with the meal
    // meal.seasonal_fruit = String::from("blueberries");
}
```

### pub enum

- `pub` enum's variants are `public by default`

```rs
mod back_of_house {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}

pub fn eat_at_restaurant() {
    let order1 = back_of_house::Appetizer::Soup;
    let order2 = back_of_house::Appetizer::Salad;
}
```

# 7.4. Bringing Paths Into Scope with the use Keyword

1. crate root way
2. relative way

```rs
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting; // or
// use self::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

## Creating Idiomatic use Paths

- `function`: bring into scope by parent
- `enum`, struct: bring into scope itself
- `same name` into scope: should be brought by parent
  - or use as Keywork

## Providing New Names with the as Keyword

```rs
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    // --snip--
}

fn function2() -> IoResult<()> {
    // --snip--
}
```

## Re-exporting Names with pub use

- When we bring a name into scope with the `use` keyword, the name available in the new scope is private => `pub use`!

```rs
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

## Using External Packages

```rs
// Cargo.toml
rand = "0.8.3"

// main.rs
use rand::Rng;

fn main() {
    let secret_number = rand::thread_rng().gen_range(1..101);
}
```

## Using Nested Paths to Clean Up Large use Lists

```rs
use std::{cmp::Ordering, io};
use std::io::{self, Write};
```

## The Glob Operator

```rs
use std::collections::*;
```

# 7.5 Separating Modules into Different Files

- `mod [moduleName];` : load module from file

```rs
// src/lib.rs
mod front_of_house; // ';' => load the contents of the module from another file with same name

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

- 1-layer file

  ```rs
  // src/front_of_house.rs
  pub mod hosting {
      pub fn add_to_waitlist() {}
  }
  ```

- 2-layer file

  ```rs
  // src/front_of_house.rs
  pub mod hosting;

  // src/front_of_house/hosting.rs
  pub fn add_to_waitlist() {}
  ```

# 8.1. Storing Lists of Values with Vectors

## Creating a New Vector

- by `Vec::new function`

```rs
let v: Vec<i32> = Vec::new();
```

- by `vec! macro`

```rs
let v = vec![1, 2, 3];
```

## Updating a Vector

- Vector should be `mut`

```rs
let mut v = Vec::new();
v.push(5);
```

## Dropping a Vector Drops Its Elements

- When the vector gets dropped, all of its contents are also dropped

```rs
fn main() {
    {
        let v = vec![1, 2, 3, 4];
        // do stuff with v
    } // <- v goes out of scope and is freed here
}
```

## Reading Elements of Vectors

1. v[i] way

```rs
let v = vec![1, 2, 3, 4, 5];
let third: &i32 = &v[2];
println!("The third element is {}", third);
```

2. v.get(i) way - safer for invalid index

```rs
match v.get(2) {
    Some(third) => println!("The third element is {}", third),
    None => println!("There is no third element."),
}
```

### Can’t have mutable and immutable references in the same scope

- adding a new element onto the end of the vector might require allocating new memory and copying the old elements to the new space

```rs
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0];
v.push(6);
println!("The first element is: {}", first);
```

## Iterating over the Values in a Vector

- for loop to get immutable references

```rs
    let v = vec![100, 32, 57];
    for i in &v {
        println!("{}", i);
    }
```

- mutable

```rs
    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }
```

## Using an Enum to Store Multiple Types

- vectors can only store values that are the same type => use `Enum`

```rs
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

# 8.2. Storing UTF-8 Encoded Text with Strings

- UTF-8 encoded
- Two string types in Rust
  1. Core language => &str (string slice)
  2. Std lib => String

## Creating a New String

- string is also one of vector type

```rs
let mut s = String::new();
```

- literal directly

```rs
let s = "initial contents".to_string();
let s = String::from("initial contents");
```

## Updating a String

### Appending to a String with push_str and push

```rs
let mut s = String::from("foo");
s.push_str("bar"); // string slice
s.push('l'); // single char
```

### Concatenation with the + Operator or the format! Macro

```rs
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used
```

- coerce the &String argument into &str
- deref coercion `&s2` => &s2[..]
- take ownership of `s1`
- appends a copy of the `s2`
- returns ownershop of the result.

### `format!`: templete-literal String

```rs
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{}-{}-{}", s1, s2, s3);
```

## Indexing into Strings

- can't access with index

### Internal Representation

- A String is a wrapper over a `Vec<u8>`

## Slicing Strings

- [] with a range to create a string slice => possible
- if range is at the middle of byte => panic

```rs
let hello = "Здравствуйте";
let s = &hello[0..4];
```

## Methods for Iterating Over Strings

```rs
for c in "नमस्ते".chars() {
    println!("{}", c);
}
```

# 8.3. Storing Keys with Associated Values in Hash Maps

## Creating a New Hash Map

```rs
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

- homogeneous: all of the keys must have the same type, and all of the values must have the same type

### Create Hash map from vectors

```rs
use std::collections::HashMap;
let teams = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];
let mut scores: HashMap<_, _> = // infer <String, i32>
    teams.into_iter().zip(initial_scores.into_iter()).collect();
```

## Hash Maps and Ownership

- `types` that implement the `Copy trait`, like `i32` are copied into the hash map
- `owned value` like `String` moves, the hash map will be the owner

```rs
use std::collections::HashMap;

let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value);
// field_name and field_value are invalid at this point
```

## Accessing Values in a Hash Map

1. `Hashmap.get()`
2. `for (key, value) in &Hashmap {}`

```rs
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
let team_name = String::from("Blue");

let score = scores.get(&team_name);

for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

## Updating a Hash Map

### Overwriting a Value

```rs
use std::collections::HashMap;
let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);
println!("{:?}", scores); // {"Blue": 25}
```

### Only Inserting a Value If the Key Has No Value

#### `or_insert`:

##### if exists:

- returns a mutable reference to the previous value

##### else:

- inserts the parameter
- returns a mutable reference to the new value

```rs
    use std::collections::HashMap;
    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);

    scores.entry(String::from("Yellow")).or_insert(50);
    scores.entry(String::from("Blue")).or_insert(50);
    println!("{:?}", scores); // {"Blue": 10, "Yellow": 50}
```

## Updating a Value Based on the Old Value

```rs
use std::collections::HashMap;
let text = "hello world wonderful world";
let mut map = HashMap::new();
for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
println!("{:?}", map);
```

## Hashing Functions

- SipHash: default function, higher safety
- switch to another function => use `BuildHasher` trait

# 9.1. Unrecoverable Errors with panic!

- recoverable => `Result<T, E>`
  - file not found
- unrecoverable => `panic!`
  - bugs like trying to access a location beyond the end of an array

## panic!

- print a failure message
- unwind and clean up the stack
- quit

### Unwinding the Stack or Aborting in Response to a Panic

- switch from unwind to abort => smaller binary

```rs
// Cargo.toml
[profile.release]
panic = 'abort'
```

## Using a panic! Backtrace

- trace panic! from a library or our code

```sh
$ RUST_BACKTRACE=1 cargo run
```

# 9.2 Recoverable Errors with Result

- Most errors aren’t serious enough to require the program to stop entirely

```rs
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## Matching on Different Errors

### With match pattern

```rs
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => {
                panic!("Problem opening the file: {:?}", other_error)
            }
        },
    };
}
```

### With closure

```rs
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {:?}", error);
            })
        } else {
            panic!("Problem opening the file: {:?}", error);
        }
    });
}
```

### Shortcuts for Panic on Error: unwrap and expect

```rs
let f = File::open("hello.txt").unwrap(); // just panic!()
let f = File::open("hello.txt").expect("Failed to open hello.txt"); // panic!("with message")
```

## Propagating Errors

```rs
#![allow(unused)]
fn main() {
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let f = File::open("hello.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e), // io:Error
    };

    let mut s = String::new();

    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e), // io::Error
    }
}
}
```

### A Shortcut for Propagating Errors: the ? Operator

- similar operation with match but,,
- ? operator calls the `from` function, the error type received is converted into the error type defined in the return type of the current function

```rs
fn read_username_from_file() -> Result<String, io::Error> {
    let mut f = File::open("hello.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

- could even shorten

```rs
fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();

    File::open("hello.txt")?.read_to_string(&mut s)?;

    Ok(s)
}
```

- more shorten

```rs
fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt") // implemented same way inside
}
```

### The ? Operator Can Be Used in Functions That Return Result

- should return `Result` or `Option` or `std::ops::Try`
- To use `?`,
  1. Define return type `Result<T, E>`
  2. One of the Result<T, E> methods - unwrap_or...
  3. Match
- main should return `()` or `Result<T, E>`

```rs
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> { // can define Any error
    let f = File::open("hello.txt")?;

    Ok(())
}
```

# 9.3. To panic! or Not to panic!

## Examples, Prototype Code, and Tests

- Example should be concise
- Prototype should be fast
- Whole Test should be failed when assertion got false

## Cases in Which You Have More Information Than the Compiler

- Use unwrap when you think logiaclly it will never get failure

## Guidelines for Error Handling

- good for panic!
  - If someone passes values that don't make sense
  - Using external library able to return invalid state
- But better to return `Result` first then `panic!`

## Creating Custom Types for Validation

- Typing is better than error handling

  - If use type, we don't need to parse Option, just compile or fail
  - If use `u32`, no need to handle negative case

- eg) For repeative type checker

```rs
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

    pub fn value(&self) -> i32 { // getter to get private value from outside the module
        self.value
    }
}
```

# 10.1. Generic Data Types

- `Generic`: abstract stand-ins for concrete types or other properties
- Preview
  1. review how to extract a function to reduce code duplication
  2. use the same technique to make a generic function from two functions that differ only in the types of their parameters
  3. how to use generic types in struct and enum definitions.
  4. how to use traits to define behavior in a generic way
  5. lifetimes, a variety of generics that give the compiler information about how references relate to each other

## Removing Duplication by Extracting a Function

```rs
fn largest(list: &[i32]) -> i32 {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let result = largest(&number_list);
    println!("The largest number is {}", result);
}
```

1. Identify duplicate code.
2. Extract the duplicate code into the body of the function and specify the inputs and return values of that code in the function signature.
3. Update the two instances of duplicated code to call the function instead.

## In Function Definitions

- the function `largest` is `generic` over some type `T`
- but does not work yet? => need to bound cmp trait

```rs
fn largest<T>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest(&char_list);
    println!("The largest char is {}", result);
}
```

## In Struct Definitions

```rs
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```

- `Point` is a generic over types T and U where x is of type T and y is of type U.
- Can use many generic type parameters but => needs restructuring into smaller pieces.

## In Enum Definitions

```rs
enum Option<T> {
    Some(T),
    None,
}
```

```rs
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## In Method Definitions

- `impl` can be with either methods only on concrete type or generic type
- methods way will impl the method on specific type only.
- `fn` can has different generic with `struct`

```rs
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> { // T, U for struct
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> { // V, W for method
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

## Performance of Code Using Generics

- Rust compiles generic code into code that specifies the type in each instance, we pay no runtime cost for using generics

# 10.2. Traits: Defining Shared Behavior

- A trait tells the Rust compiler about functionality a particular type has and can share with other types
- similar with `interface` of other languages.

## Defining a Trait

- Trait definitions are a way to group method signatures together to define a set of behaviors necessary to accomplish some purpose

```rs
pub trait Summary {
    fn summarize(&self) -> String;
}
```

## Implementing a Trait on a Type

- syntax

```rs
pub trait TraitName {
    fn FunctionName(&self) -> T
}

impl TraitName for TypeName {
    fn FunctionName(&self) -> T {
        ...
    }
}
```

- example

```rs
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

- Summary for NewsArticle and Tweet
- can implement a trait on a type only if eitehr the trait or the type is local to our crate

## Default Implementations

- default: define and implementation at the same time instead of empty impl block

```rs
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

- default + custom impl: custom should be implemented

```rs
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}

impl Summary for Tweet {
    fn summarize_author(&self) -> String {
        format!("@{}", self.username)
    }
}
```

## Traits as Parameters

```rs
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

## Trait Bound Syntax

- verbose version of Traits as Parameters
- good for multiple params with same trait

```rs
pub fn notify<T: Summary>(item1: &T, item2: &T) {
    ...
}
```

## Specifying Multiple Trait Bounds with the + Syntax

```rs
pub fn notify(item: &(impl Summary + Display)) {
```

```rs
pub fn notify<T: Summary + Display>(item: &T) {
```

## Clearer Trait Bounds with where Clauses

```rs
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {
```

```rs
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```

## Returning Types that Implement Traits

```rs
fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```

- good for closures and iterators
- can return only a single type

## Fixing the largest Function with Trait Bounds

- To compare: bound `PartialOrd` trait for T
- Cannot move out of type [T], a non-copy slice => bound `Copy`

```rs
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest(&char_list);
    println!("The largest char is {}", result);
}
```

### Homework: instead of bounding Copy trait, change return type with &T => solve lifetiem problem

## Using Trait Bounds to Conditionally Implement Methods

```rs
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> { // always impl
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> { // impl only it has Display and PartialOrdt trait
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

### Blanket implementation

- go to implementors section for details
- impl for every type T having Display

```rs
impl<T: Display> ToString for T {
    // --snip--
}
```

# 10.3. Validating References with Lifetimes

- Every reference has a lifetime; the scope for which that reference is valid
- Use when the lifetimes of references could be related in a few different ways.

## Preventing Dangling References with Lifetimes

## The Borrow Checker

```rs
fn main() {
    {
        let r;                // ---------+-- 'a
                              //          |
        {                     //          |
            let x = 5;        // -+-- 'b  |
            r = &x;           //  |       |
        }                     // -+       |
                              //          |
        println!("r: {}", r); //          |
    }                         // ---------+
}
```

- `'b` is shorter than `'a`: the subject of the reference doesn't live as long as the reference

```rs
    {
        let x = 5;            // ----------+-- 'b
                              //           |
        let r = &x;           // --+-- 'a  |
                              //   |       |
        println!("r: {}", r); //   |       |
                              // --+       |
    }                         // ----------+
```

- `r` will always be valid while `x` is valid

## Generic Lifetimes in Functions

```rs
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest(x: &str, y: &str) -> &str { // expected named lifetime parameter
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

## Lifetime Annotation Syntax

- Lifetime annotations describe the relationships of the lifetimes of multiple references to each other without affecting the lifetimes.

```rs
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

## Lifetime Annotations in Function Signatures

- Lifetimes are constraints:
  - Reject any values that don't adhere to these constraints
- Doesn't need to know exatly how long `x` and `y` will live, only that some scope can be substituted for `'a`
- Lifetimes might be different each time the function is called => need to annotate manually

```rs
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

- the generic lifetime `'a` will get the concrete lifetime that is equal to the smaller of the lifetimes of `x` and `y`

```rs
fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str()); // borrowed value does not live long enough
    }
    println!("The longest string is {}", result); // borrow later used here
}
```

## Thinking in Terms of Lifetimes

```rs
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
    x
}
```

- `y` does not have any relationship with the lifetime of `x` or `return value`
- If `return type` is `reference`, it needs to match the lifetime param for `one of the params`.
- Else, it must refer to a value created within this function, which would be a `Dangling reference`

```rs
fn longest<'a>(x: &str, y: &str) -> &'a str {
    let result = String::from("really long string");
    result.as_str() // returns a value referencing data owned by the current function
}
```

## Lifetime Annotations in Struct Definitions

- should add a lifetime annotation on every reference in the struct's definition

```rs
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

- an instance of `ImportantExcerpt` can't outlive the reference it holds in its `part` filed
- novel -> first_sentence -> part
- novel doesn't go out of scope until after the `ImportantExcerpt` goes out of scope

## Lifetime Elision

- every reference has a lifetime, need to specify except below rules

1. Each reference param gets its own lifetime param

```rs
fn foo<'a, 'b>(x: &'a i32, y: &'b i32)
```

2. If only one input lifetime param, assign to all output lifetime params

```rs
fn foo<'a>(x: &'a i32) -> &'a i32
```

3. If multiple input lifetime params, but with `&self`; which means method; the lifetime of `&self` is assigned to all output lifetime

```rs
fn longest(x: &str, y: &str) -> &str {
```

- adjust 1st rule

```rs
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &str {
```

- try 2nd rule => fail, it has more than 2 input params
- TRY 3rd rule => fail, it has multiple params but no self => not method => we should manully write lifetime annotation

## Lifetime Annotations in Method Definitions

```rs
impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
}
```

- 1st elision rule for `&self`
- 3rd elision rule for return type

## The Static Lifetime

```rs
let s: &'static str = "I have a static lifetime.";
```

- can live for the entire duration of the program
- all string literal is stored directly in teh program's binray => `'static`

## Generic Type Parameters, Trait Bounds, and Lifetimes Together

```rs
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>( // lifetimes is also a type of generic
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
