# Association Lists

- An **association list** is a simple implementation of a map or dictionary, a list of **(key, value)** pairs:

  ```ocaml
  let d = [("rectangle",4);("triangle",3)];;
  - val d : (string * int) list = [("rectangle", 4); ("triangle", 3)]
  (* Combines lists and pairs, no new data structure needed.
  Useful for small datasets where performance is not critical. *)
  ```

## Basic Operations

### Insertion

```ocaml
(** [insert k v lst] inserts key [k] with value [v] into [lst] *)
let insert k v lst = (k, v) :: lst
```

- Prepends the pair.
- Constant time: **O(1)**.
- Does not check for duplicates.

### Lookup

```ocaml
(** [lookup k lst] returns Some v if key k is bound to v; None otherwise *)
let rec lookup k = function
  | [] -> None
  | (k', v) :: t -> if k = k' then Some v else lookup k t
```

- Searches left to right.
- Returns the **most recently inserted** binding if duplicates exist.
- Linear time: **O(n)**.

---

## Comparison to Other Maps

| Feature     | Association List   | Other Maps (e.g., Hashtbl, Map module) |
| ----------- | ------------------ | -------------------------------------- |
| Insert      | O(1)               | O(log n) or amortized O(1)             |
| Lookup      | O(n)               | O(log n) / O(1)                        |
| Duplication | Allowed            | Usually not allowed                    |
| Best Use    | Small data, simple | Large data, fast lookup                |

---

## Library Functions

- `List.assoc_opt` : safer form of `lookup`
  ```ocaml
  List.assoc_opt "rectangle" d   (* => Some 4 *)
  List.assoc_opt "circle" d      (* => None *)
  ```
- No built-in `insert` because it’s simple to cons a pair manually.

## to make a insertion which don't allow duplicate
  ```ocaml
  let alist = [(1,2);(1,3);(3,4)]; ;;
  - val alist : (int * int) list = [(1, 2); (1, 3); (3, 4)]

  let rec insert_new x y = function
    | [] -> (x,y)::[]
    | (x',y')::lst -> if (x'=x && y'=y) then (x',y')::lst else
    (x',y')::(insert_new x y lst);;
  - val insert_new : 'a -> 'b -> ('a * 'b) list -> ('a * 'b) list = <fun>

  insert_new 1 3 alist ;;
  - : (int * int) list = [(1, 2); (1, 3); (3, 4)]
  insert_new 3 5 alist ;;
  - : (int * int) list = [(1, 2); (1, 3); (3, 4); (3, 5)]
  ```


