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

## Recursion
Recursion occurs when a function contains within it a call to itself. Recursion can result in very neat, elegant code that is intuitive to follow. Though it can also result in a very large amount of memory being used if the recursion gets too deep.

Common examples of where recursion is used:
- Walking recursive data structures such as linked lists, binary trees, etc.
- Exploring possible scenarios in games such as chess

Recursion always consists of two main parts. A terminating case that indicates when the recursion will finish and a call to itself that must make progress towards the terminating case.

For example, this function will perform multiplication by recursively adding:
```
#include <stdio.h>

unsigned int multiply(unsigned int x, unsigned int y)
{
    if (x == 1)
    {
        /* Terminating case */
        return y;
    }
    else if (x > 1)
    {
        /* Recursive step */
        return y + multiply(x-1, y);
    }

    /* Catch scenario when x is zero */
    return 0;
}

int main() {
    printf("3 times 5 is %d", multiply(3, 5));
    return 0;
}
```

## Linked Lists
Linked lists are the best and simplest example of a dynamic data structure that uses pointers for its implementation. However, understanding pointers is cruitical to understanding how linked lists work. You must also be familar with dynamic memory allocation and structures.

Essentially, linked lists function as an array that can grow and shrink as needed, from any point in the array.

Linked lists have a few advantages over arrays:
- Items can be added or removed from the middle of the list
- There is no need to define an initial size

However, linked lists also have a few disadvantages:
- There is no "random" access - it is impossible to reach the nth item in the array without first iterating over all items up until that item. This means we have to start from the beginning of the list and count how many times we advance in the list until we got to the desired item.
- Dynamic memory allocation and pointers are required, which complicates the code and increases the risk of memory leaks and segment faults.
- Linked lists have a much larger overhead over arrays, since linked lists items are dynamically allocated (which is less effeciency in memory usage) and each item is the list also must store an additional pointer.

### What is a linked list?
A linked list is a set of dynamically allocates nodes, arranged in such a way that each node contains one value and one pointer. The pointer always points to the next member of the list. If the pointer is NULL, then it is the last node in the list.

A linked list is held using a local pointer variable which points to the first item of the list. If that pointer is also NULL, then the list is considered to be empty.

Let's define a linked list node:
```
typedef struct node {
    int val;
    struct node * next;
} node_t;
```

Notice that we are defining the struct in a recursive manner, which is possible in C. Let's name our node type `node_t`.

Now we can use the nodes. Let's create a local variable which points to the first item of the list (called `head`).
```
node_t * head = NULL;
head = (node_t *) malloc(sizeof(node_t));
if (head == NULL) {
    return 1;
}

head->val = 1;
head-> next = NULL;
```

We've just create the first variable in the list. We must set the value, and the next item to be empty, if we want to finish populating the list. Notice that we should always check if malloc return a NULL value or not.

To add a variable to the end of the list, we can just continue advancing to the next pointer:
```
node_t * head = NULL;
head = (node_t *) malloc(sizeof(node_t));
head->val = 1;
head->next = (node_t *) malloc(sizeof(node_t));
head->next->val = 2;
head->next->next = NULL;
```

This can go on and on, but wwhat we should actually do is advance to the last item of the list, until the `next` variable will be `NULL`.

### Iterating over a list
Let's build a function that prints out all the items of a list. To do this, we need to use a `current` pointer that will keep track of the node we are currently printing. After printing the value of the node, we set the `current` pointer to the next node, and print again, until we've reached the end of the list (the next node is NULL).
```
void print_list(node_t * head) {
    node_t * current = head;

    while (current != NULL) {
        printf("%d\n", current->val);
        current = current->next;
    }
}
```

### Adding an item to the end of the list
To iterate over all members of the linked list, we use a pointer called `current`. We set it to start from the head and then in each step, we advnace the pointer to the next item in the list, until we reach the last item.
```
void push(node_t * head, int val) {
    node_t * current = head;
    while (current->next != NULL) {
        current = current->next;
    }

    /* now we can add a new variable */
    current->next = (node_t *) malloc(sizeof(node_t));
    current->next->val = val;
    current->next->next = NULL;
}
```

### Adding an item to the beginning of the list (pushing to the list)
To add to the beginning of the list, we will need to do the following: 
- Create a new item and set it's value
- Link the new item to the pointe ro the head of the list
- Set the head of the list to be our new item

This will effecively create a new head to the list with a new value, and keep the rest of the list linked to it.

Since we use a function to do this operation, we want to be able to modify the head variable. To do this, we must pass a pointer variable (a double pointer) so we will be able to modify the pointer itself.
```
void push(node_t ** head, int val) {
    node_t * new_node;
    new_node = (node_t *) malloc(sizeof(node_t));

    new_node->val = val;
    new_node->next = *head;
    *head = new_node;
}
```

### Removing the first item (popping from the list)
To pop a variable, we will need to reverse this action:

- Take the next item that the head points to and save it
- Free the head item
- Set the head to be the next item that we've stored on the side

Here is the code:
```
int pop(node_t ** head) {
    int retval = -1;
    node_t * next_node = NULL;

    if (*head == NULL) {
        return -1;
    }

    next_node = (*head)->next;
    retval = (*head)->val;
    free(*head);
    *head = next_node;

    return retval;
}
```

