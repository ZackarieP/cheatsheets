# C Syntax Cheatsheet

## Data types
### Pointers
Pointers are variables that hold the memory address of some value that we're interested in.
The & operator returns a variable's memory address.
The * operator returns the value stored at a memory address.
```cpp
int a = 1;
// Declare an int pointer whose value is the address of a. Access the address of a regular variable with the & operator
int* a_pointer = &a; 
// Dereference the pointer to get the value stored at the memory address it holds
*a_pointer += 1; 
```
A common mistake when using pointers is to declare "dangling pointers," or pointers that reference invalid locations in memory (e.g. by making the pointer static, or by pointers referencing locations in memory that have been `free()`ed). That memory location will either be invalid, or overwritten with new data from some other function call. This results in segfaults or the dangling pointer referencing invalid data, with unexpected consequences for your program's execution.
```cpp
// A contrived example
void func() {
    int a = 1;
    static int pointer* a_pointer = &a;
}

void another_func() {
    func();
    *a_pointer +=1; // func() has returned, a_pointer now points to an invalid memory location
    printf("%d\n", *a_pointer); // Value != 2, will cause a segfault or some very unexpected program behavior
}
```

### Structs
The C analogue to classes in object-oriented languages like Java is structs. Structs can be defined and then used like so:

```cpp
// This defines a struct called person
struct person {
    int age;
    char* name;
}

// We can "instantiate" person "objects"
struct person my_person;
my_person.age = 10;
my_person.name = "Ben";
```

Since repeatedly declaring a struct pointer of type `struct node` is verbose, we can use the `typedef` keyword to create aliases for structs:

```cpp
typedef struct person {
    int age;
    char* name;
} t_person;

// Now we can use the aliased t_person instead of writing `struct person`
t_person another_person;
another_person.age = 12;
another_person.name = "Cameron";
```

We can create pointers to structs and manipulate them:
```cpp
person my_person;
my_person.age = 22;
my_person.name = "Joyce";

person* person_pointer = &my_person;
// These two lines are equivalent
(*person_pointer).age++;
person_pointer->age++; // Shorthand for dereferencing a struct pointer and accessing a property within the struct
```

### Strings
All strings in C are arrays of `char`s. When a string is initialized and assigned to a variable, that variable is really a pointer to the first character in the array.
```cpp
char* readonly_string = "Hello";
char editable_string[] = "World";
```

Usage of some built-in C functions for working with strings is illustrated below.
```cpp
int string_length = strlen(readonly_string); // Value = 5
bool comp = strncmp(readonly_string, editable_string); // Value = false
char* concat = strncat(readonly_string, ", ", editable_string);
```

### Arrays
Arrays are blocks of contiguous memory that store the same datatype. Their length must be known at the time that they are instantiated.

## Function anatomy
All functions must declare their return type, as well as the types of their arguments.
```cpp
// This function doesn't return anything
void fun(int num) {
    printf("%d\n", num)
}

// This function returns an int
int main(int arg1) {
    fun(arg1);
    return 0;
}

main(1)
```

### Static functions
In C, functions are globally scoped by default. `static` functions are scoped to only the file that they're declared in.

### Passing arguments by reference
In any other language, we're used to passing arguments by *value*. In C, you can also pass arguments by *reference*:

```cpp
void fun(int* pointer) {
    (*pointer)++; // Dereference the pointer with *, and increment the value at the address held by the pointer
}

int a = 0;
fun(&a); // Pass in the address of a rather than the value held in a
printf("%d\n", a); // Value is now 1
```

## Control flow
### If statements
```cpp
if (cond_1) {
    // Do something
} else if (cond_2) {
    // Do something else
} else {
    // Catch all
}
```

### For loops
```cpp
for (int i = 0; i < 10; i++) {
    printf("%d\n", i);
}
```

### While loops
```cpp
int n = 10;
while (n > 0) {
    n--;
}
```
To further manipulate the control flow of a function, use `continue` to skip the rest of the loop, and `break` to terminate the loop.
