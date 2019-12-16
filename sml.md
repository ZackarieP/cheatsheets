# SML-NJ Syntax Cheatsheet

SML-NJ reference sheet for Dr Andrew Hilton's [course on compilers](https://adhilton.pratt.duke.edu/ece-553-compiler-construction).

## Types
The main primitives in SML are as follows:
```sml
"Hello World"   (*type string*)
3               (*type int*)
3.14            (*type real*)
true            (*type bool*)
(1, "hi")       (*type "int  * string", i.e. a tuple of int, string*)
```

## Operands
```sml
<               (*less than*)
>               (*greater than*)
<=              (*less than or equal to*)
>=              (*greater than or equal to*)
=               (*equals*)
<>              (*not equals*)
not             (*negate operator*)
andalso         (*logical and*)
orelse          (*logical or*)
```

## Control flow
If/else syntax:
```sml
if <boolean expression> then "hello" else "goodbye"
```
Switch statement syntax (expressions returned must be of the same type):
```sml
case <expr> of
    pattern1 => "result 1"
|   pattern2 => "result 2"
|   pattern3 => "result 3"
|   _ => "catchall"
```

## Functions
All functions in SML take and return one value. Arguments and return values can be a single primitive, or a single tuple containing as many elements as is needed.
Defining a function:
```sml
fun square x = x * x
```

Calling a function:
```sml
square 2            (*evaluates to 4*)
```

Calling a function which takes a tuple of multiple arguments:
```sml
fun add (x,y) = x + y
add (2,3)           (*evaluates to 5*)
```

### Recursion
Defining a tail-recursive function which takes a tuple of multiple arguments:
```sml
(*Usage: mult (a, b, 0) where the third argument is an accumulator*)
fun mult (0, b, acc) = acc
    | mult (a, b, acc) = mult (a - 1, b, acc + b)
```

Better idea: use `let` to define inner helper functions. A note on binding scopes: names bound with `let` are locally scoped and limited to the `let` expression in which they appear. Moreover, values bound with `let` cannot be rebound / "updated" after they have been bound to a name.

Here is the tail-recursive `mult` from the previous code block, rewritten with `let` to abstract away the implementation, which uses an accumulator variable:
```sml
fun mult (a,b) = 
    let fun helper (0, acc) = acc
            | helper (ctr, acc) = helper (ctr - 1, acc + b)
    in
        helper (a,0)
    end
```