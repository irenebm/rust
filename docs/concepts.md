# Common Programming Concepts

## Variables and Mutability

By default, variables are immutable. 
Rust gives it to you to write your code in a way that takes advantage of the safety and easy concurrency that Rust offers. 

When a variable is immutable, once a value is bound to a name, you can’t change that value.
You receive the error message `cannot assign twice to immutable variable 'x'` when you tried to assign a second value to the immutable `x` variable.

The Rust compiler guarantees that when we state that a value won’t change, it really won’t change, so we don’t have to keep track of it ourself.

We can make variables mutable by adding `mut` in front of the variable name.
Adding `mut` also conveys intent to future readers of the code by indicating that other parts of the code will be changing this variable’s value.

Example:

```rs
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");     // The value of x is: 5
    x = 6;
    println!("The value of x is: {x}");     // The value of x is: 6
}
```

### Constants

Constants are values that are bound to a name and are not allowed to change.

We aren’t allowed to use `mut` with constants. 
Constants aren’t just immutable by default—they’re always immutable.
We declare constants using the `const` keyword instead of `let`, and the type of the value *must* be annotated.

Constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.

Constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

Example:

```rs
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Rust’s naming convention for constants is to use all uppercase with underscores between words.
It helps to have only one place in your code you would need to change if the hardcoded value needed to be updated in the future.

### Shadowing

We can declare a new variable with the same name as a previous variable.
The first variable is *shadowed* by the second.
We can shadow a variable by using the same variable’s name and repeating the use of the `let` keyword:

```rs
fn main() {
    let x = 5;

    let x = x + 1;

    println!("The value of x is: {x}");     // The value of x is: 6
}
```

Shadowing is different from marking a variable as `mut` because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword. 
By using `let`, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed.

The other difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name:

```rs
    let spaces = "   ";
    let spaces = spaces.len();
```

The first `spaces` variable is a string type and the second `spaces` variable is a number type.
If we try to use `mut` for this, as shown here, we’ll get a compile-time error.

## Data types