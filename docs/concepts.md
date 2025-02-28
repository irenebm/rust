# Common Programming Concepts

## Variables and Mutability

By default, variables are immutable. 
This is one of many nudges Rust gives you to write your code in a way that takes advantage of the safety and easy concurrency that Rust offers. 
Let’s explore how and why Rust encourages you to favor immutability and why sometimes you might want to opt out.

When a variable is immutable, once a value is bound to a name, you can’t change that value.

You receive the error message `cannot assign twice to immutable variable 'x'` when you tried to assign a second value to the immutable `x` variable.

## Constants