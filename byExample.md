## type_of

```rs
use std::any::type_name;

fn type_of<T>(_: T) -> &'static str {
    type_name::<T>()
}
```

## handle `Option` (3way)

```rs
if let Some(var) = option_var {
}
```

```rs
match option_var {
    Some(var) => {},
    None => {},
}
```

```rs
option_var.ok_or_else(|| Err(..))?
// Option       Result           return Error
```

## while let

```rs
// rustlings-option2
fn main() {
    let optional_word = Some(String::from("rustlings"));
    // TODO: Make this an if let statement whose value is "Some" type
    if let Some(word) = optional_word {
        println!("The word is: {}", word);
    } else {
        println!("The optional word doesn't contain anything");
    }

    let mut optional_integers_vec: Vec<Option<i8>> = Vec::new();
    for x in 1..10 {
        optional_integers_vec.push(Some(x));
    }

    // TODO: make this a while let statement - remember that vector.pop also adds another layer of Option<T>
    // You can stack `Option<T>`'s into while let and if let
    while let Some(integer) = optional_integers_vec.pop() {
        if let Some(integer) = integer {
            println!("current value: {}", integer);
        }
    }
}
```

## Some(ref v) to avoid copy error

```rs
// rustlings-option3
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match y {
        Some(ref p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => println!("no match"),
    }
    y; // Fix without deleting this line.
}
```
