# Installation

Install Rust using `rustup`:

```sh
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

The command downloads a script and starts the installation of the rustup tool, which installs the latest stable version of Rust.

After the installation, you should see the following message:

```sh
Rust is installed now. Great!
```

You will also need a linker, which is a program that Rust uses to join its compiled outputs into one file. Linux users should generally install `GCC` or `Clang`, according to their distribution’s documentation.

To check whether you have Rust installed correctly, enter:

```sh
rustc --version
```

# Updating

To update Rust, run the following command:

```sh
rustup update
```

# Uninstalling

To uninstall Rust and `rustup`, run the following uninstall script from your shell:

```sh
rustup self uninstall
```