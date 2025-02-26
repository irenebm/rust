# Programming a Guessing Game

We’ll implement a classic beginner programming problem: a guessing game. Here’s how it works: the program will generate a random integer between 1 and 100. It will then prompt the player to enter a guess. After a guess is entered, the program will indicate whether the guess is too low or too high. If the guess is correct, the game will print a congratulatory message and exit.

## Setting Up a New Project

```bash
cargo new guessing_game
cd guessing_game
```

## Processing a Guess

The first part of the guessing game program will ask for user input, process that input, and check that the input is in the expected form:

```rs
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

* By default, Rust has a set of items defined in the standard library that it brings into the scope of every program. This set is called the prelude. If a type you want to use isn’t in the prelude, you have to bring that type into scope explicitly with a `use` statement. 
* Using the `std::io` library provides you with a number of useful features, including the ability to accept user input.
The `fn` syntax declares a new function.
* We create a variable to store the user input, like this: `let mut guess = String::new();`. We use the `let` statement to create the variable. 
* In Rust, variables are immutable by default, meaning once we give the variable a value, the value won’t change. To make a variable mutable, we add `mut` before the variable name.
* The equal sign (`=`) tells Rust we want to bind something to the variable now. On the right of the equal sign is the value that guess is bound to, which is the result of calling `String::new`, a function that returns a new instance of a `String`. 
* The `::` syntax in the `::new` line indicates that `new` is an associated function of the `String` type. 
* This `new` function creates a new, empty string.
* We call the `stdin` function from the `io` module, which will allow us to handle user input. If we hadn’t imported the `io` library at the beginning of the program, we could still use the function by writing `std::io::stdin`. The `stdin` function returns an instance of `std::io::Stdin`, which is a type that represents a handle to the standard input for your terminal.
* The `.read_line(&mut guess)` calls the `read_line` method to get input from the user. We’re also passing `&mut guess` as the argument to tell it what string to store the user input in. The full job of `read_line` is to take whatever the user types into standard input and append that into a string (without overwriting its contents). The string argument needs to be mutable so the method can change the string’s content.
* The `&` indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times. References are immutable by default, you need to write `&mut guess` rather than `&guess` to make it mutable.
*  It’s often wise to introduce a newline and other whitespace to help break up long lines when you call a method with the `.method_name()` syntax.
* `read_line` puts whatever the user enters into the string we pass to it, but it also returns a `Result` value. `Result` is an *enumeration* which is a type that can be in one of multiple possible states. We call each possible state a *variant*. The purpose of these `Result` types is to encode error-handling information. `Result’s` variants are `Ok` and `Err`. The Ok variant indicates the operation was successful, and it contains the successfully generated value. The `Err` variant means the operation failed, and it contains information about how or why the operation failed.

