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

### Anonymous functions
We can define anonymous functions with the same fat-arrow syntax available in JavaScript, and using the keyword `fn`. The following anonymous function takes a single integer argument and returns its square:
```sml
(fn x => x*x)
```
This is especially useful for higher-order functions, which are introduced in a later section.

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

### Types
You can give types names in SML:
```sml
type someParam = {x:int,
                  y:real,
                  z:string}
```
This can be used to type records in the context of function arguments, thus telling the SML compiler what the types of record entries are.

### Lists
In SML-NJ, `list` is not a type itself, but rather a type constructor for types like `int list` (a list containing only `int`s) and `string list` (a list containing only `string`s).

Declaring a list:
```sml
[1,2,3]             (*using square brackets*)
1::2::3::[]         (*using the :: operator*)
```

The `::` operator operates like `cons x L` in Scheme—it draws a box with a value `x` and a pointer to the list `L`. We can write cons as follows:

```sml
fun cons (x, L) = x::L
```

#### Polymorphism
Functions are assigned 'generic' types `'a` if their arguments could potentially be of different types. When inferring types, SML infers the most general type possible for a function.

There are also "eqtypes", which refers to any type that can be compared for equality. For example, `int`s and `string`s can be compared for equality, while functions cannot.

### Defining your own datatypes

The general syntax for defining a datatype in SML is as follows:
```sml
datatype <datatype name> =
    <constructor name 1> of <type>
  | <constructor name 2> of <type>
  | <constructor name 3> of <type>
  ...
```

For example, lists in SML are defined as 
```sml
datatype 'a list =
    :: of 'a * 'a list 
  | nil
```

## Higher-order functions

SML provides `map`, `filter`, `foldr` and `foldl`.

### map
`map` takes two arguments, a function and a list, and applies the function to every member of the list. It returns a new list of the same length as the original list, but with the user-specified function applied to each element.

Syntax:
```sml
map f somelist
```

### filter
`filter` takes two arguments, a function and a list, and applies the function to every member of the list. Unlike `map`, the function must be a predicate: it must take a single argument and return a boolean value for that argument. It returns a new list that contains only the elements of the original list for which the predicate evaluated to `true`.

Syntax:
```sml
filter pred somelist
```

### foldr and foldl
`foldr` and `foldl` are highly flexible higher-order functions (that can be used to implement other higher-order functions such as `map` and `filter`). They both take three arguments:
1. A function specifying how a user-provided list is to be combined
2. A user-provided list
3. An accumulator value whose initial value must be specified when `foldr` or `foldl` is called

`foldr` and `foldl` can be thought of as "accumulators": given a list of values, they "accumulate" those values into a single return value. This return value could be anything, and is determined by the user-provided function.

Syntax:
```sml
foldl f accum somelist
foldr f accum somelist
```

## Structures
Structures are a means of packaging types and values into a namespace. They are *not* like classes in object-oriented programming languages. 

Syntax for declaring a structure:
```sml
structure myStructure =
    struct
        datatype <name> =
            <constructor 1> of <type>
          | <constructor 2> of <type>
            ...

    fun foo (bar) = 
            <expr>
          | <expr>

    fun baz (qux) = 
            <expr>
          | <expr>

end

(* Syntax for accessing properties inside a structure: *)
myStructure.baz

(* Syntax for loading a module *)
open myStructure
baz             (* You can then directly reference members *)
```

## Signatures

Signatures are analogous to interfaces in TypeScript or Java. They specify an interface to a structure, including constructors and member functions, that the programmer can then implement however they wish. Once a signature has been ascribed to an interface, all other names that are not declared in the interface are hidden.

There are two kinds of signature ascription in SML:
1. Transparent: the types of the names in the structure are visible
2. Opaque: the types of the names in the structure are not visible

The syntax for declaring each is as follows:
```sml
structure FooBar : Foo          (* Transparent *)
structure FooBar :> Foo         (* Opaque *)
```

Opaque signature ascription is generally preferred unless the types of the names in the structure do matter.

## Functors
Functors take structures as arguments and also return structures. This enables us to instantiate some structure `Foo` whose functionality depends on structures `Bar`, `Baz`, etc. that all conform to some specified signature `Qux`. The resultant functor returns instances of `Foo` and only references signature `Qux` without directly referencing `Bar` or `Baz`. This decouples the creation of `Foo` from whichever underlying structure was used in its construction, leading to more modular and maintainable code.

Syntax:
```sml
functor myFunctor (x: SOME_TYPE) :> SomeSig
    ...
```

## References
[Functors](http://www.cs.cornell.edu/courses/cs312/2006fa/recitations/rec08.html)