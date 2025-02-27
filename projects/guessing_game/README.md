# Programming a Guessing Game

We’ll implement a classic beginner programming problem: a guessing game. 

Here’s how it works: the program will generate a random integer between 1 and 100. 
It will then prompt the player to enter a guess. 
After a guess is entered, the program will indicate whether the guess is too low or too high. 
If the guess is correct, the game will print a congratulatory message and exit.

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

* By default, Rust has a set of items defined in the standard library that it brings into the scope of every program. This set is called the *prelude*. If a type you want to use isn’t in the prelude, you have to bring that type into scope explicitly with a `use` statement. 
* Using the `std::io` library provides you with a number of useful features, including the ability to accept user input.
* The `fn` syntax declares a new function.
* We create a variable to store the user input, like this: `let mut guess = String::new();`. 
    * We use the `let` statement to create the variable. 
    * In Rust, variables are immutable by default, meaning once we give the variable a value, the value won’t change. To make a variable mutable, we add `mut` before the variable name.
    * The equal sign (`=`) tells Rust we want to bind something to the variable now. On the right of the equal sign is the value that guess is bound to, which is the result of calling `String::new`, a function that returns a new instance of a `String`. 
    * The `::` syntax in the `::new` line indicates that `new` is an associated function of the `String` type. 
    * This `new` function creates a new, empty string.
* We call the `stdin` function from the `io` module, which will allow us to handle user input. 
    * If we hadn’t imported the `io` library at the beginning of the program, we could still use the function by writing `std::io::stdin`. 
    * The `stdin` function returns an instance of `std::io::Stdin`, which is a type that represents a handle to the standard input for your terminal.
* The `.read_line(&mut guess)` calls the `read_line` method to get input from the user. 
    * We’re also passing `&mut guess` as the argument to tell it what string to store the user input in. 
        * The `&` indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times. 
            * References are immutable by default, you need to write `&mut guess` rather than `&guess` to make it mutable.
    * The full job of `read_line` is to take whatever the user types into standard input and append that into a string (without overwriting its contents). The string argument needs to be mutable so the method can change the string’s content.
    * `read_line` also returns a `Result` value. 
        * `Result` is an *enumeration* which is a type that can be in one of multiple possible states. We call each possible state a *variant*. The purpose of these `Result` types is to encode error-handling information. `Result’s` variants are `Ok` and `Err`.
            1. The `Ok` variant indicates the operation was successful, and it contains the successfully generated value. 
            2. The `Err` variant means the operation failed, and it contains information about how or why the operation failed.
        * `Result` has an `expect` method that you can call. If this instance of `Result` is an `Err` value, `expect` will cause the program to crash and display the message that you passed as an argument to `expect`. If this instance is an `Ok` value, `expect` will take the return value that `Ok` is holding and return that value to you so you can use it. If you don’t call expect, the program will compile, but you’ll get a warning.
* `println!("You guessed: {}", guess);` prints the string that now contains the user’s input. The {} set of curly brackets is a placeholder: think of {} as little crab pincers that hold a value in place. 
*  It’s often wise to introduce a newline and other whitespace to help break up long lines when you call a method with the `.method_name()` syntax.

At this point, the first part of the game is done: we’re getting input from the keyboard and then printing it.

## Generating a Secret Number

We need to generate a secret number that the user will try to guess. The secret number should be different every time so the game is fun to play more than once. We’ll use a random number between 1 and 100 so the game isn’t too difficult. Rust doesn’t yet include random number functionality in its standard library. However, the Rust team does provide a `rand` `crate` with said functionality.

A `crate` is a collection of Rust source code files. The project we’ve been building is a *binary* `crate`, which is an executable. The `rand` `crate` is a *library* crate, which contains code that is intended to be used in other programs and can’t be executed on its own.

We need to modify the `Cargo.toml` file to include the `rand` `crate` as a dependency:

```toml
[dependencies]
rand = "0.8.5"
```

In the `Cargo.toml` file, everything that follows a header is part of that section that continues until another section starts.

