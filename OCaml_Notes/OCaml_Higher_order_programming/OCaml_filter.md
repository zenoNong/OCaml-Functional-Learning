Here is a **concise and clear summary** of **Section 4.3: `filter`** in OCaml:

---

## 📘 4.3. `filter` – Summary Notes

### ✅ **What is `filter`?**

* `filter` is a **higher-order function** that:

  * Takes a **predicate** function `p : 'a -> bool`
  * Returns a **sublist** of elements for which `p` returns `true`

---

### 🧪 Example: Filtering Even and Odd Numbers

#### Initial definitions:

```ocaml
let even n = n mod 2 = 0
let odd n = n mod 2 <> 0

let rec evens = function
  | [] -> []
  | h :: t -> if even h then h :: evens t else evens t

let rec odds = function
  | [] -> []
  | h :: t -> if odd h then h :: odds t else odds t
```

---

### 🧠 Abstracting with `filter`

We notice the pattern in `evens` and `odds` is the same — only the condition differs.
So, we factor out the predicate:

```ocaml
let rec filter p = function
  | [] -> []
  | h :: t -> if p h then h :: filter p t else filter p t
```

And then define:

```ocaml
let evens = filter even
let odds = filter odd
```

✅ **Type**:

```ocaml
filter : ('a -> bool) -> 'a list -> 'a list
```

---

## 🔁 4.3.1. **Filter and Tail Recursion**

### ❌ Naïve version is not tail recursive.

✅ Tail-recursive version:

```ocaml
let rec filter_aux p acc = function
  | [] -> List.rev acc
  | h :: t -> if p h then filter_aux p (h :: acc) t else filter_aux p acc t

let filter p = filter_aux p []
```

* Builds the result list **in reverse using `::` (cons)**.
* Uses `List.rev` at the end to **correct the order**.
* This is how **`List.filter`** is implemented in the **OCaml standard library**.

---

### 🔍 Why the reversal?

* Using `::` is **constant-time**, but prepends to the list.
* Reversal makes result correct in **linear time**.
* OCaml chooses to include reversal **by default in `List.filter`**, unlike `List.map`.

---

## 🌍 4.3.2. Filter in Other Languages

### 🔸 Python:

```python
list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4]))  # → [2, 4]
```

* `filter` is lazy; `list()` forces evaluation.

### 🔸 Java (Streams API):

```java
Stream.of(1, 2, 3, 4)
  .filter(x -> x % 2 == 0)
  .collect(Collectors.toList());  // → [2, 4]
```

---

## 📌 Final Takeaways

| Concept                    | Key Point                               |
| -------------------------- | --------------------------------------- |
| `filter`                   | Selects elements satisfying a predicate |
| Tail-recursive version     | Uses accumulator + `List.rev`           |
| OCaml stdlib `List.filter` | Built-in reversal included              |
| Cross-language support     | Common in Python, Java, etc.            |

> Like `map`, `filter` improves **code reuse, clarity, and abstraction** by factoring common patterns.

---

Let me know if you’d like a comparison chart of `map` vs `filter` or a visual guide!
