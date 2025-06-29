# Variants

Variants are data types where a value can be **one of several possibilities**, like enums in other languages.

## Defining Variants & Constructing Values

```ocaml
type day = Mon | Tue |Wed | Thur | Fri | Sat;;
let d = Mon;; (*val d : day = Mon*)
```

- Each option (`Sun`, `Mon`, ...) is a **constructor**.
- Constructor names **must start with uppercase** letters.

## Accessing Variants — Pattern Matching

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
```


## Semantics

- **Dynamic**: A constructor is a value (no computation).
- **Static**: If `t = ... | C | ...`, then `C : t`

## Scope

- Later type definitions **shadow** earlier ones.

```ocaml
type t1 = C | D
type t2 = D | E
let x = D    (* x : t2 *)
```

