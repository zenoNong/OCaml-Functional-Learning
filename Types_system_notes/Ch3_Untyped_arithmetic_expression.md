# Introduction
It is an introduction of a language called the untyped calculus of booleans and numbers, that only has:

- Boolean constants: true, false
- Conditional expressions: if t then t else t
- The number zero: 0
- Arithmetic operations: succ t (successor), pred t (predecessor)
- A test: iszero t (checks if a number is zero)
- This language is called the untyped calculus of booleans and numbers.

## Why use such a simple language?
- It’s simple enough to focus on the core ideas of programming language theory, like syntax, semantics, and evaluation.
- It helps introduce fundamental concepts such as:
    - Abstract syntax (the structure of programs)
    - Inductive definitions and proofs (defining things recursively and proving properties)
    - Evaluation (how programs compute results)
    - Modeling errors (handling nonsensical programs)

## How is the language defined?
The language’s grammar is written in a style similar to BNF (Backus-Naur Form), a standard way to describe programming languages. For example:
```ocaml
t ::= true
    | false
    | if t then t else t
    | 0
    | succ t
    | pred t
    | iszero t
```
- t is a metavariable: a placeholder for any term in the language.
- Each line describes a different way to build a term.
- Metavariables (like t, s, u) are used in the description of the language, not in the language itself.
- They help us talk about the structure of programs in a general way.

## What is a program in this language?
- A program is just a term built using the rules above.
- Example programs:
    - if false then 0 else 1 evaluates to 1
    - iszero (pred (succ 0)) evaluates to true

## How are numbers represented?
- Numbers are represented as nested succ applied to 0.
    - For example, 3 is written as succ (succ (succ 0)).
- For convenience, examples use regular numerals (1, 2, 3), but formally, they are built from succ and 0.

## What are values?
- Values are the simplest results of evaluation:
    - Boolean constants (true, false)
    - Numbers (any number of succ applied to 0)