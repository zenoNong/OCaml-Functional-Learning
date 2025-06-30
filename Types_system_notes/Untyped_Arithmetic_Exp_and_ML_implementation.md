# PART 1: Language Syntax & Representation

### Sections Covered:

* **3.1 Introduction**
* **3.2 Syntax**
  (Definitions 3.2.1 – 3.2.6)
* **4.1 Syntax (OCaml translation)**

### Key Ideas Introduced:

### Why Start with a Toy Language?

To talk about **type systems**, we first need to understand:

* How to define a language formally (syntax)
* How to describe what programs "do" (semantics)
* How to catch errors (type safety and runtime behavior)

A **very small language** with:

* **Booleans**: `true`, `false`, `if...then...else`
* **Numbers**: `0`, `succ`, `pred`, `iszero`

is enough to develop these ideas clearly, without unnecessary complexity.

---

### How is the Syntax Defined?

We learn **3 different approaches** to formally define valid expressions (called “terms”) in the language:

---

### 1. **BNF-style grammar** (Backus-Naur Form)

```bnf
t ::= true | false
    | if t then t else t
    | 0 | succ t | pred t | iszero t
```

* Each line says what kind of expression (`t`) is allowed.
* `t` is a **metavariable**—not part of the language, but used in rules about the language.

---

### 2. **Inductive definition**

This defines the smallest set of terms `T` such that:

1. Constants `true`, `false`, `0` are in `T`.
2. If `t1 ∈ T`, then `succ t1`, `pred t1`, and `iszero t1` are in `T`.
3. If `t1, t2, t3 ∈ T`, then `if t1 then t2 else t3 ∈ T`.

This says:

* Start with constants
* Build up more complex expressions using the language's operators
* Nothing else is allowed (the set is minimal)

---

### 3. **Layered sets (`S₀, S₁, ...`)**

Build sets incrementally:

* `S₀ = ∅`
* `S₁ = {true, false, 0}`
* `S₂` = apply one constructor to something in `S₁`
* Keep building...

This is useful to **visualize how terms grow** stage-by-stage.

---

### OCaml Syntax (Chapter 4)

The abstract syntax is implemented in OCaml with a **variant type**:

```ocaml
type term =
  | TmTrue of info
  | TmFalse of info
  | TmIf of info * term * term * term
  | TmZero of info
  | TmSucc of info * term
  | TmPred of info * term
  | TmIsZero of info * term
```

* Each constructor corresponds to a kind of language term.
* `info` carries metadata (like position in file), not logically important.

---

### Why So Many Syntax Forms?

All three forms of syntax definitions serve different purposes:

* **BNF**: for humans and parsers
* **Inductive rules**: for proofs
* **OCaml code**: for interpreters

They all describe the same thing: **how to build valid programs**.

---

### Illustration:

Take the example program:

```plaintext
if iszero (pred (succ 0)) then true else false
```

Its OCaml representation:

```ocaml
TmIf (info,
  TmIsZero (info,
    TmPred (info,
      TmSucc (info,
        TmZero info))),
  TmTrue info,
  TmFalse info)
```

This shows how an abstract syntax tree in OCaml **mirrors** the logic of the language syntax.

---

# PART 2: Induction & Structural Properties of Terms

### Sections Covered:

* **3.3 Induction on Terms**
* Definitions:

  * `size(t)`
  * `depth(t)`
  * `Consts(t)`
* **Principles of Induction (3.3.4)**

---

### What Is This Section Doing?

Now that we know how to **build terms**, we want to:

1. Define functions that operate on them
2. Prove facts about them (like size, structure, etc.)
3. Learn how to **formally reason** about them

---

### Properties We Can Compute:

### Constants in a Term (`Consts(t)`):

List of all literal constants (`true`, `false`, `0`) in a term.

### Size of a Term:

Total number of nodes in the term's abstract syntax tree.

```ocaml
size(true) = 1
size(succ(t1)) = size(t1) + 1
...
```

### Depth of a Term:

How many levels deep the tree goes.

```ocaml
depth(if t1 then t2 else t3) =
  max(depth(t1), depth(t2), depth(t3)) + 1
```

---

### Three Types of Induction You’ll Use:

| Induction Type | What it does                                  |
| -------------- | --------------------------------------------- |
| **On Depth**   | Assume P holds for all terms of smaller depth |
| **On Size**    | Assume P holds for all terms of smaller size  |
| **Structural** | Assume P holds for subterms                   |

