# Options

- An `option` type is a way to represent a value that might be missing, that is, function might **not return a value**. It has two forms:
  - `Some x` means a value is present.
  - `None` means no value.
- For any type `t`, `t option` is a type that can be either `Some value` or `None`. For example, `int option` can be `Some 42` or `None`.
-  OCaml avoids `null` to prevent null-pointer bugs. `None` is a safe, typed alternative.
- Some and None are special constructors (not keywords, but built-in values) for the option type
- `option` is a **type constructor**: for any `t`, `t option` is a valid type.
- Like `list`, `option` is not a type itself.

## Example
```ocaml
let extract o =
  match o with
  | Some i -> string_of_int i
  | None -> ""

extract (Some 42)    (* => "42" *)
extract None         (* => "" *)
```

## Writing Safe Functions with Options
```ocaml
let rec list_max = function
  | [] -> None
  | h :: t ->
    match list_max t with
    | None -> Some h
    | Some m -> Some (max h m)
```
- If the list is empty: return `None`
- Else: recursively compute and compare using `Some`

## Pattern Matching Encourages Safety

```ocaml
match maybe_val with
| Some x -> (* use x *)
| None ->   (* handle empty case *)
```

OCaml forces you to **handle all cases** (unlike Java or C++).


