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

Since we know that the memory is sequential, we can assume that if we move ahead in the memory to the next character, we'll receive the next letter in the string, until we reach the end of the string, marked with a **null terminator** (the character with the ordinal value of 0. noted as `\0`).

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
C structures are large variables which contain several named variables inside. Structures are the basic foundation for *objects* and *classes* in C. Structures are used for:
- Serialization of data
- Passing multilple arguments in and out of functions through a sungle argument
- Data structures such as linked lists, binary trees, and more

The most basic example of structures are **points**, which are a single entity that contains two variables - `x` and `y`. Let's define a point:
```
struct point {
    int x;
    int y;
}
```
Now, lets define a new point, and use it. Assume the function `draw` receive a point and draws it on a screen. Without structs, using it would require two arguments - each for every coordinate:
```
int x = 10;
int y = 5;
draw (x, y);
```
Using structs, we can pass a point argument:
```
struct point p;
p.x = 10;
p.y = 5;
draw(p);
```
To access point variables, we use the dot `.` operator.

### Typedefs
Typedefs allow us to define types with a different name. This can come in handy when dealing with structs and pointers. In this case, we'd want to get rid of the long definition of a point structure. We can use the following syntax to remove the `struct` keyword from each time we want to define a new point:
```
typedef struct {
    int x;
    int y;
} point;
```
This will allow us to define a new point like this:
```
point p;
```

-----
I think that this is a really cool function of C. Being able to simplify type definition can lead to a lot cleaner code. I do wonder however how much it is used in real world code. Does this affect readability of code, particularly for new people coming onto a project or does that issue get resolved with proper documentation?
-----
Structures can also hold pointers - which allows them ot hold strings or pointers to other structures as well - which is their real power. For example, we can define a vehicle structure in the following manner:
```
typedef struct {
    char * brand;
    int model;
} vehicle;
```
Since brand is a char pointer, the vehicle type can contain a string.
``` 
vehicle mycar;
mycar.brand = "Ford";
mycar.model = 2007;
```

## Function Arguments by Reference
If we understand that function arguments are passed by value, by which means they are compied in and out of functions. What if we pass pointers to values instead of the values themselves? This will allow us to give functions control over the variables and structures of the parent functions and not just a copy of them, thus directly reading and writing the original object.

Lets say we want to write a function which increments a number by one, called `addone`. This will not work:
```
void addone(int n) {
    // n is local variable which only exists within the function scope
    n++; // therefore incrementing it has no effect
}

int n;
printf("Before: %d\n", n);
addone(n);
printf("After: %d\n", n);
```
However, this will work:
```
void addone(int *n) {
    // n is a pointer here which point to a memory-adress outside the function scope
    (*n)++; // this will effectively increment the value of n
}

int n;
printf("Before: %d\n", n);
addone(&n);
printf("After: %d\n", n);
```
The difference between these two code blocks is that in the second version of `addone`. The function receives a pointer to the variable `n` as an argument, and then it can manipulate it, because it knows where it is in memory.
Notice that when we called the `addone` function, we **must** pass a reference to the variable `n` (`addone(&n)`), and not the variable itself - this is done so that the funciton knows the address of the variable, and won't just receive a copy of the variable itself.

### Pointers to structures
Let's say we want to create a function which moves a point forward in both `x` and `y` directions, called `move`. Instead of sending two pointers, we can send only one pointer to the function of the point structure:
```
void move(point * p) {
    (*p).x++;
    (*p).y++;
}
```
However, if we wish to dereference a structure and access one of it's internal members, we have a shorthand syntax for that, because this operation is widely used in data structures. We can rewrite this function using the following syntax:
```
void move(point * p) {
    p->x++;
    p->y++;
}
```

## Dynamic allocation
Dynamic allocation allows us to build complex data structures such as linked lists. Allocating memory dynamically helps us to store data without initially knowing the size of the data in the time we wrote the program.

To allocate a chunk of memory dynamically, we need to have a pointer ready to store the location of the newly allocated memory. We can access memory that was allocated to us using that same pointer, and we can use that pointer to free the memory again, once we have finished using it.

Let's assume we want to dynamically allocate a person structure. The person is defined like this:
```
typedef struct {
    char * name;
    int age;
} person;
```
To allocate a new person in the `myperson` argument, we use the following syntax;
```
person * myperson = (person *) malloc(sizeof(person));
```
This tells the compiler that we want to dynamically allocate just enough to hold a person struct in memory and then return a pointer of type `person` to the newly allocated data. The memory allocation function `malloc()` reserves the specified memory space. In this case, this is the size of `person` in bytes.

The reason we write `(person *)` before the all to `malloc()` is that `malloc()` return a "void pointer", which is a pointer that doesn't have a type. Writing `(person *)` in front of it is called *typecasting*, and changes the type of the pointer return from `malloc()` to be `person`. However, it isn't strictly necessary to write it like this as C will implicitly convert the type of the return pointer to that of the pointer it is assigned to if you don't typecast it.

Note that `sizeof` is not an actual function, because the compiler interprets it and translates it to the actual memory size of the person struct. 

To access the person's members, we can use the `->` notation:
```
myperson->name = "John";
myperson->age = 27;
```

After we are done using the dynamically allocated struct, we can release it using `free`:
```
free(myperson);
```

Note that the free does not delete the `myperson` variable itself, it simply releases the data that it points to. The `myperson` variable will still point to somewhere in memory - but after calling `myperson` we are not allowed to access that area naymore. We must not use that pointer again until we allocate new data using it.

## Arrays and Pointers