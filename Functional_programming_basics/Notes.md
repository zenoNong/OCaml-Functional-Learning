# Functional Programming

## Introduction
Functional programming is a programming paradigm that treats computation as 
the evaluation of mathematical functions and avoids changing state and mutable data.

## Key Concepts
- Pure Function
- Immutability
- First-Class Functions
- Higher-Order Functions
- Recursion
- Referential Transparency
  
- Pure Function
- Immutability
- First-Class Functions
- Higher-Order Functions
- Recursion
- Referential Transparency
  
### 1. Pure Functions
- Same Input -> Same Output(Determinism), No side effects i.e. no global variable modification, no R/W file, no print log, no mutate input.
- Note: Impure break at least one of these two rules. 
- Example:
```ocaml
let add x y = x + y  (* Pure function *)
let get_ramdom () = Random.random() (* Just pseudo, to show impure*)
```

### 2. Immutability
- Data cannot be changed once created, instead we create new data structures
- Helps prevent bugs and makes code easier to reason about

### 3. First-Class Functions
- Functions can be used just like any other *data value*. It can be 
  - Assigned to variables
  - Passed as arguments to function
  - Returned from other functions
  - Put in a data structeru
```ocaml
let apply f x = f x  (* Function as parameter *)
let add x y = x + y (* Define normal function*)
let fn = add        (* Store in a varaible*)
let result = fn 2 3 (* Use the variable as a function*)
```

### 4. Higher-Order Functions
- Functions that take other functions as arguments
- Common examples:map, filter, reduce, fold, etc
```ocaml
let numbers = [1; 2; 3; 4; 5]
let doubled = List.map (fun x -> x * 2) numbers
let apply_twice f x = f (f x)  (* It means:  “Take a function f, apply it once to x, then apply f again to that result.”*)
let square x = x * x
let result = apply_twice square 2 (* here function square is taken as an argument for the other function apply_twice*)
```

### 5. Recursion
- Primary looping construct in functional programming. There is no "for" oro "while" loop here.
- Example:
```ocaml
let rec factorial n =
  match n with
  | 0 -> 1
  | n -> n * factorial (n-1)
```
### 6. Referential Transparency
- Variables once assigned don't change value throughout the program.
Infact, functional programming have assignment statement.
- Example:
```ocaml
x = x + 1 (* as this expression change the value assigned to x it is not referentially transparent*)
```

## Benefits of Functional Programming

1. **Predictability**
   - Easier to test and debug
   - No side effects
   - Deterministic outcomes

2. **Concurrency**
   - Immutable data prevents race conditions
   - Easier to implement parallel operations

3. **Modularity**
   - Functions are independent units
   - Easy to compose and reuse

## Common Functional Programming Patterns

### Pattern Matching
```ocaml
type shape =
  | Circle of float
  | Rectangle of float * float

let area = function
  | Circle r -> 3.14 *. r *. r
  | Rectangle (w, h) -> w *. h
```

### Option Types
- Handle absence of values safely
```ocaml
type 'a option = None | Some of 'a
```

### Function Composition
- Building complex functions from simple ones
```ocaml
let compose f g x = f (g x)
```

## Common Functional Programming Languages

- OCaml
- Haskell
- Erlang
- F#
- Scala (hybrid)
- Clojure

## References

- [OCaml Documentation](https://ocaml.org/docs/)
- [Real World OCaml](https://dev.realworldocaml.org/)
- [Functional Programming: Concepts and Examples](https://ocaml.org/learn/tutorials/)
