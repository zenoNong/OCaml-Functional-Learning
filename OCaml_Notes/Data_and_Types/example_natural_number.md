# Example: Trees

## Tree Representation with Tuples

```ocaml
(* Binary tree with nodes and leaves *)
type 'a tree =
  | Leaf
  | Node of 'a * 'a tree * 'a tree
```

- `Leaf`: represents an empty tree.
- `Node (value, left, right)`: a node containing value and two subtrees.

Structure similarity:

```ocaml
type 'a tree =          type 'a mylist =
  | Leaf                 | Nil
  | Node of ...          | Cons of ...
```

Example construction:

```ocaml
(*
         4
       /   \
      2     5
     / \   / \
    1   3 6   7
*)
let t =
  Node (4,
    Node (2,
      Node (1, Leaf, Leaf),
      Node (3, Leaf, Leaf)),
    Node (5,
      Node (6, Leaf, Leaf),
      Node (7, Leaf, Leaf)))
```

Size of tree:

```ocaml
let rec size = function
  | Leaf -> 0
  | Node (_, l, r) -> 1 + size l + size r
```

---

## Tree Representation with Records

```ocaml
type 'a tree =
  | Leaf
  | Node of 'a node

and 'a node = {
  value: 'a;
  left: 'a tree;
  right: 'a tree
}
```

Example:

```ocaml
(* Represents: 2 with children 1 and 3 *)
let t =
  Node {
    value = 2;
    left = Node {value = 1; left = Leaf; right = Leaf};
    right = Node {value = 3; left = Leaf; right = Leaf}
  }
```

Search:

```ocaml
let rec mem x = function
  | Leaf -> false
  | Node {value; left; right} -> value = x || mem x left || mem x right
```

Preorder traversal:

```ocaml
let rec preorder = function
  | Leaf -> []
  | Node {value; left; right} -> [value] @ preorder left @ preorder right
```

Improved linear-time version:

```ocaml
let preorder_lin t =
  let rec pre_acc acc = function
    | Leaf -> acc
    | Node {value; left; right} -> value :: (pre_acc (pre_acc acc right) left)
  in pre_acc [] t
```

- Uses accumulator to build list efficiently.
- Exactly one `::` per `Node`, so runs in **linear time**.