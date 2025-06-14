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