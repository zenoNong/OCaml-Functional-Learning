# OCaml Notes

## Basics
  OCaml is a functional programming language. It uses the Lambda-Calculus framework in it's internal working and static type system for early detection of error in the code.

  Some basic codes are: 
  ```ocaml
  let rec add x y = 
    if x = 0 then y else add (x-1) (y+1);;
  add x 4 6;; (* gives 10*)
    (* a simple program to add two number using recursion*)
  ```
## Data Types
There are many data tyes. They are:
- int 
- float
- char
- boolean
- string
- predefined data structures like tuples, array, list.
- also can create own data structures by records adn variants.

Some example codes:

```ocaml
    # let x = 10 ;; (*-val x : int = 10*)
    # let y = 2.5 ;; (*-val y : float = 2.5*)
    # let eval = (20 = x) ;; (*-val eval : # # bool = false*)
    # x + 10 ;; (*- : int = 20*)
    # y +. 2.7 ;; (*- : float = 5.2*)
    # 'a';; (*- : char = 'a'*)
    
    # {|This is a quoted string, here, neither \ nor " are special characters|};; (*- : string = "This is a quoted string, here, neither \\ nor \" are special characters" *)
```
**Some details about list data structure**
It can be created using [ ] where elements are seperated by **;** (not ,). Or, we can join element to list with **::**(cons) or list to list using **@** operator
```ocaml
let list = [1;2;3] (*val list : int list = [1; 2; 3]*)
5::list ;; (*- : int list = [5; 1; 2; 3]*)
[7]@list ;; (*- : int list = [7; 1; 2; 3]*)
```
## Expressions
In Ocaml everything is an expression and the primary goal is to evaluate it to a value(it has no further computation). It can be **2, true, "hello"** etc

## Primitive types and Values
### 1. int: Integers.
- Usual interger are say 1, 2, etc. And operators are available: +, -, *, /, and mod. It is of size 64 bit.
- Code example:
  ```ocaml
  65 / 60
  - : int = 1
  65 mod 60
  - : int = 5
  ```
  ### 2. Type float: Floating-point numbers. 
  - Syntactically, they must always contain a dot, for eg. **" 3.14 or 3.0 or even 3.. "** Operators are +., -., *., /.
  - There is not automatic comversion. We have to use built_in functions like **int_of_float, float_of_int.** There can be rounding errors also.
  ```ocaml
  3.14 *. (float_of_int 2)
  - : float = 6.28
  0.1 +. 0.2
  - : float = 0.300000000000000044
  ```

  ### 3. bool: Booleans
  - It give **true or false**. Some operators are: **&&, ||, >, <, =, ==** etc
  - Note: difference in = and == operator.
    - the = operator check structural equality. Works deeply, like a recursive comparison for compound data (lists, records, etc.).
    - the == operator physical equality(memory address)
    - More detailed: there are two equality operators in OCaml, = and ==, with corresponding inequality operators <> and !=. Operators = and <> examine structural equality whereas == and != examine physical equality.
    ```ocaml
    let x = [1; 2];;
    let y = x;;
    let z = [1; 2];;
    x == y  (* true — same object *)
    x = y (* true - same structure*)
    x == z (* false - different object*)
    x = z (* true - same structure*)
    ```
  ### 4. Type char: Characters
  - It is written with single quote 'a','b' etc. It is 8-bits. 
  - Some conversion: **char_of_int, int_of_char**.
  ### 4. Type string: Strings
    - Sequence of chars withing **" "** such as "abc".
    - The string concatenation operator is **^**
    - Some conversion: **string_of_int, string_of_float,  string_of_bool**. There is no **string_of_char**. But can use the library function, String.make to do it.
    - For the same three primitive types, there are built-in functions to convert from a string if possible: **int_of_string, float_of_string, and bool_of_string**. No char_of_string but we can access individual characters withh 0-based index.
    ```ocaml
    "abc" ^ "def"
    - : string = "abcdef"
    String.make 1 'z'
    - : string = "z"
    int_of_string "123"
    - : int = 123
    int_of_string "not an int"
    Exception: Failure "int_of_string".
    "abc".[0]
    - : char = 'a'
    "abc".[3]
    Exception: Invalid_argument "index out of bounds".
    ```
