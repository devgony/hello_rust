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
