# Currying
## **Curried vs. Uncurried Functions**

### **Curried Function**

* Takes **one argument at a time**, returning a new function each time.

```ocaml
let add x y = x + y
(* Type: int -> int -> int *)
```

* Supports **partial application** (e.g., `let add5 = add 5`).

---

### **Uncurried Function**

* Takes a **tuple** as its argument.

```ocaml
let add' t = fst t + snd t
(* Type: int * int -> int *)

(* Or with pattern matching: *)
let add'' (x, y) = x + y
```

* Does **not support partial application**

---

## **Terminology**

* **Curried**: `t1 -> t2 -> t3`
* **Uncurried**: `(t1 * t2) -> t3`

---

## **Conversion Between Curried and Uncurried**

```ocaml
let curry f x y = f (x, y)
(* Type: ('a * 'b -> 'c) -> 'a -> 'b -> 'c *)

let uncurry f (x, y) = f x y
(* Type: ('a -> 'b -> 'c) -> ('a * 'b) -> 'c *)
```

### Examples:

```ocaml
let uncurried_add = uncurry add
let curried_add = curry add''
```

---

## **Use Cases**

| Situation                        | Use Curried | Use Uncurried |
| -------------------------------- | ----------- | ------------- |
| Want **partial application**     |  Yes       |  No          |
| Working with **tuples**          |  No        |  Yes         |
| Functional composition/pipelines |  Easy      |  Verbose     |