## Assertions
- It acts as a **mini runtime checkpoint** embedded int the code. The expressin **assert e** evaluates **e**. If **true** nothing happens, if **false** it raise exception and stop executon.
  ```ocaml
  let () = assert (f input1 = output1)
  (*Those assert that f input1 should be output1, and so forth. The let () = ... part of those is used to handle the unit value returned by each assertion.*)
  let f x = x + 1;;
  val f : int -> int = <fun>
  let () = assert (f 3 = 3);;
  Exception: Assert_failure ("//toplevel//", 1, 9).
  let () = assert (f 3 = 4);;
  ```
## If Expressions
- The expression **if e1 then e2 else e3** evaluates to  e2 if e1 evaluates to true, and to e3 otherwise. We call **e1** the guard of the if expression.
- It is similar to the ternary operator **? :**  and can be put anywhere.
- If expressions can be nested: ***if e1 then e2 else if e3 then e4 else ..... else en***. Here the last en is compulsory.

## Let Expression
-  It binds a value of an expression to a name. Various ways to use it:
    ```ocaml
    let x = 42;;
    val x : int = 42

    let x = 42 in x + 1
    - : int = 43

    (let x = 42) + 1
    Error: Syntax error 
    (* its because we don't provide complete info. we are providing only the binding part, without saying what to do after that.*)

    (let x = 42 in x) + 1
    - : int = 43
    (* its working beacause we give expression to do after the binding*)
    ```
  - A new binding of a variable shadows any old binding of the variable name. But eventually the old binding could reappear as the shadow recedes.

# FUNCTIONS
Theya are the buildig blocks of OCaml.
## Function Definition
- Definitions are not expressions, nor are expressions definitions—they are distinct syntactic classes. For example: let x = 42;; is not expressin but a function definition where value 42 is being bound to the name x;
- Recursive function are defined like this: ***let rec f x1 x2 ... xn = e***
- Mutually recursive functions can be defined with the ***and*** keyword:
    ```ocaml
    let rec even n = 
    n = 0 || odd (n-1)
    and 
    odd n = 
    n != 0 && even (n-1);;
    (*val even : int -> bool = <fun>
      val odd : int -> bool = <fun>*)
    ```
