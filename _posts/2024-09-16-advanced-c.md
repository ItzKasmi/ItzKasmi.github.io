## Advanced C

## Introduction

In my last post I learned all about the basics of C. How are variables initialized, what are types in C, static vs global, and more... Today I'm going to be learning about more complicated subjects in C. Things like pointers, structures, dynamic allocation, and more. I am following [**learn-c.org**](https://learn-c.org)'s advanced tutorial series. Please check them out for a more in-depth and detailed guide!

## Pointers
Pointers are also variables and play an important role in C. They are used for several reasons such as:
- Strings
- Dynamic memory allocation
- Sending function arguments by reference
- Building complicated data structures
- Pointing to functions
- Building special data structures (i.e. Tree, Tries, etc...)
- and more.

### What is a pointer?
A pointer is an integer variable which holds a **memory address** that points to a value, instead of holding the actual value itself.
The computer's memory is a sequential store of data, and a pointer points to a specific part of the memory. Our program can use pointers in such a way that the pointers point to a large amount of memory. Depending on how much we decide to read from that point on.

### Strings as pointers
Strings in C (Referred to as C-Strings to differentiate them from other strings when mixed with C++)
The following line:
```
char * name = "John";
```
does three things:
- It allocates a local (stack) variable called `name`, which is a pointer to a single character.
- It causes the string "John" to appear somewhere in the program memory (after it is compiled and executed, of course).
- It initializes the `name` argument to point to where the `J` character resides at (which is followed by the rest of the string in memory).

If we try to access the `name` variable as an array, it will work, and will return the ordinal value of the character `J`, since the `name` variable actually points exactly to the beginning of the string.

Since we know that the memory is sequential, we can assume that if we move ahead in the memory to the next character, we'll receive the next letter in the string, until we reach the end of the string, marked wil a **null terminator** (the character with the ordinal value of 0. noted as `\0`).

### Dereferencing
Dereferencing is the act of referring to where the pointer points, instead of the memory address. We are already using dereferencing in arrays, but we just didn't know it yet. The brackets operator - `[0]` for example, accesses the first item of the array. And since arrays are actually pointers, accessing the first item in the array is the same as dereferncing a pointer. Dereferencing a pointer is done using the asterisk operator `*`.

If we want to create an array that will point to a different varaible in our stack, we can write the following code:
```
/* define a local variable a */
int a = 1;

/* define a pointer variable, and point it to a using the & operator */
int * pointer_to_a = &a;

printf("The value a is %d\n", a);
printf("The value of a is also %d\n", *pointer_to_a);
```
Notice we used the `&` operator to point at the variable `a`, which we have just created.
We then reffered to it using the derefernecing operator. We can also change the contents of the dereferenced variable:
```
int a = 1;
int * pointer_to_a = &a;

/* let's change the variable a */
a += 1;

/* we just changed the variable again! */
*pointer_to_a += 1;

/* will print out 3 */
printf("The value of a is now %d\n", a);
```

## Structures