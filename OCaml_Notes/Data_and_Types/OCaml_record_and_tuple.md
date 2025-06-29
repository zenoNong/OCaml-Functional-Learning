# Records and Tuples
## Records

- A **record** is a fixed collection of **named fields**:

  ```ocaml
  type ptype = TNormal | TFire | TWater
  type mon = { name : string; hp : int; ptype : ptype }

  let c = { name = "Charmander"; hp = 39; ptype = TFire }
  ```

- **Access fields** using `dot` notation:

  ```ocaml
  c.hp         (* => 39 *)
  ```

- **or `Pattern matching`** on records:
  ```ocaml
  match c with { name = n; hp = h; ptype = t } -> h
  (* or with sugar: *)
  match c with { name; hp; ptype } -> hp
  ```
### Record Copy

- Copy and modify a record:
  ```ocaml
  { c with hp = 50 }
  (* This creates a new record. It does **not** mutate the old one. Also same as *)
  { hp = 50; name = c.name; ptype = c.ptype }
  ```

## Tuples
- Tuples group values by **position**, not by name:
  ```ocaml
  (1, 2)                  (* - : int * int = (1, 2) *)
  (true, "Hello")         (* - : bool * string = (true, "Hello") *)
  ([1; 2; 3], (0.5, 'X')) (* - : int list * (float * char) = ([1; 2; 3], (0.5, 'X')) *)
  ```

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
  If ei ==> vi then (e1, ..., en) ==> (v1, ..., vn) (* These are the values, in running we don't need the type inference as it was checked during static *)
  ```

- **Static semantics**:

  ```ocaml
  If ei : ti then (e1, ..., en) : t1 * ... * tn (* It checks the types before runtime. *)
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

- Use a **variant** when a value is **either/or**.
- Use a **record or tuple** when a value is **both/and**.

### Type Theory Insight

- Variants = **Sum types** (`Σ`)
- Tuples/Records = **Product types** (`×`)