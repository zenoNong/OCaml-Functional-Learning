# Exceptions

## Defining Exceptions

```ocaml
exception A
exception B
exception Code of int
exception Details of string
```

- `exception E of t`: defines a new exception, optionally carrying a value of type `t`.
- Similar to defining a **variant constructor**.

To raise:

```ocaml
raise (Failure "something went wrong")
failwith "msg"   (* same as raise (Failure "msg") *)
```

## Catching Exceptions

```ocaml
try e with
| p1 -> e1
| p2 -> e2
```

- If `e` **raises**, match exception with a branch.
- If `e` **does not raise**, return value of `e`.

## Exception Values

All exceptions have type `exn`:

- `exn` is an **extensible variant**.
- You can add constructors using `exception`.
- The `raise` expression creates and throws an exception **packet** (includes value + stack trace).

## Propagation Semantics

If a subexpression raises, that packet **propagates**:

```ocaml
let _ = raise A in raise B  (* Raises A *)
(raise B) (raise A)          (* Raises A; right evaluated first *)
```

 **Evaluation order** matters but is **unspecified** in some constructs like tuples:

```ocaml
(raise A, raise B)  (* May raise B or A depending on compiler *)
```

Force order using `let`:

```ocaml
let a = raise A in let b = raise B in (a, b)  (* Raises A *)
```

## Exception Patterns

```ocaml
match List.hd [] with
| [] -> "empty"
| _ :: _ -> "non-empty"
| exception (Failure s) -> s
```

- Pattern `exception p` matches raised exceptions.
- Syntactic sugar for combining `match` and `try`:

```ocaml
try match e with
| p1 -> e1 | p3 -> e3
with
| p2 -> e2 | p4 -> e4
```

## OUnit and Exceptions

```ocaml
open OUnit2

let tests = "suite" >::: [
  "empty" >:: (fun _ ->
    assert_raises (Failure "hd") (fun () -> List.hd []))
]

let _ = run_test_tt_main tests
```

- `assert_raises exn (fun () -> expr)` checks if `expr` raises `exn`.
- Second argument is a **thunk**: a delayed computation `unit -> 'a`.
- Allows control over **when** to evaluate `expr`.

---

