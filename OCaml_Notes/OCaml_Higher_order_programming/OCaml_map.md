# `map`

## **What is `map`?**

`map` is a **higher-order function** that:

* Applies a function `f` to **each element** of a list.
* Returns a **new list** with the results.

```ocaml
let rec map f = function
  | [] -> []
  | h :: t -> f h :: map f t
```

**Type**:

```ocaml
map : ('a -> 'b) -> 'a list -> 'b list
```

---

## **Abstraction via `map`**

### Before Abstraction:

```ocaml
let rec add1 = function | [] -> [] | h::t -> (h+1)::add1 t
let rec concat_bang = function | [] -> [] | h::t -> (h^"!")::concat_bang t
```

### After Abstraction:

* Common pattern: operate on each head element and recurse.
* Only the operation `f h` differs.

```ocaml
let rec transform f lst =          (* generic abstraction *)
  match lst with
  | [] -> []
  | h :: t -> f h :: transform f t (* here f h updates the element *)

let map = transform                (* renaming to map *)
```

Now:

```ocaml
let add1 = map (fun x -> x + 1)
let concat_bang = map (fun x -> x ^ "!")
```

---

## **Side Effects and Evaluation Order**

### OCaml evaluates function arguments **right-to-left**.

Problem with naive map:

```ocaml
let rec map f = function | [] -> [] | h::t -> f h :: map f t
```

If `f` has side effects (like printing), evaluation order may be surprising:

```ocaml
let p x = print_int x; x + 1
map p [1;2]  (* prints 2, then 1 *)
```

**Fix using `let` to force left-to-right evaluation:**

```ocaml
let rec map f = function
  | [] -> []
  | h :: t -> let h' = f h in h' :: map f t
```

---

## **Tail Recursion and Performance**

### Attempt 1 (Tail Recursive, but Quadratic Time):

```ocaml
let rec map_tr_aux f acc = function
  | [] -> acc
  | h :: t -> map_tr_aux f (acc @ [f h]) t   (* BAD: append is O(n) *)
```

Result: **O(n²)** time complexity.

### Attempt 2 (Tail Recursive and Linear Time):

```ocaml
let rec rev_map_aux f acc = function
  | [] -> acc
  | h :: t -> rev_map_aux f (f h :: acc) t

let rev_map f lst = rev_map_aux f [] lst
let map f lst = List.rev (rev_map f lst)
```

* Reverses the list after transformation.
* **O(n) time and tail recursive**.

**Lesson**:

* **Cons `::` is fast**, append `@` is slow.
* **Tail recursion ≠ always better** — may trade time for space.


## Final Takeaways

| Concept             | Key Point                                            |
| ------------------- | ---------------------------------------------------- |
| `map`               | Abstracts per-element transformation                 |
| Partial Application | `let add1 = map (fun x -> x + 1)` returns a function |
| Evaluation Order    | Use `let` to enforce side-effect order               |
| Tail Recursion      | Avoid `@`, use cons `::` and reverse                 |
| Cross-Language      | `map` is universal (Python, Java, etc.)              |

> Always prefer OCaml's built-in `List.map` for efficiency and clarity.