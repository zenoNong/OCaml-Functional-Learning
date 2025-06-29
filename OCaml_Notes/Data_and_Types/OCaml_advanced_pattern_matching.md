# Advanced Pattern Matching

## Useful Pattern Forms

- `p1 | ... | pn` - **Or pattern**: matches if any `pi` matches. All patterns must bind **same variables**.
  ```ocaml
  let is_vowel c =
    match c with
    | 'a' | 'e' | 'i' | 'o' | 'u' -> true
    | _ -> false
    (*Matches if c is any of the vowels.*)
  ```
- `(p : t)` - **Typed pattern**: matches `p` and constrains it to type `t`.
  ```ocaml
    let square_int (x : int) = x * x
    (*Constrains x to be an int.*)
  ```
- `c` - **Constant pattern**: literal matches (e.g., `1`, `true`, `"ok"`).
  ```ocaml
    let yes_or_no s =
    match s with
    | "yes" -> true
    | "no" -> false
    | _ -> failwith "unknown"
    (*Matches exact string values.*)
  ```
- `'a'..'z'` - **Character range pattern**.
  ```ocaml
    let is_lowercase c =
    match c with
    | 'a'..'z' -> true
    | _ -> false
    (*Matches any lowercase letter.*)
  ```
- `p when e` - **Guard pattern**: matches `p` only if `e` evaluates to `true`.
  ```ocaml
    let sign n =
    match n with
    | x when x > 0 -> "positive"
    | x when x < 0 -> "negative"
    | 0 -> "zero"
    (*Matches x only if the condition after when is true.*)
  ```

---

## Pattern Matching with `let`

- General `let` syntax:

  ```ocaml
  let p = e1 in e2
  (* p can be any pattern — not just an identifier.*)
  ```

## Pattern Matching with Functions

### Function Syntax
- `let f p1 ... pn = e1 in e2`       (* let expression *), `let f p1 ... pn = e `              (* top-level definition *), `fun p1 ... pn -> e`                (* anonymous function *)

  ```ocaml
    let square x = x * x in
    square 3 + square 4
    (*Defines square locally.
    Calculates square 3 + square 4 (which is 9 + 16 = 25).*)
  ```

## Pattern Matching Examples

### Matching Record Fields

```ocaml
type ptype = TNormal | TFire | TWater

type mon = {
  name : string;
  hp : int;
  ptype : ptype
}
```

```ocaml
(* OK *)
let get_hp m = match m with { name = n; hp = h; ptype = t } -> h
(* better *)
let get_hp m = match m with { name = _; hp = h; ptype = _ } -> h
(* better *)
let get_hp m = match m with { name; hp; ptype } -> hp
(* better *)
let get_hp m = match m with { hp } -> hp
(* best *)
let get_hp m = m.hp
```

### Matching Tuples

```ocaml
let fst (x, _) = x
let snd (_, y) = y
```

These are predefined in `Stdlib`.

```ocaml
(* OK *)
let thrd t = match t with x, y, z -> z
(* good *)
let thrd t = let x, y, z = t in z
(* better *)
let thrd t = let _, _, z = t in z
(* best *)
let thrd (_, _, z) = z
```

> Standard library only includes `fst` and `snd`, not for 3+ tuples.