### Removing the last item of the list
Removing the last item from a list is very similar to adding it to the end of the list, but with one big exception - since we have to change one item before the last item, we actually have to look two items ahead and see if the next item is the last one in the list:
```
int remove_last(node_t * head) {
    int retval = 0;
    /* if there is only one item in the list, remove it */
    if (head->next == NULL) {
        retval = head->val;
        free(head);
        return retval;
    }

    /* get to the second to last node in the list */
    node_t * current = head;
    while (current->next->next != NULL) {
        current = current->next;
    }

    /* now current points to the second to last item of the list, so let's remove current->next */
    retval = current->next->val;
    free(current->next);
    current->next = NULL;
    return retval;

}
```

### Removing a specific item
To remove a specific item from the list, either by its index from the beginning of the list or by its value, we will need to go over all the items, continuously looking ahead to find out if we've reached the node before the item we wish to remove. This is because we need to change the location to where the previous node points to as well.

Here is the algorithm:
- Iterate to the node before the node we wish to delete
- Save the node we wish to delete in a temporary pointer
- Set the previous node's next pointer to point to the node after the node we wish to delete
- Delete the node using the temporary pointer

There are a few edge cases we need to take care of, so make sure you understand the code.
```
int remove_by_index(node_t ** head, int n) {
    int i = 0;
    int retval = -1;
    node_t * current = *head;
    node_t * temp_node = NULL;

    if (n == 0) {
        return pop(head);
    }

    for (i = 0; i < n-1; i++) {
        if (current->next == NULL) {
            return -1;
        }
        current = current->next;
    }

    if (current->next == NULL) {
        return -1;
    }

    temp_node = current->next;
    retval = temp_node->val;
    current->next = temp_node->next;
    free(temp_node);

    return retval;

}
```

## Unions
C Unions are essentially the same as C Structures, except that instead of containing multiple variables each with their own memory a Union allows for multiple names to the same variable. These names can treat the memory as different types (and the szie of the unuin will be the size of the largest type, + any padding the compiler might decide to give it)

So if you wanted to be able to read a variable's memory in different ways, for example read an integer one byte at a time, you could have something like this:
```
union intParts {
    int theInt;
    char bytes[sizeof(int)];
};
```

Allowing  you to look at each byte individually without casting a pointer and using pointer arithmetic:
```
union intParts parts;
parts.theInt = 5968145;

printf("The int is %i\nThe bytes are [%i, %i, %i, %i]\n",
parts.theInt, parts.bytes[0], parts.bytes[1], parts.bytes[2], parts.bytes[3]);

// vs

int theInt = parts.theInt;
printf("The int is %i\nThe bytes are [%i, %i, %i, %i]\n",
theInt, *((char*)&theInt+0), *((char*)&theInt+1), *((char*)&theInt+2), *((char*)&theInt+3));

// or with array syntax which can be a tiny bit nicer sometimes

printf("The int is %i\nThe bytes are [%i, %i, %i, %i]\n",
    theInt, ((char*)&theInt)[0], ((char*)&theInt)[1], ((char*)&theInt)[2], ((char*)&theInt)[3]);
```

Combining this with a structure allows you to create a "tagged" union which can be used to store multiple different types, one at a time.

For example, you might have a "number" struct, but you don't want to use something like this:
```
struct opertor {
    int intNum;
    float floatNum;
    int type;
    double doubleNum;
};
```

Because your program has a lot of them and it takes a bit too much memory for all of the variables, so you could use this:
```
struct operator {
    int type;
    union {
        int intNum;
        float floatNum;
        double doubleNum;
    } types;
};
```

Like this the size of the struct is just the size of the int `type` + the size of the largest type in the union (the double). Not a huge gain, only 8 or 16 bytes, but the concept can be applied to similar structs.

Use:
```
operator op;
op.type = 0; // int, probably better as an enum or macro constant
op.types.intNum = 352;
```

Also, if you don't give the union a name then it's members are accessed directly from the struct:
```
struct operator {
    int type;
    union {
        int intNum;
        float floatNum;
        double doubleNum;
    }; // no name!
};

operator op;
op.type = 0; // int
// intNum is part of the union, but since it's not named you access it directly off the struct itself
op.intNum = 352;
```

Another, perhaps more useful feature, is when you always have multiple variables of the same type, and you want to be able to use both names (for readability) and indexes (for ease of iteration), in that case you can do something like this:
```
union Coins {
    struct {
        int quarter;
        int dime;
        int nickel;
        int penny;
    }; // anonymous struct acts the same way as an anonymous union, members are on the outer container
    int coins[4];
};
```

In that example you can see that there is a struct which contains the four (common) coins in the United States. 

Since the union makes the variables share the same memory the coins array matches with each int in the struct (in order):
```
union Coins change;
for(int i = 0; i < sizeof(change) / sizeof(int); ++i)
{
    scanf("%i", change.coins + i); // BAD code! input is always suspect!
}
printf("There are %i quarters, %i dimes, %i nickels, and %i pennies\n",
    change.quarter, change.dime, change.nickel, change.penny);
```