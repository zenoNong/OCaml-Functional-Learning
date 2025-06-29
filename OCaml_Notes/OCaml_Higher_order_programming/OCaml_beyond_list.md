Here’s a **concise, structured summary** of **Section 4.5: *Beyond Lists*** — which explores how functional programming concepts like `map`, `fold`, and `filter` extend beyond lists to trees and other data structures.

---

## 📘 4.5. Beyond Lists – Summary Notes

---

### 🌳 Tree Data Structure

```ocaml
type 'a tree =
  | Leaf
  | Node of 'a * 'a tree * 'a tree
```

Binary trees with values at each node and possibly empty (Leaf) subtrees.

---

## 🔹 4.5.1. `map` on Trees

Applies a function to each node value:

```ocaml
let rec map_tree f = function
  | Leaf -> Leaf
  | Node (v, l, r) -> Node (f v, map_tree f l, map_tree f r)
```

📌 Similar to `List.map`, but recursively applies `f` at every node.

---

## 🔹 4.5.2. `fold` on Trees

### 🧠 **Goal**: Abstract recursion over trees, generalizing what `fold_right` does for lists.

#### Fold on Lists (custom representation):

```ocaml
type 'a mylist = Nil | Cons of 'a * 'a mylist

let rec fold_mylist f acc = function
  | Nil -> acc
  | Cons (h, t) -> f h (fold_mylist f acc t)
```

* Replaces:

  * `Nil` with `acc`
  * `Cons` with application of `f`

#### Fold on Trees:

```ocaml
let rec fold_tree f acc = function
  | Leaf -> acc
  | Node (v, l, r) -> f v (fold_tree f acc l) (fold_tree f acc r)
```

* Replaces:

  * `Leaf` with `acc`
  * `Node (v, l, r)` with `f v left_result right_result`
* Requires **ternary** function `f: 'a -> 'b -> 'b -> 'b` because each node has 3 components.

### 🧪 Example Uses:

```ocaml
let size t = fold_tree (fun _ l r -> 1 + l + r) 0 t
let depth t = fold_tree (fun _ l r -> 1 + max l r) 0 t
let preorder t = fold_tree (fun x l r -> [x] @ l @ r) [] t
```

* `size`: counts nodes
* `depth`: computes max depth
* `preorder`: returns preorder traversal

---

### 💡 Why not `fold_left`?

* Tail recursion (used in `fold_left`) is **not feasible** for trees due to **two recursive branches**.
* Can't complete either branch before starting the other.
* So, fold on trees always resembles `fold_right`.

---

### 🧠 Technique: Writing `fold` for Any Variant Type

1. **Take arguments for each constructor**.
2. **Recursively call `fold` on any values of type `t`**.
3. **Use `fold` arguments to combine recursive results**.

This pattern defines a **catamorphism** (generalized fold) — foundational in category theory.

---

## 🔹 4.5.3. `filter` on Trees

⚠️ Trickier than lists due to hierarchical structure.

### Challenge:

* If we remove a node, **what happens to its children**?

  * If both children survive filtering, how do we reassemble the tree?
  * Needs domain-specific restructuring.

### 📌 Simplified Strategy:

* **Filter a node → keep or prune entire subtree**

```ocaml
let rec filter_tree p = function
  | Leaf -> Leaf
  | Node (v, l, r) ->
    if p v then Node (v, filter_tree p l, filter_tree p r)
    else Leaf
```

* Keeps only nodes where `p v = true`; otherwise removes entire subtree at that node.

---

## 📌 Key Takeaways

| Concept      | Lists                             | Trees                                       |
| ------------ | --------------------------------- | ------------------------------------------- |
| `map`        | Apply `f` to each element         | Apply `f` to each node                      |
| `fold`       | Binary op, accumulates left/right | Ternary op, accumulates root/left/right     |
| Tail Rec?    | Yes (`fold_left`)                 | No — due to binary structure                |
| `filter`     | Selective element inclusion       | More complex — subtree pruning              |
| General Fold | Catamorphism                      | Same — recursive combinator per constructor |

---

Let me know if you'd like **visual diagrams** for tree folding or catamorphism examples!
