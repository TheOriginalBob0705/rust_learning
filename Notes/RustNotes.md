# Syntax - Basic

Every program in Rust starts in the `main` function:
```rust
fn main() {
  ...
}
```

Methods in Rust have `!` after them:
```rust
fn main() {
  println!("Hello world!");
}
```

Variables are assigned using `let`

Print to standard output by `print!()` or `println!()`
- `println!()` adds a newline at the end of output

Scope is defined by the **block of code** in which it's declared

Functions are named blocks of code that are reusable

**Shadowing** allows a variable to be re-declared in the **same scope** with the **same name**

## Binding and Mutability

1. A variable can only be used if it has been initialized

```
fn main() {
  let x: i32; // Uninitialized but used, ERROR!
  let y: i32; // Uninitialized but unused, only a warning!

  assert_eq!(x, 5);
  println!("Success!");
}
```

This needs to be fixed, as shown below:
```
fn main() {
  let x: i32 = 5; // Uninitialized but used, ERROR!
  let y: i32; // Uninitialized but unused, only a warning!

  assert_eq!(x, 5);
  println!("Success!");
}
```

Use `mut` to mark a variable as mutable:
```
fn main() {
  let mut x = 1;
  x += 2;

  assert_eq!(x, 3);
  println!("Success!");
}
```

## Scope

A scope is the range within the program for which the item is valid.

```
fn main() {
  let x: i32 = 10;

  {
    let y: i32 = 5;
    println!("The value of x is {} and the value of y is {}", x, y);
  }

  println!("The value of x is {} and the value of y is {}", x, y);
}
```

This will not compile. The fixed code is:
```
fn main() {
  let x: i32 = 10;
  let y: i32 = 5;

  {
    println!("The value of x is {} and the value of y is {}", x, y);
  }

  println!("The value of x is {} and the value of y is {}", x, y);
}
```

Here is another exercise:
```
fn main() {
  println!("{}, world", x);
}

fn define_x() {
  let x: &str = "hello";
}
```

And here is how to fix it:
```
fn main() {
  define_x();
}

fn define_x() {
  let x: &str = "hello";
  
  println!("{}, world", x);
}
```

## Shadowing

You can declare a new variable with the same name as a previous variable, here we can say the first one is shadowed by the second one:
```
fn main() {
  let x: i32 = 5;

  {
    let x = 12;
    assert_eq!(x, 12);
  }

  assert_eq!(x, 5);

  let x: i32 = 42;
  println!("{}", x);
}
```
Note that in the inner scope, the x holds a value of 12, while in the main scope it holds a value of 5 still. The x declared in the main scope does not care about the x declared in the inner scope

Here is another exercise:
```
fn main() {
  let mut x: i32 = 1;
  x = 7;
  // Shadowing and re-binding
  let x = x;
  x += 3;

  let y: i32 = 4;
  // Shadowing
  let y: &str = "I can also be bound to text!";

  println!("Success!");
}
```
This code will not compile. Notice that when we re-initialized x by using `x = x;`, we took away its mutable state because we did not use the `mut` keyword. So we need to remove the `let x = x;` line for this to compile.

But why do we shadow? Why not just place a assign a new value to the variable?

Well, look at y. We can assign a value of a completely different type to the variable by using shadowing.

# Unused variables
```
fn main() {
  let x = 1;
}

// Warning: unused variable: 'x'
```

There are two ways to 'solve' this. First, use an underscore:
```
fn main() {
  let _x = 1;
}
```

Or, use a tag to allow unused variables:
```
#[allow(unused_variables)]

fn main() {
let x = 1;
}
```

## Destructuring

We can use a pattern with `let` to destructure a tuple to separate variables

For now, think of tuples as data structures that can hold data of various types.
```
fn main() {
  let (x, y) = (1, 2);
  x += 2;

  assert_eq!(x, 3);
  assert_eq!(y, 2);
  
  println!("Success!");
}
```

Again, x is immutable here. You need to explicitly state whether a variable is mutable with `mut`:
```
fn main() {
  let (mut x, y) = (1, 2);
  x += 2;

  assert_eq!(x, 3);
  assert_eq!(y, 2);
  
  println!("Success!");
}
```

Introduced in Rust 1.59, you can use tuple, slice, and struct patterns as the left-hand side of an assignment:
```
fn main() {
  let (x, y);

  (x,..) = (3, 4);
  [.., y] = [1, 2];

  assert_eq!([x,y], [3, 2]);

  println!("Success!");
}
```