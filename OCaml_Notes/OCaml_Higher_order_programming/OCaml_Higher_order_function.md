# Higher-Order Functions

## **What is a Higher-Order Function?**

A **higher-order function** is a function that:

* **Takes a function as an argument**, and/or
* **Returns a function as a result**.

In OCaml, all functions technically take one argument and return a result, so even partially applied functions are valid higher-order constructs.

---

## Examples of Basic Functions

```ocaml
let double x = 2 * x         (* val double : int -> int *)
let square x = x * x         (* val square : int -> int *)

let quad x = double (double x)        (* val quad : int -> int *)
let fourth x = square (square x)      (* val fourth : int -> int *)
```

---

## **Abstraction via Higher-Order Functions**

You can abstract repeated patterns with a higher-order function:

```ocaml
let twice f x = f (f x)  (* val twice : ('a -> 'a) -> 'a -> 'a *)
```

Use it like:

```ocaml
let quad x = twice double x
let fourth x = twice square x
```

---

# The Abstraction Principle

> **"Avoid stating the same thing more than once."**
> → Instead, **factor out** repeated patterns using **functions**.

Higher-order functions help in:

* **Code reuse**
* **Efficiency**
* **Modularity**
* **Maintainability**

## Meaning of “Higher Order”

* **First-order logic** quantifies over individual elements.
* **Second-order logic** quantifies over properties (sets/functions).
* **Higher-order logic** quantifies over properties of properties, and so on.

Similarly in programming:

* **First-order functions**: take/return data.
* **Higher-order functions**: take/return other functions.


# TL;DR

* **Higher-order functions** make code more modular and reusable.
* Common examples include: `twice`, `compose`, `apply`, `both`, `cond`.
* They reflect **the abstraction principle**: don’t repeat logic—factor it out!
* Analogous to higher-order logic: functions that work on functions.
