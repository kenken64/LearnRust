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
