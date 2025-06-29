# Algebraic Data Types

## Variant Types with Data (Tagged Unions)

```ocaml
type point = float * float

type shape =
  | Point of point
  | Circle of point * float
  | Rect of point * point
```

- **Each constructor (tag)** can carry **custom data**.
- This is also known as a **tagged union** or **sum type**.

## Example Usage

```ocaml
let area = function
  | Point _ -> 0.0
  | Circle (_, r) -> Float.pi *. (r ** 2.0)
  | Rect ((x1, y1), (x2, y2)) -> (x2 -. x1) *. (y2 -. y1)

let center = function
  | Point p -> p
  | Circle (p, _) -> p
  | Rect ((x1, y1), (x2, y2)) -> ((x1 +. x2) /. 2.0, (y1 +. y2) /. 2.0)
```

---

## Sum + Product Types

- **Sum**: you get one constructor out of many.
- **Product**: each constructor may carry multiple values (tuples or records).
- Together these form **Algebraic Data Types** (ADTs).

---

## Custom Union Examples

```ocaml
type string_or_int =
  | String of string
  | Int of int
```

Used in lists:

```ocaml
type string_or_int_list = string_or_int list

let rec sum = function
  | [] -> 0
  | String s :: t -> int_of_string s + sum t
  | Int i :: t -> i + sum t
```

---

## Discriminating Tags

```ocaml
type t = Left of int | Right of int

let double_right = function
  | Left i -> i
  | Right i -> 2 * i
```

---

## Syntax Summary

```ocaml
type t = C1 [of t1] | C2 [of t2] | ...
```

- Constructors can be **constant** or **non-constant**.
- Expressions: `C e` or `C`

### Semantics

- `C e ==> C v` if `e ==> v`
- `C : t` if declared in `type t = ...`

### Pattern Match

```ocaml
match x with
| C1 p -> ...
| C2 p -> ...
```

- Adds new pattern form `C p`

---

## Avoid Catch-All Cases

```ocaml
match color with
| Blue -> "blue"
| _ -> "red"   (* ⚠ will incorrectly match Green too *)
```

Safer version:

```ocaml
match color with
| Blue -> "blue"
| Red -> "red"
(* Compiler warns if new variants like Green are added! *)
```

---

## Recursive Variants

```ocaml
type intlist = Nil | Cons of int * intlist
```

Functions:

```ocaml
let rec sum = function | Nil -> 0 | Cons (h, t) -> h + sum t
let rec length = function | Nil -> 0 | Cons (_, t) -> 1 + length t
```

---

## Mutually Recursive Types

```ocaml
type node = { value : int; next : mylist }
and mylist = Nil | Node of node
```

> Cyclic type synonyms are disallowed: `type t = t * t`

---

## Parameterized Variants (Polymorphic Types)

```ocaml
type 'a mylist = Nil | Cons of 'a * 'a mylist

let rec length = function
  | Nil -> 0
  | Cons (_, t) -> 1 + length t

let empty = function
  | Nil -> true
  | Cons _ -> false
```

- `'a` makes the type **polymorphic** — works for any element type.
- Example: `int mylist`, `string mylist`

```ocaml
type ('a, 'b) pair = { first : 'a; second : 'b }
let x = { first = 2; second = "hello" }
```

---

## Polymorphic Variants

```ocaml
let f = function
  | 0 -> `Infinity
  | 1 -> `Finite 1
  | n -> `Finite (-n)
```

- Use backticks (\`) for **anonymous variant constructors**.
- Type: `[> `Finite of int | `Infinity ]`
- You don’t need to declare them ahead of time.

Pattern match:

```ocaml
match f 3 with
| `NegInfinity -> "negative infinity"
| `Finite n -> "finite"
| `Infinity -> "infinite"
```

> Flexible and often used in libraries.

---

## Built-in Variants

```ocaml
(* Lists *)
type 'a list = [] | (::) of 'a * 'a list

(* Options *)
type 'a option = None | Some of 'a
```

- OCaml's lists and options are recursive, parameterized variants under the hood.

---

