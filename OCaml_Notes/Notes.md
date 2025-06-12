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
