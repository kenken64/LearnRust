# Learn Rust

A collection of examples and notes while learning Rust programming.

## Chapter 1: Getting Started

- `hello_world/` - Basic "Hello, World!" program
- `hello_cargo/` - Introduction to Cargo, Rust's build system and package manager

## Chapter 2: Programming a Guessing Game

### Key Concepts

#### The `let` Keyword

In Rust, `let` is used to declare a variable and bind a value to it.

```rust
let mut guess = String::new();
```

Breaking this down:

- **`let`** — Declares a new variable
- **`mut`** — Makes the variable mutable (by default, Rust variables are immutable)
- **`guess`** — The variable name
- **`String::new()`** — Creates a new empty `String` and binds it to `guess`

##### Immutability by Default

```rust
let x = 5;
x = 6; // ❌ Error! Variables are immutable by default
```

##### Making Variables Mutable

```rust
let mut x = 5;
x = 6; // ✅ Works because of `mut`
```

##### Type Annotations

Rust can infer types, but you can be explicit:

```rust
let guess: String = String::new();
```

##### Shadowing

You can redeclare a variable with the same name:

```rust
let x = 5;
let x = x + 1; // New variable that shadows the previous one
```

#### References and Borrowing (`&`)

The `&` symbol creates a **reference** to a variable, allowing you to borrow it without taking ownership.

```rust
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
```

##### Breaking down `&mut guess`:

| Symbol | Meaning |
|--------|---------|
| `&` | Creates a reference (borrowing) |
| `mut` | Makes the reference mutable |
| `guess` | The variable being referenced |

##### Types of References

1. **Immutable reference `&`** — Can read but not modify:
   ```rust
   let s = String::from("hello");
   let r = &s;  // Can read s through r, but can't modify
   ```

2. **Mutable reference `&mut`** — Can read AND modify:
   ```rust
   let mut s = String::from("hello");
   let r = &mut s;  // Can modify s through r
   r.push_str(" world");
   ```

##### Why Borrowing?

Instead of moving `guess` into `read_line()` (which would make it unusable afterward), you **lend** it temporarily. After `read_line()` finishes, you still own `guess` and can use it.

#### Placeholders `{}`

Placeholders in `println!` are used to insert values into the output string:

```rust
println!("You guessed: {}", guess);
```

- `{}` — A placeholder that gets replaced with the value of `guess`
- Multiple placeholders are filled in order:
  ```rust
  let x = 5;
  let y = 10;
  println!("x = {} and y = {}", x, y);  // Output: x = 5 and y = 10
  ```

#### Error Handling with `Result` and `.expect()`

The `.expect()` method is used for error handling in Rust.

```rust
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
```

##### The `Result` Type

`read_line()` returns a `Result` type, not the actual value. `Result` is an enum with two variants:

| Variant | Meaning |
|---------|---------|
| `Ok(value)` | Operation succeeded, contains the result |
| `Err(error)` | Operation failed, contains error info |

##### What `.expect()` Does

1. **If `Ok`** — Extracts and returns the value inside (number of bytes read)
2. **If `Err`** — Crashes the program and displays your custom message

##### Alternatives to `.expect()`

```rust
// Using .unwrap() - crashes with generic message
.read_line(&mut guess).unwrap();

// Using match - proper error handling
match io::stdin().read_line(&mut guess) {
    Ok(_) => println!("Got input!"),
    Err(e) => println!("Error: {}", e),
}

// Using ? operator (in functions that return Result)
io::stdin().read_line(&mut guess)?;
```

`.expect()` is commonly used in examples and quick prototypes. For production code, you'd typically use `match` or the `?` operator for proper error handling.

#### Using External Crates (`rand`)

External crates (libraries) are added to `Cargo.toml` and imported with `use`.

```rust
use rand::Rng;

let secret_number = rand::thread_rng().gen_range(1..=100);
```

- **`rand::Rng`** — A trait that defines methods for random number generators
- **`rand::thread_rng()`** — Returns a random number generator local to the current thread
- **`gen_range(1..=100)`** — Generates a random number between 1 and 100 (inclusive)

##### Adding a Crate to Cargo.toml

```toml
[dependencies]
rand = "0.8.5"
```

#### Comparing Values with `match` and `Ordering`

The `std::cmp::Ordering` enum is used to compare two values.

```rust
use std::cmp::Ordering;

match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

##### The `Ordering` Enum

| Variant | Meaning |
|---------|---------|
| `Ordering::Less` | First value is less than second |
| `Ordering::Greater` | First value is greater than second |
| `Ordering::Equal` | Both values are equal |

##### The `match` Expression

`match` compares a value against patterns and runs code for the matching arm:

```rust
match value {
    pattern1 => code1,
    pattern2 => code2,
    pattern3 => code3,
}
```

#### Type Conversion with `.parse()`

Convert a string to a number using `.parse()`:

```rust
let guess: u32 = guess
    .trim()
    .parse()
    .expect("Please type a number!");
```

- **`.trim()`** — Removes whitespace and newline characters
- **`.parse()`** — Converts the string to another type (inferred from `u32` annotation)
- **`u32`** — An unsigned 32-bit integer

##### Shadowing for Type Conversion

Notice `guess` is redeclared — this is **shadowing**, which lets you reuse the variable name with a new type:

```rust
let mut guess = String::new();  // guess is a String
let guess: u32 = guess.trim().parse()...;  // guess is now a u32
```

#### Loops with `loop`

The `loop` keyword creates an infinite loop:

```rust
loop {
    println!("Please input your guess.");
    // ... game logic
}
```

- Runs forever until explicitly broken with `break`
- Useful for games or programs that need to repeat until a condition is met