* The specifier `0.8.5` is actually shorthand for `^0.8.5`, which means any version that is at least `0.8.5` but below `0.9.0`. Cargo considers these versions to have public APIs compatible with version `0.8.5`, and this specification ensures you’ll get the latest patch release that will still compile with the code in this chapter. Any version `0.9.0` or greater is not guaranteed to have the same API as what the examples use.
* When we include an external dependency, Cargo fetches the latest versions of everything that dependency needs from the registry, which is a copy of data from [Crates.io](https://crates.io/), where people in the Rust ecosystem post their open source Rust projects for others to use.
* After updating the registry, Cargo checks the `[dependencies]` section and downloads any crates listed that aren’t already downloaded. 

Cargo has a mechanism that ensures you can rebuild the same artifact every time you or anyone else builds your code: Cargo will use only the versions of the dependencies you specified until you indicate otherwise.
To handle this, Rust creates the `Cargo.lock` file the first time you run `cargo build`, Cargo figures out all the versions of the dependencies that fit the criteria and then writes them to the `Cargo.lock` file. 
When you build your project in the future, Cargo will see that the `Cargo.lock` file exists and will use the versions specified there.
This lets you have a reproducible build automatically.

When you do want to update a crate, Cargo provides the command `update`, which will ignore the `Cargo.lock` file and figure out all the latest versions that fit your specifications in `Cargo.toml`. 
Cargo will then write those versions to the `Cargo.lock` file. 
In this case, Cargo will only look for versions greater than `0.8.5` and less than `0.9.0`.
To use `0.9.0` or any version in the `0.9.x` series, you’d have to update the `Cargo.toml` file.

The next step is to update `src/main.rs`:

```rs
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```

* The `Rng` trait defines methods that random number generators implement.
* We call the `rand::thread_rng` function that gives us the particular random number generator we’re going to use.
* We call the `gen_range` method that takes a range expression as an argument and generates a random number in the range. The kind of range expression we’re using here takes the form `start..=end` and is inclusive on the lower and upper bounds.
* The second new line prints the secret number. This is useful while we’re developing the program to be able to test it, but we’ll delete it from the final version. 

Each crate has documentation with instructions for using it.
The `cargo doc --open` command will build documentation provided by all your dependencies locally and open it in your browser.

## Comparing the Guess to the Secret Number

Now that we have user input and a random number, we can compare them:

```rs
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

*  The `Ordering` type is another enum and has the variants `Less`, `Greater`, and `Equal`.
* The `cmp` method compares two values and can be called on anything that can be compared, here it’s comparing `guess` to `secret_number`. Then it returns a variant of the `Ordering` enum.
* We use a `match` expression to decide what to do next based on which variant of `Ordering` was returned from the call to `cmp`. A `match` expression is made up of *arms*. An *arm* consists of a pattern to match against, and the code that should be run if the value given to `match` fits that arm’s pattern. The `match` expression ends after the first successful match.

The code won’t compile yet:

```bash
error[E0308]: mismatched types
  --> src/main.rs:24:21
   |
24 |     match guess.cmp(&secret_number) {
   |                 --- ^^^^^^^^^^^^^^ expected `&String`, found `&{integer}`
   |                 |
   |                 arguments to this method are incorrect
   |
   = note: expected reference `&String`
              found reference `&{integer}`
```

The core of the error is that Rust cannot compare a string and a number type.
Rust has a strong, static type system. 
However, it also has type inference. 
When we wrote `let mut guess = String::new()`, Rust was able to infer that guess should be a `String`.
The `secret_number`, on the other hand, is a number type. 
A few of Rust’s number types can have a value between 1 and 100: `i32`, a 32-bit number; `u32`, an unsigned 32-bit number; `i64`, a 64-bit number; etc.
Unless specified, Rust defaults to an `i32`.

We want to convert the `String` the program reads as input into a number type so we can compare it:

```rs
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

We create a variable named `guess`. 
Rust allows us to *shadow* the previous value of `guess` with a new one. 
*Shadowing* lets us reuse the `guess` variable name rather than forcing us to create two unique variables, such as `guess_str` and `guess`.
This feature is often used when you want to convert a value from one type to another type.

* The `trim` method on a `String` instance will eliminate any whitespace at the beginning and end.
* The `parse` method on strings converts a string to another type. We need to tell Rust the exact number type we want by using `let guess: u32`. 
    * The colon (`:`) after guess tells Rust we’ll annotate the variable’s type.
    * The `parse` method will only work on characters that can logically be converted into numbers.

## Allowing Multiple Guesses with Looping

The `loop` keyword creates an infinite loop. 
We’ll add a `loop` to give users more chances at guessing the number:

```rs
    // --snip--

    println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
}
```

As you can see, we’ve moved everything from the `guess` input prompt onward into a loop. 
Be sure to indent the lines inside the loop. 
The program will now ask for another `guess` forever.
The user could always interrupt the program by using the keyboard shortcut `ctrl-c`, but we want the game to also stop when the correct number is guessed:

```rs
        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

Adding the `break` line after `You win!` makes the program exit the `loop` when the user guesses the secret number correctly. 

To further refine the game’s behavior, rather than crashing the program when the user inputs a non-number, let’s make the game ignore a non-number so the user can continue guessing:

```rs
        // --snip--

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        // --snip--
```

* We switch from an `expect` call to a `match` expression to move from crashing on an error to handling the error.
* If `parse` is able to successfully turn the string into a number, it will return an `Ok` value that contains the resultant number.
* If parse is not able to turn the string into a number, it will return an `Err` value that contains more information about the error. 
    * The underscore, `_`, is a catchall value; we’re saying we want to match all `Err` values, no matter what information they have inside them, so the program will execute the second arm’s code, `continue`, which tells the program to go to the next iteration of the `loop` and ask for another guess. 

Let’s delete the `println!` that outputs the secret number.

```rs
use rand::Rng;
use std::cmp::Ordering;
use std::io;


fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");


        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

We’ve successfully built the guessing game. 
Congratulations!
