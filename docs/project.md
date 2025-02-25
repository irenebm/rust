# Hello, World!

## Creating a Project Directory

You’ll start by making a directory to store your Rust code. 
It doesn’t matter to Rust where your code lives.

```sh
mkdir ~/projects
cd ~/projects
mkdir hello_world
cd hello_world
```

## Writing and Running a Rust Program

Rust files always end with the `.rs` extension. 
If you’re using more than one word in your filename, the convention is to use an underscore to separate them. 
For example, use `hello_world.rs` rather than `helloworld.rs`.

Open the `main.rs` file you just created and enter the code:

```rs
fn main() {
    println!("Hello, world!");
}
```

* It’s good style to place the opening curly bracket on the same line as the function declaration, adding one space in between.
* `println!` calls a Rust macro. If it had called a function instead, it would be entered as `println` (without the `!`). Using a `!` means that you’re calling a macro instead of a normal function and that macros don’t always follow the same rules as functions.

Save the file and go back to your terminal window. 
On Linux or macOS, enter the following commands to compile and run the file:

```sh
rustc main.rs
./main
```

Regardless of your operating system, the string `Hello, world!` should print to the terminal. 

* Before running a Rust program, you must compile it using the Rust compiler by entering the `rustc` command and passing it the name of your source file.
* After compiling successfully, Rust outputs a binary executable.
* Rust is an ahead-of-time compiled language, meaning you can compile a program and give the executable to someone else, and they can run it even without having Rust installed.


---

# Hello, Cargo!

Cargo is Rust’s build system and package manager. 
Cargo handles a lot of tasks for you, such as building your code, downloading the libraries your code depends on, and building those libraries. 

Cargo comes installed with Rust, check whether Cargo is installed by entering the following in your terminal:

```sh
cargo --version
```

## Creating a Project with Cargo

Navigate back to your projects directory and run the following:

```sh
cargo new hello_cargo
cd hello_cargo
```

* The first command creates a new directory and project called `hello_cargo`, and creates its files in a directory of the same name.
* Cargo has generated two files and one directory: a `Cargo.toml` file and a `src` directory with a `main.rs` file inside.
* It has also initialized a new Git repository along with a `.gitignore` file. Git files won’t be generated if you run cargo new within an existing Git repository; you can override this behavior by using `cargo new --vcs=git`.

Open `Cargo.toml`:

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"

[dependencies]
```

This file is in the TOML (Tom’s Obvious, Minimal Language) format, which is Cargo’s configuration format.

* The first line, `[package]`, is a section heading that indicates that the following statements are configuring a package. 
* The next three lines set the configuration information Cargo needs to compile your program: the name, the version, and the edition of Rust to use.
* The last line, `[dependencies]`, is the start of a section for you to list any of your project’s dependencies.

Open `src/main.rs`:

```rs
fn main() {
    println!("Hello, world!");
}
```

* Cargo has generated a “Hello, world!” program for you!
* Cargo expects your source files to live inside the src directory. The top-level project directory is just for README files, license information, configuration files, and anything else not related to your code.
* If you started a project that doesn’t use Cargo you can convert it to a project that does use Cargo. Move the project code into the `src` directory and create an appropriate `Cargo.toml` file. One easy way to get that `Cargo.toml` file is to run `cargo init`, which will create it for you automatically.

## Building and Running a Cargo Project

From your `hello_cargo` directory, build your project by entering the following command:

```sh
cargo build
```

This command creates an executable file in `target/debug/hello_cargo` rather than in your current directory. Because the default build is a debug build, Cargo puts the binary in a directory named debug.

You can run the executable with this command:

```sh
./target/debug/hello_cargo
```

If all goes well, `Hello, world!` should print to the terminal.

* Running `cargo build` for the first time also causes Cargo to create a new file at the top level: `Cargo.lock`. This file keeps track of the exact versions of dependencies in your project. This project doesn’t have dependencies, so the file is a bit sparse. You won’t ever need to change this file manually; Cargo manages its contents for you.

We can also use `cargo run` to compile the code and then run the resultant executable all in one command:

```sh
cargo run
```

* Notice that this time we didn’t see output indicating that Cargo was compiling `hello_cargo`. Cargo figured out that the files hadn’t changed, so it didn’t rebuild but just ran the binary.

Cargo also provides a command called `cargo check`. This command quickly checks your code to make sure it compiles but doesn’t produce an executable:

```sh
cargo check
```

* `cargo check` is much faster than `cargo build` because it skips the step of producing an executable. Run `cargo check` periodically while writing the program to make sure it compiles. Then run `cargo build` when you're ready to use the executable.

## Building for Release

When your project is finally ready for release, you can use `cargo build --release` to compile it with optimizations.
This command will create an executable in *target/release* instead of *target/debug*.