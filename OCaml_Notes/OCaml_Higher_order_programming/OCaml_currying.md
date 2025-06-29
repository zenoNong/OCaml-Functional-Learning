Here’s a **concise and complete note** for **Section 4.7: Currying** from your OCaml material:

---

## 📘 4.7 Currying — Summary Notes

---

### ✅ **Curried vs. Uncurried Functions**

#### 1️⃣ **Curried Function**

* Takes **one argument at a time**, returning a new function each time.

```ocaml
let add x y = x + y
(* Type: int -> int -> int *)
```

* ✅ Supports **partial application** (e.g., `let add5 = add 5`).

---

#### 2️⃣ **Uncurried Function**

* Takes a **tuple** as its argument.

```ocaml
let add' t = fst t + snd t
(* Type: int * int -> int *)

(* Or with pattern matching: *)
let add'' (x, y) = x + y
```

* ❌ Does **not support partial application**

---

### ℹ️ **Terminology**

* **Curried**: `t1 -> t2 -> t3`
* **Uncurried**: `(t1 * t2) -> t3`
* Named after **Haskell Curry** (logician, not the food 😄).

---

### 🔁 **Conversion Between Curried and Uncurried**

```ocaml
let curry f x y = f (x, y)
(* Type: ('a * 'b -> 'c) -> 'a -> 'b -> 'c *)

let uncurry f (x, y) = f x y
(* Type: ('a -> 'b -> 'c) -> ('a * 'b) -> 'c *)
```

#### ✅ Examples:

```ocaml
let uncurried_add = uncurry add
let curried_add = curry add''
```

---

### 📌 **Use Cases**

| Situation                        | Use Curried | Use Uncurried |
| -------------------------------- | ----------- | ------------- |
| Want **partial application**     | ✅ Yes       | ❌ No          |
| Working with **tuples**          | ❌ No        | ✅ Yes         |
| Functional composition/pipelines | ✅ Easy      | ❌ Verbose     |

---

Let me know if you'd like this in **Markdown format** for GitHub or integrated into your functional programming notes!