Most proofs use **structural induction** because it’s closest to how code and syntax trees actually work.

---

### Illustration:

Let’s prove:

```math
|Consts(t)| ≤ size(t)
```

Take:

```ocaml
if iszero (pred 0) then true else false
```

Tree size:

* `if` (1)
* `iszero` (1)
* `pred` (1)
* `0` (1)
* `true` (1)
* `false` (1)

So `size = 6`, `Consts = {0, true, false}` → size ≥ number of constants 

You’d prove this by structural induction on `t`.

---

# PART 3: Semantics — How Programs Compute

### Sections Covered:

* **3.4 Semantic Styles**
* **3.5 Evaluation**

  * One-step rules
  * Normal forms & stuck terms
  * Multi-step evaluation
  * Termination proof

---

### What's the Goal Here?

Now we want to define the **meaning of a program** — what does it evaluate to?

There are three major styles of semantics:

---

### 1. **Operational Semantics (used in TAPL)**

* Defines execution as rewriting terms step-by-step
* Example:

  ```plaintext
  if true then 0 else 1 → 0
  ```

---

### 2. **Denotational Semantics**

* Maps each term to a mathematical object (number, boolean, etc.)
* More abstract; used in formal verification

---

### 3. **Axiomatic Semantics**

* Specifies what’s provable about the program (e.g., loop invariants)

TAPL uses **small-step operational semantics**:

```plaintext
t → t′
```

---

### Evaluation Rules:

### Booleans:

```plaintext
if true then t2 else t3 → t2     (E-IfTrue)
if false then t2 else t3 → t3    (E-IfFalse)
if t1 → t1′ then
  if t1 then t2 else t3 → if t1′ then t2 else t3   (E-If)
```

### Numbers:

```plaintext
pred 0 → 0                       (E-PredZero)
pred (succ nv) → nv             (E-PredSucc)
iszero 0 → true
iszero (succ nv) → false
```

---

### What is a "Stuck Term"?

* A **value** is fully evaluated (e.g., `true`, `0`, `succ 0`)
* A **normal form** is any term with no evaluation step left
* A **stuck term** is a normal form but NOT a value → it's an error!

Example:

```plaintext
succ false    ← stuck term
```

Because no rule says how to evaluate `succ false`, but it's not a valid value.

---

### Why does this matter?

* **Stuck = runtime error**
* Later, **type systems** will be used to **prevent stuck terms** from even being written

---

### Illustration:

Let’s evaluate:

```plaintext
pred (succ (pred 0))
```

1. `pred 0 → 0`
2. `succ 0` stays as is
3. `pred (succ 0) → 0`

Final result: `0`

---

# PART 4: OCaml Interpreter Implementation

### Sections Covered:

* **4.2 Evaluation (`eval1`, `eval`)**

---

### How to Run the Semantics in Code?

In Chapter 4, the semantics from Chapter 3 are **translated directly** into OCaml functions.

---

### `eval1`: One-step evaluator

Mimics the rules like `if true then t2 else t3 → t2`

```ocaml
let rec eval1 t = match t with
  | TmIf(_, TmTrue(_), t2, _) -> t2
  | TmIf(_, TmFalse(_), _, t3) -> t3
  | TmIf(fi, t1, t2, t3) ->
      let t1' = eval1 t1 in
      TmIf(fi, t1', t2, t3)
  ...
```

---

### `eval`: Repeats `eval1` until term is done

```ocaml
let rec eval t =
  try eval (eval1 t)
  with NoRuleApplies -> t
```

This simulates the `→*` multi-step semantics.

---

### What Happens with Errors?

If no evaluation rule applies:

* `eval1` raises `NoRuleApplies`
* This ends evaluation in `eval`

This naturally handles both:

* Successfully finished terms (values)
* Stuck terms (runtime errors)

---

### Illustration:

Input:

```ocaml
TmPred (info, TmSucc (info, TmZero info))
```

Evaluation steps:

1. Matches `E-PredSucc` ⇒ returns `TmZero info`
2. Done — final value.

---

# CONCLUSION: What This Setup Teaches

| Concept        | You Learn                                             |
| -------------- | ----------------------------------------------------- |
| Syntax         | How to formally define the shape of valid programs    |
| Semantics      | How programs compute via rewriting                    |
| Induction      | How to rigorously prove properties of terms           |
| Implementation | How to convert theory into working code               |
| Runtime Errors | How to catch "stuck" programs — later solved by types |

