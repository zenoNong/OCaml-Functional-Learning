# `fold`
## **What is Fold?**

* `fold` **reduces** a list to a single value by **combining** its elements.
* Generalizes operations like **sum**, **concat**, etc.
* Two main versions: `fold_right` and `fold_left`.

---

# `combine` (Foundation for Fold)

```ocaml
let rec combine op init = function
  | [] -> init
  | h :: t -> op h (combine op init t)
```

* General pattern for recursive combining.

* Can define:

  ```ocaml
  let sum = combine ( + ) 0
  let concat = combine ( ^ ) ""
  ```

* Conceptual: `[a; b; c]` becomes `a op (b op (c op init))`

---

# `fold_right`

```ocaml
let rec fold_right f lst acc =
  match lst with
  | [] -> acc
  | h :: t -> f h (fold_right f t acc)
```

* **Processes list from right to left**
* Example: `fold_right ( + ) [1;2;3] 0` → `1 + (2 + (3 + 0))`
* Not tail-recursive

**Use when combining from right matters**, e.g., list construction.

---

# Tail Recursion & `combine_tr`

```ocaml
let rec combine_tr f acc = function
  | [] -> acc
  | h :: t -> combine_tr f (f acc h) t
```

* Processes list **left to right**
* **Tail-recursive**
* Different behavior for non-commutative operations like `-`:

  * `combine ( - ) 0 [3;2;1]` → `3 - (2 - (1 - 0))` → `2`
  * `combine_tr ( - ) 0 [3;2;1]` → `(((0 - 3) - 2) - 1)` → `-6`

---

# `fold_left`

```ocaml
let rec fold_left f acc = function
  | [] -> acc
  | h :: t -> fold_left f (f acc h) t
```

* Standard library version of `combine_tr`
* Tail-recursive and left-associative
* Example:

  ```ocaml
  let sum = fold_left ( + ) 0
  let concat = fold_left ( ^ ) ""
  ```

---

# fold\_left vs. fold\_right

| Feature             | `fold_left`                     | `fold_right`                       |
| ------------------- | ------------------------------- | ---------------------------------- |
| Direction           | Left-to-right                   | Right-to-left                      |
| Tail-recursive      | Yes                             | No                                 |
| Argument Order      | `f acc x`                       | `f x acc`                          |
| Suitable For        | Accumulating (e.g., sum)        | Constructive recursion (e.g., map) |
| Commutativity Safe? | Only if operator is commutative | Depends on operation               |

Use **`ListLabels`** module for clarity with labeled arguments:

```ocaml
ListLabels.fold_left ~f:(fun acc x -> ...) ~init:... list
ListLabels.fold_right ~f:(fun x acc -> ...) ~init:... list
```

---

# Labeled Fold (Custom)

You can define fold with fully labeled arguments:

```ocaml
let rec fold_left ~op:(f: acc:'a -> elt:'b -> 'a) ~init:acc lst = ...
```

But standard operators like `( + )` can’t be passed directly unless redefined with labels.

---

# Fold is Powerful – Can Implement:

```ocaml
let length lst = List.fold_left (fun acc _ -> acc + 1) 0 lst
let rev lst = List.fold_left (fun acc x -> x :: acc) [] lst
let map f lst = List.fold_right (fun x acc -> f x :: acc) lst []
let filter f lst = List.fold_right (fun x acc -> if f x then x :: acc else acc) lst []
```

**Note**:

* Fold helps abstract recursion, but may sacrifice **readability**.
* OCaml’s standard library prefers direct recursion for clarity.

---

# Comparing Recursive, Fold, and Library Functions

Example: `lst_and : bool list -> bool` (returns true if all elements are true)

| Approach         | Code                                           | Lazy?                          |       |
| ---------------- | ---------------------------------------------- | ------------------------------ | ----- |
| Recursive        | \`let rec lst\_and\_rec = function \[] -> true | h::t -> h && lst\_and\_rec t\` | Yes Yes |
| Fold             | `let lst_and_fold = List.fold_left (&&) true`  | No No                           |       |
| Library Function | `let lst_and_lib = List.for_all (fun x -> x)`  | Yes Yes                          |       |

---

# Final Takeaways

* **`fold_right`**: Non-tail-recursive, right-to-left
* **`fold_left`**: Tail-recursive, left-to-right
* Choose wisely based on **efficiency**, **operation type**, and **readability**
* Folding enables compact, reusable implementations — but don’t overuse at cost of clarity
