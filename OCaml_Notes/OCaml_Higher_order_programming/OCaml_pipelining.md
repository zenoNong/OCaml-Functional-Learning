# Pipelining
## **Goal**

Demonstrate a **clear and functional programming style** using the **pipeline operator `|>`** to process data through a sequence of transformations.

## Example: Sum of Squares from 0 to `n`

### Efficient Math Formula (best performance):

$$
\text{sum\_sq}(n) = \frac{n(n+1)(2n+1)}{6}
$$

### Assume we **forgot** the formula.

---

## **Imperative Approach (Python)**

```python
def sum_sq(n):
    sum = 0
    for i in range(0, n+1):
        sum += i * i
    return sum
```

---

## **Tail-Recursive OCaml Version**

```ocaml
let sum_sq n =
  let rec loop i sum =
    if i > n then sum
    else loop (i + 1) (sum + i * i)
  in loop 0 0
```

**Efficient**: Constant space and time-efficient recursion
**Less declarative/functional in style**

---

## **Functional Pipelined Version**

```ocaml
let rec ( -- ) i j = if i > j then [] else i :: i + 1 -- j
let square x = x * x
let sum = List.fold_left ( + ) 0

let sum_sq n =
  0 -- n
  |> List.map square
  |> sum
```

What’s happening:

1. `0 -- n`: Builds list `[0; 1; 2; ...; n]`
2. `List.map square`: Squares each element
3. `sum`: Adds all the squares

**Readable** and **declarative**
**Inefficient** — creates intermediate lists (space: O(n))

---

## Other Less Clear Versions

```ocaml
(* Verbose with unnecessary let-bindings *)
let sum_sq n =
  let l = 0 -- n in
  let sq_l = List.map square l in
  sum sq_l

(* Nested calls — hard to read *)
let sum_sq n =
  sum (List.map square (0 -- n))
```

---

## About the Pipeline Operator `|>`

* **Definition**: `x |> f` is the same as `f x`
* Promotes **top-to-bottom** and **left-to-right** reading
* Encourages writing code as a **pipeline of transformations**
* Used heavily in OCaml **module-based APIs** and in libraries like `Core`, `Base`, etc.

---

## Efficiency Caveat

* The pipeline version builds **intermediate lists**, increasing space and runtime
* The tail-recursive version avoids those by computing directly

> **Important:** The **inefficiency is not caused by `|>`** itself — but by building full intermediate lists.