## Function application
- Function application in OCaml is written like `f x`, without parentheses.
- OCaml functions take exactly **one argument**, but through *currying*, they can appear to take multiple.
- The operator `|>` is **reverse function application**:
  ```ocaml
  e1 |> e2   ≡   e2 e1

## Anonymous Functions
- Like values are not needed to be bound to name, function also don't need to have name.
- ***fun*** is a keyword indicating an anonymous function. ***fun x1 ... xn -> e***
  ```ocaml
  let inc x = x + 1
  let inc = fun x -> x + 1
  val inc : int -> int = <fun>
  val inc : int -> int = <fun>
  (*They are syntactically different but semantically equivalent.*)
  ```
## Pipeline
- It is an infix operator and written as |> to make simplification in function applicatioon which use lots of parenthateis.
  ```ocaml
  let inc x = x + 1;;
  let square x = x * x;;
  5 |> inc |> square |> inc |> inc |> square;;
  square (inc (inc (square (inc 5))));;
  - : int = 1444
  - : int = 1444
  (* both of them are similar and produce the same result, the pipeline method is earsier to read*)
  ```
## Polymorphic funcitions
- Consider the code:
    ```ocaml
    let id x = x
    (* val id : 'a -> 'a = <fun> *)
    let id = fun x -> x

    id 42        (* int → 42 *)
    id true      (* bool → true *)
    id "hello"   (* string → "hello" *)
    ```
- Here 'a is a type variable, meaning id works for all types (int, bool, string, etc.).
- Type variables begin with ', e.g., 'a, 'b, 'c.
- **Restricting Polymorphism with Type Annotation:**
  ```ocaml
  let id_int (x : int) : int = x  (* val id_int : int -> int *)
  id_int true  (* Error: expects int, got bool *)

  (*We can assign a more restrictive type to a polymorphic function below:*)
  let id_int' : int -> int = id
  (*This is safe because we're discarding extra generality, not breaking any contracts.*)
  ```
## Partial Application
- Partial application = apply a function to fewer args than it expects.
  ```ocaml
    let add x y = x + y
    let add5 = add 5
    add5 2   (* 7 *)
    (*Equivalent codes*)
    let add x y = x + y
    let add x = fun y -> x + y
    let add = fun x -> fun y -> x + y
  ```
  
## Function Associativity
- **Core Idea:** Every OCaml function takes one argument and returns a function. So, 
  ```ocaml
  let f x y z = ...
  (* it is same as below*)
  let f = fun x -> (fun y -> (fun z -> ...))
  ```
- Function type associativity: 
***int -> int -> int -> int = int -> (int -> (int -> int))***
- Function application: f a b c = ((f a) b) c

## Operators as Functioons
- Operators can be treated as prefix functions:
  ```ocaml
  ( + ) 3 4       (* → 7 *)
  let add3 = ( + ) 3
  add3 2          (* → 5 *)
  (* Be cautious with (*)-(* is start of comment, so  needs to be written ( * ). *)
  ```
- You can define custom infix operators:
  ```ocaml
  let ( ^^ ) x y = max x y
  2 ^^ 3   (* 3 *)
  ```
## Tail Recursion
- **The Problem:** Recursive functions consume stack space:
  ```ocaml
  let rec count n =
    if n = 0 then 0 else 1 + count (n - 1)
  (*Fails for large n:*)
  count 1_000_000   (* Stack overflow *)
  ```
- Solution using tail recursive version:
  ```ocaml
  let rec count_aux n acc =
    if n = 0 then acc else count_aux (n - 1) (acc + 1)

  let count_tr n = count_aux n 0
  (*Recursive call is in tail position: last thing done in the function.*)
  (*Stack frame is reused = no stack overflow. The value is accumulated and the fuction need not to maintain stack frame*)
  ```
# Documenting OCaml Code with OCamldoc

- OCaml provides a built-in documentation tool called **OCamldoc**, similar to Java’s Javadoc.  
- It extracts **specially formatted comments** from source files and renders them as HTML for easier readability.

---

## How to Document:
  ```ocaml
  (** [sum lst] is the sum of the elements of [lst]. *)
  let rec sum lst = ...
  ```
- (** ... *) denotes an OCamldoc comment.

- [...] indicates inline code formatting (rendered as monospaced in HTML).

- Supported Tags:
OCamldoc supports various tags, similar to Javadoc:
- @author
- @param
- @return
- @deprecated
- @raise, etc.

- However, in idiomatic OCaml, these are used sparingly or avoided in favor of clear, declarative prose.

## What to Document
- OCaml favors a concise and declarative style, similar to its standard library.

- Good Style (Concise, Declarative):
  ```ocaml
  (** [sum lst] is the sum of the elements of [lst]. *)
  let rec sum lst = ...
  (*The doc starts with an example usage: sum lst.*)

  (*The word "is" emphasizes a mathematical description, not an imperative one.*)

  (*The comment uses actual argument names to explain behavior.*)
  ```


- Bad Style (Too Verbose, Redundant):
  ```ocaml
  (** Sum a list.
      @param lst The list to be summed.
      @return The sum of the list. *)
  let rec sum lst = ...
  (* This verbose style says no more than the concise version but is harder to read. *)
  ```

- Enhanced with Edge Case:
  ```ocaml
  (** [sum lst] is the sum of the elements of [lst].
      The sum of an empty list is 0. *)
  let rec sum lst = ...
  (*Clearly documents edge case behavior.*)```

## Preconditions and Postconditions
- These are concepts from formal methods:

  - **Preconditions**: What must be true before a function runs.

  - **Postconditions**: What is guaranteed after the function runs.

- Examples:
  ```ocaml
  (** [lowercase_ascii c] is the lowercase ASCII equivalent of character [c]. *)

  (** [index s c] is the index of the first occurrence of character [c] in string [s].
      Raises: [Not_found] if [c] does not occur in [s]. *)

  (** [random_int bound] is a random integer between 0 (inclusive) and [bound] (exclusive).
      Requires: [bound] is greater than 0 and less than 2^30. *)
    ```
- Interpretation:
  - The Raises: clause is a postcondition — it tells when the function will raise an exception.

  - The Requires: clause is a precondition — it tells what input constraints must be satisfied.

- What Not to Do:
Avoid writing obvious preconditions like:
  ```ocaml
  (** [lowercase_ascii c] is the lowercase ASCII equivalent of [c].
      Requires: [c] is a character. *)
  (* This is redundant and unidiomatic in OCaml.

  The type checker already enforces that c is a char, so this adds no value.*)
  ```
## Summary
  | Feature             | Description                                           |
| ------------------ | ----------------------------------------------------- |
| `(** ... *)`        | OCamldoc comment syntax                               |
| `[...]`             | Monospaced code formatting                            |
| Declarative style   | Start with `[function arguments] is ...`              |
| Avoid Javadoc style | No need for `@param` / `@return` unless truly helpful |
| Document edge cases | e.g., behavior on empty list                          |
| `Requires:` clause  | For true preconditions (not type info)                |
| `Raises:` clause    | Document exceptions and when they occur               |

# Printing Basics
- OCaml provides built-in printing functions for primitive types:
  ```ocaml
  print_char   : char -> unit
  print_string : string -> unit
  print_int    : int -> unit
  print_float  : float -> unit
  print_endline: string -> unit
  ```
- print_endline "hello" prints "hello" followed by a newline.

- All return type unit, which has only one value: () OCaml’s equivalent of void in Java or None in Python.

## Unit
- Type: unit = only one value ()
- Used when a function has side effects (like printing) but no meaningful return.
  ```ocaml
  print_endline "Hi"  (* : unit = () *)
  ```
## Semicolon (;)  Sequencing Expressions
- There are three idiomatic ways to sequence print statements:
  1. Using let _ = to discard result.
  2. Using let () = for unit-valued expressions.
  3. Idiomatic: using ; as separator.
    ```ocaml
    let _ = print_endline "Hello" in
    let _ = print_endline "World" in
    print_endline "!"

    let () = print_endline "Hello" in
    let () = print_endline "World" in
    print_endline "!"

    print_endline "Hello";
    print_endline "World";
    print_endline "!"
    ```
## Printf Module
- For formatted output, use ***Printf.printf***:
  ```ocaml
  Printf.printf "%s: %F\n%!" name num
  (* %s → string, %F → float (capital F for OCaml-style floats), \n → newline, %! → flush the output buffer*)
  ```
- ***sprintf*** returns a string instead of printing to console.
  ```ocaml
  let string_of_stat name num =
    Printf.sprintf "%s: %F" name num (* Output: "mean: 84.39" *)
  ```
## What is Pattern Matching?
- Pattern matching is a way to check the shape of data and extract values from it, all in one step.

- Think of it as a super-powered if-else or switch statement, but it can look inside data structures (like lists, tuples, or custom types) and pull out pieces for you.

- Simple Example: Numbers
  ```ocaml
  If n is 0, it returns "zero".
  If n is 1, it returns "one".
  The _ matches anything else.
  ```
- If `n` is `0`, it returns "`zero`".
- If `n` is `1`, it returns "`one`".
- The `_` matches anything else.
- **Example: Lists**
  ```ocaml
  let describe_list lst =
    match lst with
    | [] -> "empty list"
    | [x] -> "one element"
    | x :: y :: _ -> "at least two elements"
    ```
- `[]` matches an empty list.
- `[x]` matches a list with one element.
- `x :: y :: _ `matches a list with at least two elements.
- **Example: Tuples**
  ```ocaml
  let add_pair tup =
    match tup with
    | (x, y) -> x + y
  ```
- `(x, y)` matches a pair (tuple with two elements) and gives you `x` and `y`.
- **Example: Custom Types**
  ```ocaml
  type shape =
    | Circle of float
    | Rectangle of float * float

  let area s =
    match s with
    | Circle r -> 3.14 *. r *. r
    | Rectangle (w, h) -> w *. h
  ```
- `Circle r` matches a circle and gives you the radius.
- `Rectangle (w, h)` matches a rectangle and gives you width and height.
- **Why Use Pattern Matching?**
- Checks the shape of your data (is it empty? does it have two elements? is it a circle?).
- Extracts values for you (like x, y, r, w, h).
- Forces you to handle all cases (the compiler warns if you forget something).

# Type Synonyms

- A **type synonym** defines a new name for an existing type, improving code **readability** and **self-documentation**.

```ocaml
type point = float * float (* A 2D point as an (x, y) coordinate *)
type vector = float list   (* A vector as a list of floats *)
type matrix = float list list (* A matrix as a list of row vectors *)
```

- These synonyms do **not** create new types — they are **interchangeable** with the original types.

## Interchangeability

```ocaml
let get_x = fun (x, _) -> x

let p1 : point = (1., 2.)
let p2 : float * float = (1., 3.)

let a = get_x p1    (* OK *)
let b = get_x p2    (* OK *)
```

- The type `point` and `float * float` are treated **identically**.
- `get_x` works regardless of whether the argument is annotated as `point` or as `float * float`.