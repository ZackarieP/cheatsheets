# SML-NJ Syntax Cheatsheet

SML-NJ reference sheet for Dr Andrew Hilton's [course on compilers](https://adhilton.pratt.duke.edu/ece-553-compiler-construction).

## Types
The main primitives in SML are as follows:
```sml
"Hello World"   (*type string*)
3               (*type int*)
3.14            (*type real*)
true            (*type bool*)
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

### Let

Names bound with `let` are locally scoped and limited to the `let` expression in which they appear. Moreover, values bound with `let` cannot be rebound / "updated" after they have been bound to a name.

A common `let` usage pattern combines some names bound with `let` with the `in` and `end` keywords. Together, this can be interpreted as a series of local bindings, to be used `in` some expression. Those local bindings go out of scope and are no longer valid after the `end` of this scope.

```sml
let val <name> = <value>
    val <name> = <value>
    in
        <expression>
    end
```

### Recursion
Defining a tail-recursive function which takes a tuple of multiple arguments:
```sml
(*Usage: mult (a, b, 0) where the third argument is an accumulator*)
fun mult (0, b, acc) = acc
    | mult (a, b, acc) = mult (a - 1, b, acc + b)
```

Better idea: use `let` to define inner helper functions! Here is the tail-recursive `mult` from the previous code block, rewritten with `let` to abstract away the implementation, which uses an accumulator variable:
```sml
fun mult (a,b) = 
    let fun helper (0, acc) = acc
            | helper (ctr, acc) = helper (ctr - 1, acc + b)
    in
        helper (a,0)
    end
```

## Data Structures

### Tuples
Tuples are typed, fixed-length data structures which can contain arguments of any type. All functions take arguments and return values in the form of a single, typed tuple.

```sml
()              (*type unit, i.e. a zero-element tuple*)
(1, "hi")       (*type "int  * string", i.e. a tuple of int, string*)
```
Note that the `unit` type is analogous to `void` in C or `null` in Java and TypeScript. It signifies no return value. Functions with side effects but no return value will have a `unit` return type.

### Records
Records are tuples with named fields. (In fact, tuples also have fields named 1,2,3..., so technically tuples are special cases of records.) They are somewhat analogous to structs in C.
```sml
{a=1, b=3.14, c="hello"}        (*type {a::int, b::real, c::string}*)
```

To reference and extract fields by name with "field selection functions" with the following syntax: `#<field name> <record name>`
```sml
let val myrecord = {a=1, b=3.14, c="hello"}
    in
        #a myrecord
    end
```

Declaring function argument signatures using records and pattern-match on the values:
```sml
(*function definition*)
fun fib {x=0} = 1
    | fib {x=1} = 1
    | fib {x} = fib {x=x-1} + fib {x=x-2}

(*calling the function*)
fib {x=0}           (*evaluates to 1*)
fib {x=4}           (*evaluates to 5*)
```

### Lists
In SML-NJ, `list` is not a type itself, but rather a type constructor for types like `int list` (a list containing only `int`s) and `string list` (a list containing only `string`s).

Declaring a list:
```sml
[1,2,3]             (*using square brackets*)
1::2::3::[]         (*using the :: operator, which operates like a cons in Scheme*)
```

#### Polymorphism
Functions are assigned 'generic' types `'a` if their arguments could potentially be of different types. When inferring types, SML infers the most general type possible for a function.

There are also "eqtypes", which signify any type that can be compared for equality. For example, `int`s and `string`s can be compared for equality, while functions cannot.
