# Records and Tuples
## Records

- A **record** is a fixed collection of **named fields**:

  ```ocaml
  type ptype = TNormal | TFire | TWater
  type mon = { name : string; hp : int; ptype : ptype }

  let c = { name = "Charmander"; hp = 39; ptype = TFire }
  ```

- **Access fields** using dot notation:

  ```ocaml
  c.hp         (* => 39 *)
  ```

- **Pattern matching** on records:

  ```ocaml
  match c with { name = n; hp = h; ptype = t } -> h
  (* or with sugar: *)
  match c with { name; hp; ptype } -> hp
  ```

### Syntax and Semantics

- **Record expression**:

  ```ocaml
  { f1 = e1; ...; fn = en }
  ```

  Order of fields is irrelevant.

- **Field access**:

  ```ocaml
  e.f
  ```

  `f` must be a defined field name, not a dynamic value.

- **Dynamic semantics**:

  ```ocaml
  If ei ==> vi, then { f1 = e1; ...; fn = en } ==> { f1 = v1; ...; fn = vn }
  If e ==> { ...; f = v; ... } then e.f ==> v
  ```

- **Static semantics**:

  - Types:
    ```ocaml
    { f1 : t1; ...; fn : tn }
    ```
  - Type rules:
    - If `ei : ti`, then `{ f1 = e1; ...; fn = en } : t`
    - If `e : t` and `f : t2`, then `e.f : t2`

### Record Copy

- Copy and modify a record:
  ```ocaml
  { c with hp = 50 }
  ```
  This creates a new record. It does **not** mutate the old one.

  Equivalent to:
  ```ocaml
  { hp = 50; name = c.name; ptype = c.ptype }
  ```

### Pattern Matching with Records

- New pattern:

  ```ocaml
  { f1 = p1; ...; fn = pn }
  ```

- Semantics: If each `pi` matches `vi`, then pattern matches and binds accordingly.

- Sugar:

  ```ocaml
  { f1; f2 } ≡ { f1 = f1; f2 = f2 }
  ```

---

## Tuples

- Tuples group values by **position**, not by name:

  ```ocaml
  (1, 2)
  (true, "Hello")
  ([1; 2; 3], (0.5, 'X'))
  ```

- A pair = 2-tuple, triple = 3-tuple. Prefer **records** for large tuples.

### Syntax and Semantics

- **Construction**:

  ```ocaml
  (e1, e2, ..., en)
  ```

- **Types**:

  ```ocaml
  t1 * t2 * ... * tn
  ```

- **Dynamic semantics**:

  ```ocaml
  If ei ==> vi then (e1, ..., en) ==> (v1, ..., vn)
  ```

- **Static semantics**:

  ```ocaml
  If ei : ti then (e1, ..., en) : t1 * ... * tn
  ```

### Pattern Matching with Tuples

- Tuple pattern:
  ```ocaml
  (p1, ..., pn)
  ```
  Must match **exactly** same arity as the value.
  ```ocaml
  match (1, 2, 3) with (x, y, z) -> x + y + z
  (* => 6 *)
  ```

---

## Variants vs. Tuples and Records

| Type         | Meaning                       | Description                                     |     |        |
| ------------ | ----------------------------- | ----------------------------------------------- | --- | ------ |
| Variant      | **One of** several options    | Sum type (e.g., \`Sun                           | Mon | ...\`) |
| Tuple/Record | **All of** several components | Product type (e.g., `(1, true)`, `{name; age}`) |     |        |

- Use a **variant** when a value is **either/or**.
- Use a **record or tuple** when a value is **both/and**.

### Type Theory Insight

- Variants = **Sum types** (`Σ`)
- Tuples/Records = **Product types** (`×`)