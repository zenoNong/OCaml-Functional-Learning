# Variants

Variants are data types where a value can be **one of several possibilities**, like enums in other languages.

## Defining Variants & Constructing Values

```ocaml
let x = 5     (* val x : int = 5, similar to this *)
type day = Mon | Tue |Wed | Thur | Fri | Sat;;
let d = Mon;; (*val d : day = Mon*)
```

- Each option (`Sun`, `Mon`, ...) is a **`constructor`** and name must start with uppercase.
  ```ocaml
  type shape =
    | Circle of float
    | Rectangle of float * float

  let s1 = Circle 3.0 (* val s1 : shape = Circle 4.*)
  (* Here, Circle and Rectangle are constructors with arguments, used to distinguish not just the form of the data, but also what kind of data. *)
  ```

## Accessing Variants: Pattern Matching

```ocaml
let int_of_day d =
  match d with
  | Sun -> 1
  | Mon -> 2
  | Tue -> 3
  | Wed -> 4
  | Thu -> 5
  | Fri -> 6
  | Sat -> 7
  (*OCaml **does not** automatically map constructors to ints.*)
  let match_shape = function 
    | Circle r -> "Circle"
    | Rectangle (x,y) -> "Rectangle";;
```


## Scope

- Later type definitions **shadow** earlier ones.

```ocaml
type t1 = C | D
type t2 = D | E
let x = D    (* x : t2 *)
```

