# Unit Testing with OUnit

OCaml uses the **OUnit** library for unit testing (similar to JUnit in Java).

## Basic Setup
1. Define your function in `sum.ml`:
   ```ocaml
   let rec sum = function
     | [] -> 0
     | x :: xs -> x + sum xs
   ```
2. Write tests in `test.ml`:
   ```ocaml
   open OUnit2
   open Sum

   let tests = "test suite for sum" >::: [
     "empty"      >:: (fun _ -> assert_equal 0 (sum []));
     "singleton"  >:: (fun _ -> assert_equal 1 (sum [1]));
     "two_elements" >:: (fun _ -> assert_equal 3 (sum [1; 2]));
   ]

   let _ = run_test_tt_main tests
   ```
3. Add `dune` files:
   - `dune-project`: `(lang dune 3.4)`
   - `dune`:
     ```lisp
     (executable
       (name test)
       (libraries ounit2))
     ```
4. Build and run:
   ```bash
   dune build test.exe
   dune exec ./test.exe
   ```

## Debugging Failures
- If a test fails, OUnit shows which test and the error.
- Use `~printer:string_of_int` to show expected vs actual:
  ```ocaml
  assert_equal 3 (sum [1; 2]) ~printer:string_of_int
  ```

## Refactoring with Helper Functions
Avoid repeated code with a helper:
```ocaml
let make_sum_test name expected input =
  name >:: (fun _ -> assert_equal expected (sum input) ~printer:string_of_int)

let tests = "sum tests" >::: [
  make_sum_test "empty" 0 [];
  make_sum_test "singleton" 1 [1];
  make_sum_test "two_elements" 3 [1; 2];
]
```

## Testing Exceptions
- Will be covered after exception handling section.

## Test-Driven Development (TDD)
1. Write a failing test
2. Implement the minimum code to pass
3. Refactor tests/code as needed
4. Repeat

### Example: `next_weekday` function
```ocaml
type day = Sunday | Monday | Tuesday | Wednesday | Thursday | Friday | Saturday
```
Initial stub:
```ocaml
let next_weekday d = failwith "Unimplemented"
```
Write test:
```ocaml
"tue_after_mon" >:: (fun _ -> assert_equal Tuesday (next_weekday Monday))
```
Iterative improvements:
```ocaml
let next_weekday d =
  match d with
  | Monday -> Tuesday
  | Tuesday -> Wednesday
  | Wednesday -> Thursday
  | Thursday -> Friday
  | Friday | Saturday | Sunday -> Monday
```
Refactor tests:
```ocaml
let make_next_weekday_test name expected input =
  name >:: (fun _ -> assert_equal expected (next_weekday input))
```