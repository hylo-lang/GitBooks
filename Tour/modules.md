# Modules

A Hylo program is composed of **modules**, each of which is composed of one or more files. A module that defines a public `main` function is called an **entry module**, of which there must be exactly one per program.

The [Hello, World!](hello-world.md) program we wrote earlier is made of two modules. The first contains the `hello.hylo` file and is the program's entry module. The second is Hylo's standard library, which is always implicitly imported, and defines commonly-used components like the `Int` and `String` types, and the `print` function used in `hello.hylo`.

Each module defines an API resilience boundary: only public declarations are visible outside the module, and changes to non-public declarations, or to the bodies of public functions in the module cannot cause code outside the module to fail compilation. A module _may_ also define an ABI resilience boundary, within which code and details such as type layout are never encoded into other compiled modules (e.g. via inlining).

### Bundling files

You can bundle multiple files in a single module by passing all of them as arguments to `hc`. For example, in a separate file we can define a function, that prints a specialized greeting, and call it from `Hello.hylo`:

```hylo
// In `Hello.hylo`
public fun main() {
  greet("World")
}

// In `Greet.hylo`
fun greet(_ name: String) {
  print("Hello, ${name}!")
}
```

Here, we declare a function `greet` that takes a single argument of type `String`. The underscore (i.e., `_`) before the parameter name means that arguments passed here must be unlabeled. We'll come back to argument labels later.

To run this program:

* Run the command `hc Hello.hylo Greet.hylo -o hello`
* Run the command `./hello`

_Alternatively, you can put both source files in a subdirectory, say `Sources/`, and compile the program with `hc Sources -o hello`._

Note that `greet` need not be `public` to be visible from another file in the module. All entities declared at the top level of a file are visible everywhere in a module, but not beyond that module's boundary.

### Bundling modules

The simplest way to work with multiple modules is to gather source files into different subdirectories. For example, let's move `greet` in a different module, using the following directory tree:

```
Sources
  |- Hello
  |  |- Hello.hylo
  |- Greetings
  |  |- Greetings.hylo
```

Let's also slightly modify both source files:

```hylo
// In `Sources/Hello/Hello.hylo`
import Greetings
public fun main() {
  greet("World")
}

// In `Sources/Greetings/Greetings.hylo`
public fun greet(_ name: String) {
  print("Hello, ${name}!")
}
```

The statement `import Greetings` at the top of `Hello.hylo` tells the compiler it should import the module `Greetings` when it compiles that source file. Implicitly, that makes `Greetings` a dependency of `Hello`.

Notice that `greet` had to be made public so it could be visible to other modules. As such, it can be called from `Hello.hylo`.

To run this program:

* Run the command `hc --modules Sources/Hello Sources/Greetings -o hello`
* Run the command `./hello`
