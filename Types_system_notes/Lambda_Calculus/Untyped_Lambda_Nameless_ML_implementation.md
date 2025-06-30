## OVERVIEW: What Are These Three Chapters Really Doing?

These three chapters form a **theoretical-to-practical bridge** for the **untyped lambda calculus**:

| Chapter | Focus                                                                 |
| ------- | --------------------------------------------------------------------- |
| **5**   | What is the **lambda calculus**? Syntax, semantics, and substitution. |
| **6**   | How to represent lambda terms **namelessly** (de Bruijn indices).     |
| **7**   | How to **implement** the calculus in **OCaml** using the above ideas. |

Together, they show how a mathematical model of computation (λ-calculus) can be translated into working code.

---

# PART 1: Understanding the Untyped Lambda Calculus (Chapter 5)

### 5.1 Basics: What Is the Lambda Calculus?

* The **lambda calculus** is a minimal model of computation using just:

  * **Variables** (e.g., `x`)
  * **Abstraction** (functions: `λx. t`)
  * **Application** (function call: `t1 t2`)

Example:

```text
λx. x     -- identity function
(λx. x) y -- applies id to y
```

### 5.2 Operational Semantics: How It Computes

* The core computation rule is **β-reduction**:

  ```text
  (λx. t1) t2 → [x ↦ t2]t1
  ```

  That is, apply a function to an argument by replacing `x` with `t2` in `t1`.

* Example:

  ```text
  (λx. x) y → y
  ```

* This is the only form of computation in pure λ-calculus—no loops, ifs, etc.

### 5.3 Substitution & Evaluation Strategies

* Substitution (`[x ↦ s]t`) is subtle:

  * Must avoid **variable capture** (accidentally binding the wrong variable).
  * Sometimes requires **renaming** bound variables.

* **Evaluation strategies**:

  * **Full beta-reduction**: Reduce any redex, anywhere.
  * **Normal order**: Always reduce the leftmost-outermost redex first.
  * **Call-by-name / call-by-value**: Important for programming languages.

---

## Illustration 1: Key Ideas from Chapter 5

```text
Original term:    (λx. x x) (λx. x x)
Redex:            (λx. x x) (λx. x x)
β-reduction:      [x ↦ (λx. x x)](x x) → (λx. x x) (λx. x x) -- same again

This is non-terminating: a classic λ-calculus loop.
```

---

# PART 2: Nameless Representation (Chapter 6)

### Why Use Nameless Terms?

* Variable names are **fragile** during substitution (you may accidentally change meaning).
* Solution: use **de Bruijn indices**:

  * Count how far a variable is from its binder.
  * Example:

    ```text
    λx. x     →  λ. 0
    λx. λy. x →  λ. λ. 1
    ```

### Formal Definitions

* Define sets `Tn` for terms with `n` free variables.
* Define **shifting** and **substitution** operations on nameless terms:

  * Shifting adjusts indices when moving terms in/out of abstractions.
  * Substitution replaces variables by index, carefully shifting as needed.

### Evaluation Rule (E-AppAbs) in Nameless Form

```text
(λ. t12) v2 → ↑−1 ([0 ↦ ↑1(v2)]t12)
```

* Shift `v2` up (since entering deeper scope), substitute it for index `0`, then shift the result back down.

---

## Illustration 2: From Named to Nameless

Named: `λx. λy. x y`
Context: \[y, x] → y:0, x:1
Nameless: `λ. λ. 1 0`
Explanation:

* `y` is bound by 2nd lambda → index 0
* `x` is one scope above → index 1

---

# PART 3: Implementing It in OCaml (Chapter 7)

### 7.1 Terms and Contexts in Code

OCaml datatype:

```ocaml
type term =
  | TmVar of info * int * int
  | TmAbs of info * string * term
  | TmApp of info * term * term
```

* `TmVar(info, index, context_length)`
* `TmAbs(info, name_hint, body)`
* `TmApp(info, t1, t2)`

Context is a list of `(string * binding)`—it maps de Bruijn indices back to names during printing.

### 7.2 Shifting and Substitution

```ocaml
let termShift d t = ...
let termSubst j s t = ...
let termSubstTop s t = termShift (-1) (termSubst 0 (termShift 1 s) t)
```

* `termShift`: walks through `t`, adjusting indices.
* `termSubst`: replaces variable `j` with `s`.
* `termSubstTop`: full substitution at outermost level (used in evaluation).

### 7.3 Evaluation (β-Reduction)

```ocaml
let rec eval1 ctx t = match t with
  | TmApp(_, TmAbs(_, _, t12), v2) when isval ctx v2 -> termSubstTop v2 t12
  | TmApp(_, v1, t2) when isval ctx v1 -> ...
  | TmApp(_, t1, t2) -> ...
```

* Evaluates step-by-step: looks for redexes and reduces them.
* Uses `isval` to check if something is a λ-abstraction.

### 7.4 Pretty Printing

```ocaml
let rec printtm ctx t = ...
```

* Translates De Bruijn indices back into human-readable variable names.
* Uses `pickfreshname` to avoid name clashes.

---

## Illustration 3: Evaluating a Real Term

Term: `(λx. x) (λy. y)`
Nameless: `TmApp(_, TmAbs(_, "x", TmVar(_, 0, 1)), TmAbs(_, "y", TmVar(_, 0, 1)))`

Evaluation steps:

1. Recognize `TmApp (λ. 0) (λ. 0)` as a redex.
2. Use `termSubstTop` to substitute:

   * Shift argument: `↑1(λ. 0)`
   * Substitute at index `0`
   * Shift result back down

Final result: `λy. y` (TmAbs(*, "y", TmVar(*, 0, 1)))

---

## Summary: How They Connect

| Concept                   | Chapter 5            | Chapter 6                    | Chapter 7                           |
| ------------------------- | -------------------- | ---------------------------- | ----------------------------------- |
| What is λ-calculus        | Introduced           | Still assumed                | Implemented                         |
| Variable naming           | Symbolic             | De Bruijn indices (nameless) | Index-based implementation          |
| Substitution              | Informally defined   | Precisely defined            | Directly implemented as `termSubst` |
| Evaluation                | Conceptual           | Refined for nameless         | Executable (`eval`, `eval1`)        |
| Contexts (variable scope) | Named envs (FV sets) | Numbered scopes (Γ contexts) | Tracked in code                     |