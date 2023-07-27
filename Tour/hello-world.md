# Hello, World!

Tradition says language guides should start with a program that displays "Hello, World!" on the screen. Let's oblige!

```
public fun main() {
  print("Hello, World!")
}
```

Every program in Val must define a `main` function, with no parameters and no return value, as its entry point. Here, `main` contains single statement, which is a call to a global function `print` with a string argument.

_The standard library vends an API to interact with the program's environment._ _Command line arguments are accessed by reading a constant named `Environment.arguments`._ _Return statuses are signaled by calling the global function `exit(status:)`._

To run this program:

* Copy that `main` function into a file called `Hello.val`.
* Run the command `valc Hello.val -o hello`.
* Run the command `./hello` to run the executable.
