# week 1 of ocaml

# General Introduction 🤓

> source -> https://www.youtube.com/watch?v=A5IHFZtRfBs&list=PLre5AT9JnKShBOPeuiD9b-I4XROIJhkIU&index=6

### Five aspects of learning a PL

1. Syntax: How do you write language constructs?
2. Semantics: What do programs mean? (Type checking, evaluation rules)
3. Idioms: What are typical patterns for using language features to express your computation?
4. Libraries: What facilities does the language (or a third-party project) provide as "standard"? (E.g., file access, data structures)
5. Tools: What do language implementations provide to make your job easier? (E.g., top-level, debugger, GUI editor, ...)

# Expressions

### Every kind of expression has:

• Syntax
• Semantics:

- Type-checking rules (static semantics): produce a
  type, or fail with an error message
- Evaluation rules (dynamic semantics): produce a
  value, or exception or infinite loop

## Values

> A value is an expression that does not need any
> further evaluation

Note : we read an expression from right to left

```OCaml
utop # 3110;;
- : int = 31100

(* booleans in ocaml *)
utop # true;;
- : bool = false

(* String in ocaml with concatination *)
utop # "big" ^ "red";;
- : string = "bigred"


(* type float : notice the error below *)
utop # 2.0 * 3.14;;
Error: This expression has type float but an expression was expected af type int
(* so for floating point operator we need *. dot after operators *)


utop # 2.0 *. 3.14;;
- : float = 6.28

utop # -3.0 /. 4.0;;
- : float = -0.75

utop # "So" ^ " " " "it" ^ " " ~ "goes";;
- : string = "So it goes"

utop # 1 > 2 || not (2.0 < 1.0);;
- :bool = true

```

## Type inference and annotation

• OCaml compiler infers types

- Compilation fails with type error if it can't (type checking is done first)
- Hard part of language design: guaranteeing compiler can infer types when program is correctly written

• You can manually annotate types anywhere

- Replace **_e_** with **_(e : t)_**
- Useful for diagnosing type errors

```Ocaml
utop # (3110 : int);;
- : int = 3110

(* altough we cannot convert these types so:👇  *)
utop # (3110 : bool);;
Error: This expression has type int but an expression was expected of type bool



```

# Definations of Values

- By means of let, a variable can be assigned a value.
- The variable retains this value for ever!

```Ocaml
utop # let seven = 3 + 4;;
val seven : int = 7

utop # seven;;
- : int = 7

```

<h4 style="color:red;">Caveat: Variable names are start with a small letter !!!</h4>

<h5>🚨 Another definition of seven does not assign a new value to seven, but creates a new
variable with the name seven.</h5>

```Ocaml
# let seven = 42;;
val seven int = 42

# seven;;
-:int = 42

# let seven = "seven";;
val seven : string = "seven"
(*  The old variable is now hidden (but still there)!
Apparently, the new variable may even have a different type. *)
```

# Let expressions

Here we define lets as expressions and not definition so we can do more complex actions at once 🚀

```Ocaml
utop # let a = 0 in a;;
- : int = 0

utop # let b = 1 in 2 * b;;
- : int = 2

utop # a;;
Error: Unbound value a
(* now a and also b are outside the scope *)

utop # let c = 3 in (let d = 4 in c + d);;
- : int = 7


utop # let e = 5 in (e = 6 in e);;
- : int = 6

```

# Overlapping scopes

<h5 style="color:#DD664D">Principle if Name Irrelevance</h5>

> the name of variable shouldn't intrinsically matter

To ensure name irrelevance in programs, stop substituting when you reach a binding of the same name

```Ocaml
utop # let x = 5 in ((let x = 6 in x) + x);;
- : int = 11
```

## Conditionals

```Ocaml
utop # if true then "yay" else "boo";;
- : string = "boo"

(* But we cant use different types as the results *)
utop # if true then "yay" else 1;;
Error: This expression has type int but an expression was expected of type
         string

```

## if expressions

#### Syntax:

```Ocaml
if el then e2 else e3
```

> Evaluation:

- if **el** evaluates to true, and if **e2** evaluates to v, then if el then e2 else e3 evaluates to v
- if **el** evaluates to false, and if **e3** evaluates to v, then if el then e2 else e3 evaluates to v
<p style="color:red">Type checking:
if el has type bool and e2 has type t and e3 has type t
then if el then e2 else e3 has type t</p>

# functions 🥳

> Functions are values !

Can use them anywhere we use values:
• Functions can take functions as arguments
• Functions can return functions as results

#### syntax: anonymous functions

<p style="color:#DD664D">Syntax: <span style="color:#000000"> fun x1 ...... xn -> e </span> </p>

##### ! Notice no commas in the function arguments

```Ocaml
utop # (fun x -> x +1);;
- : int -> int = <fun>
(* the "<" bracket means utop cant print the value of this type , becaues this is already compiled and exists as bits of memory and no longer in original form *)
```

### Function application

Evaluation of e0 el ... en:

1. Evaluate subexpressions: e0 ==> v0, • , en ==> vn v0 must be a function: fun x1 ... xn -> e
2. Substitute vi for xi in e yielding new expression e'. Evaluate it: e' ==> v
3. Result is v

```Ocaml
(fun x -> x + 1) (2+3)
(* step 1 is done since the anonym func is already a value *)

(fun x -> x + 1) (5)
(* also evaluate the argument values here (2+3) = 5 *)

5 + 1 
(* step 2: subsitute the argument to the func *)

6
(* step 3: 6 is result *)
```

```Ocaml
(* One more example *)
(fun x y -> x - y) (3*1) (3-1)
(fun x y -> x - y) (3) (2)
3 - 2
1
```

## Named functions
Latter is ***syntactic sugar***
• not necessary
• makes language "sweeter"
```Ocaml
utop # let square = fun x -> x * x;;
val square : int -> int = <fun>

(* simplified *)
utop # let square x = x * x;;
val square : int -> int = <fun>

```