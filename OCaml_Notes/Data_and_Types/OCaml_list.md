
# OCaml Lists

## Building Lists
- OCaml lists are **`singly-linked`**, **`homogeneous`** (same type), and **`immutable`**.
  ```ocaml
  []              (* empty list, named nil *)
  e1 :: e2        (* cons: add e1 to front of e2, :: is right associative *)
  [e1; e2; e3]    (* syntactic sugar for e1 :: e2 :: e3 :: [] *)
  [e1;e2]@[e4;e5] (* adding to lists *)
  ```

## Accessing Lists
- Using **`pattern matching`** to decompose lists:
  ```ocaml
  let rec sum lst =
    match lst with
    | [] -> 0
    | h :: t -> h + sum t
  ```

- **`Standard library`**:

  ```ocaml
  List.length [1;2;3]       (* will returns 3 *)
  List.nth [3;4;5;6] 0      (* access the 0-indexed element of the list*)
  List.hd [1;2;3]           (* it return the head element, 1 *)
  List.rev [1; 2; 3]           (* reverse: [3; 2; 1] *)
  List.map (fun x -> x * 2) [1; 2; 3]   (* [2; 4; 6] *)
  List.fold_left ( + ) 0 [1; 2; 3]      (* 6, left fold: (((0+1)+2)+3) *)
  List.filter (fun x -> x mod 2 = 0) [1;2;3;4]   (* keeps even: [2; 4] *)
  List.exists (fun x -> x > 3) [1;2;3]     (* returns true *)
  List.for_all (fun x -> x < 5) [1;2;3]    (* returns true *)
  List.find (fun x -> x mod 2 = 0) [1;3;4] (* returns 4, first match *)
  List.mem 2 [1;2;3]             (* returns true if 2 is in the list *)
  List.init 5 (fun x -> x * x)  (* [0; 1; 4; 9; 16] — builds list from 0 to 4 *)
  ```

## Immutability
- Lists are **`immutable`** meaning, `elements can't be changed in-place`, `list can't be modified`.

  ```ocaml
  let lst = [1;2]
  lst[0] <- 42 (* will give error, in-place value can't be changed *)
  let new_list = 0::lst (*new_list is [0;1;2] but lst is still [1;2]*)
  (* Here lst porting is not copied, instead it same memory lst is shared as tail of new_list*)
  ```

- OCaml **shares** the tail when building new lists, not copies -> memory & time efficient, immutable.

---

## Pattern Matching with Lists

- Pattern matching with lists allows you to:

    - Check the structure of a list
    - Bind parts of the list to variables
    - Execute different code based on the pattern matched
- Basic patterns:
  ```ocaml
  match my_list with
    | [] -> "Empty list"              (* matches empty list *)
    | [x] -> "One element"           (* matches list with exactly one element *)
    | x :: y :: [] -> "Two elements" (* matches list with exactly two elements *)
    | h :: t -> "More elements"      (* matches list with head and tail *)
  ```
- Common Mistake Example
    ```ocaml
    (* INCORRECT way *)
    let length_is lst n =
    match List.length lst with
    | n -> true      (* The pattern n is catch all variable, binds any number so always return true *)
    | _ -> false     (* Never reached *)

    (* CORRECT way *)
    let length_is lst n = List.length lst = n
    ```
---

## Deep Pattern Matching

- Match list shapes precisely:
  - `_ :: []`        → list of exactly 1 element
  - `_ :: _`         → list of at least 1 element
  - `_ :: _ :: []`   → list of exactly 2 elements
  - `_ :: _ :: _ :: _` → list of at least 3 elements

---

## Immediate Matches

- Use `function` only when pattern matching is the last thing your function does
- If you need to match on earlier arguments, use regular match syntax
- `function` is syntactic sugar for `fun x -> match x` with
  ```ocaml
  let rec sum = function
    | [] -> 0
    | h :: t -> h + sum t
  ```

---

## List Comprehensions

- OCaml does **not** have list comprehensions like Python or Haskell.
- Tasks like `filtering`/`mapping` are handled with **`higher-order functions`** and **`pipeline operator |>`**.
    ```ocaml
    (* Python: [x*2 for x in range(10)] *)
    let doubled = 
    List.init 10 (fun x -> x)  (* Create list [0;1;2;...;9] *)
    |> List.map (fun x -> x * 2)

    (* Python: [x for x in range(10) if x % 2 == 0] *)
    let evens =
    List.init 10 (fun x -> x)
    |> List.filter (fun x -> x mod 2 = 0)
    ```

