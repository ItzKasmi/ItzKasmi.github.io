## Lets learn about C!

## Introduction
Today, I learned the basics of C. Luckily, it wasn't too difficult to pick up due to my experience with other languages! Even if you've never worked in C before I hope this guide will prove as a nice introduction.

### What is C?
C, is a general purpose programming langauge. What makes C unique is that it is considered a low level language. A low level language is a programming language that closely resembles machine instructions and allows for us to manage our own applications memory. This allows for more capabilities and effeciency in our programs.

### Why am I learning C?
I am currently learning C due to it's high use cases within reverse engineering. Since C has a close correlation to Assembly I can begin to learn various memory manipulation techniques and better understand how a program can directly interact with hardware.

Below you can follow the notes I have taken on C while following [**learn-c.org**](https://learn-c.org)'s tutorial series. I hope these notes can benefit you as well in your programming journey. Whether you are new to C just like me or an experienced programmer!

## The Basics of C
### File structure basics
Every C program uses libraries, which give the ability to execute necessary functions. One of the most common functions you'll see is `printf`. This function prints text to the screen and required the `stdio.h` header file to run.

To allow functions such as `printf` to run in our programs we must add the following code at the top of our file:

```
#include <stdio.h>
```

After importing our libraries, we have to define our `main` function. To my current knowledge this will always contain the first part of code to be ran in a program.

```
int main() {
  Your code will go here!
}
```

It's important that we include the `int` type infront of our main function. This indicates that the given function will return an integer or "int" as we see above. The number that is later returned from the main function will indicate whether or not our code was ran successfully. With this in mind we will return the number 0. Any number other than 0 indicates that our program failed.
```
return 0;
```

## Variables and Types
### Data Types
C has several types of variables, below are a few types of variables you may commonly see:
- Integers - Whole numbers which can be either positive or negative. `char`, `int`, `short`, `long`, `long long`.
- Unisgned Integers - Whole numbers which can only be positive. `unsigned char`, `unsigned int`, `unsigned short`, `unsigned long`, `unsigned long long`.
  - Useful when dealing with bit values, such as memory addresses or when doing manipulations such as bit masking or shifting on data.
- Floating point numbers - Numbers with fractions (Ex. 1.25, 1.03, 2.74) `float` and `double`.

C uses arrays of characters to represent strings.

C does **not** have a boolean type. Usually, it is defined using the following notation:
```
#define BOOL char
#define FALSE 0
#define TRUE 1
```

### Defining Variables
To define variables such as `nito` and `tech`, you would use the following syntax:
```
int nito;
int tech = 18;
```
As you can see, not every variables need to be assigned a variable immediately. We are simply initializing it so it is available when we need it.

Now that we have variables initialized we can assign and manipulate them like so:
```
#include <stdio.h>

int main() {
  int nito;
  int tech = 18;
  int result;
  
  nito = 12;
  result = nito + tech;

  printf("%d", result); /* Will print 30 */
  return 0;
}
```

## Arrays
Arrays are special variables which hold more than one value under the same variable name.
Defined using the syntax below, arrays are accessed by what's known as an **index**.
```
/* defines an array of 7 integers */
int numbers[7];
```
As stated above these arrays are accessed using an index. Indexing starts at 0 and goes to one less than the size of the array. Since our example above has an array of 7 integers we would access numbers with the indexes 0-6 To insert data would look like this:
```
int numbers[7];

numbers[0] = 0;
numbers[1] = 1;
numbers[2] = 2;
numbers[3] = 3;
numbers[4] = 4;
numbers[5] = 5;
numbers[6] = 6;
```
You can then access the data using indexing as well.

### Multidimensional Arrays
Another form of arrays are multi-dimensional arrays. The syntax would look like this:
```
type name[size1][size2]...[sizeN];
```
A **two-dimensional** array, which are initialized with their rows and columns. would look like this:
```
type arrayName[x][y];
```
This array would be initialized with **x** number of rows and **y** number of columns.
For example we could create an array to hold 2 rows with 4 columns each like so:
```
int a[2][4] = {
  {0, 1, 2, 3},
  {4, 5, 6, 7}
};
```
If you then needed to access or edit one of these values you would go to the corresponding row and column like `a[0][3]` which would access the first row last column. Remember that when you use indexs the final index is less than the total size of the array.

## Conditions
When you need a program to make a decision based on certain conditions you will use a conditional statement.
Here is an example of what these decisions can look like:
```
int number = 7;
if (number == 7) {
  printf("It's the number 7");
} else {
  printf("It's not the number 7");
}
```

### *if* statements
An if statement allows us to check if a condition is `true` or `false`.
As you can see in our example above, `number == 7`, you can use operators (Ex. ==, !=, etc) to check these conditions. 
After checking **if** the condition, if it's true the code within the brackets will run.
If not you need to provide an else statement like in our example above to give the program another block of code to run. 
We can also check multiple if statements using `if else` before going to our `else` code.
```
int number = 10;
if (number == 9) {
  printf("The number is 9");
} else if (number == 11) {
  printf("The number is 11");
} else {
  printf("The number is neither 9 or 11");
}
```
Finally we can also use the AND operator `&&` or the OR operator `||` to check multiple conditions in a single if statement.
```
int number = 10;
if (number != 9 && number != 11) {
  printf("The number is neither 9 or 11");
}
```
As you can see this can help with cleaning up our code readability and effeciency.

## Strings

### Defining Strings
Strings in C are just arrays of characters. We use a pointer, something I'll learn about later to define simple strings. These strings however can only be used for reading. They are defined like so:
```
char * name = "NitoTech";
```
If we wanted to define a string that we could manipulate we could use empty bracket notation `[]`. This notation tells the compiler to calculate the size of the array automatically. This is the same as if we specified it explicitly, adding one to the length of the string.
```
char name[] = "NitoTech";
/* is the same as */
char name[9];
```
The reason we have to add one to the total length of the string is for *string termination*.
String termination is a special character (equal to 0) which indicates the end of the string. The end of the string is marked because the program does not know the length of the string - only the **compiler** knows it according to the code.

### String formatting with printf
We can use the `printf` command to format a string together with other strings.
```
char * name = "NitoTech";
int age = 28;

/* prints out 'NitoTech is 28 years old.' */
printf("%s is %d years old.\n", name, age);
```
As you can see above we added a `%s` and `%d` into our `printf` string. This indicates that any variables specified after the comma will be added in their respective spots. `%s` stands for a string and `%d` stands for an integer.

### String Methods
We can use the function `strlen(var)` to return the length of a string.
```
char * name = "NitoTech";
printf("%d\n",strlen(name));
```

We can use the function `strncmp(string, string, maximum comparison length)` to compare two strings. Returning 0 if they are equal, or a different number if they are different.
```
char * name = "Jack";

if (strncmp(name, "Jack", 4) == 0) {
  printf("Hello, Jack!\n");
} else {
  printf("You are not John. Go away. \n");
}
```

We can use the function `strncat(destination string, source string, maximum number of character to append)`. To concatanate n characters of src string to the dest string.
```
char dest[20] = "Hello";
char src[20] = "World";
strncat(dest,src,3);
printf("%s\n",dest);
strncat(dest,src,20);
printf("%s\n",dest);
```
## Loops
### For Loops
For loop in C give us the ability to create a loop or a code block that runs multiple times. For loops require an iterator variable, usually notated as `i`.

For loops give the following functionality:
- Initialize the iterator variable using an initial value.
- Check if the iterator has reached its final value.
- Increase the iterator

```
int i;
for (i = 0; i < 10; i++) {
  printf("%d\n", i);
}
```
This loop will print the numbers 0 through 9. (10 print statements)
We can use this same for loop to loop through an array to get a total sum.
```
int array[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
int sum = 0;
int i;

for (i = 0; i < 10; i++) {
  sum += array[i];
}

printf("Sum of the array is %d\n", sum);
```

### While Loops
While loops are similar to for loops but will continue to execute while the condition remains true.
For example, this code will execute exactly ten times:
```
int n = 0;
while (n < 10) {
  n++;
}
```
It is important to be careful with while loops. If a condition is set that would execute infinitely it can cause your program to break or crash.

### Loop Directives
There are two important loop directives that are used in conjunction with all loop types in C - the `break` and `continue` directives.
The `break` directive halts a loop when the break line is ran.
The `continue` directive skips a line of code.

## Functions
C functions are simple, but because of how C works, the power of functions is a bit limited.
- Functions receive either a fixed or variable amount of arguments.
` Functions can only return on value, or return no value.
In C, arguments are copied by value to functions, which means that we cannot change the arguments to affect their value outside of the function. To do that we must use *pointers*.
Functions are defined using the following Syntax:
```
int foo(int bar) {
  /* do something */
  return var * 2;
}

int main() {
  foo(1);
}
```

In C, functions must be first defined before they are used in code. They can be either declared first and then implemented later on using a header file ro in the beginning of the C file, or they can be implmented in the order they are used (less preferable).
The correct way to use functions is as follows:
```
int foo(int bar);

int main() {
  /* calling foo from main */
  printf("The value of foo is %d", foo(1));
}

int foo(int bar) {
  return bar + 1;
}
```
We can also create functions that do not return a value by using the keyword `void`:
```
void foo() {
  /* do something */
}
```

## Static
`static` is a keyword in the C programming language. It can be used with variables and functions.

### Static vs Global?
While static variables have scope over the file containg them making them accessible only inside a given file, global variables can be accessed outside the file too.

### What is a static variable?
By default, variables are local to the scope in which they are defined. Variables can be declared as static to increase their scope up to file containing them. As a result, these variable can be accessed anywhere inside a file.

Consider these two examples:
```
#include<stdio.h>
int student() {
  int count = 0;
  count++;
  return count;
}

int main() {
  printf("%d \n", student());
  printf("%d \n", student());
  return 0;
}
```
In this example when we print our student function the result will be "1" each time.
This is because the count variable is reset with every function ran. To fix this we can use the `static` keyword.
```
#include<stdio.h>
int student() {
  static int count = 0;
  count++;
  return count;
}

int main() {
  printf("%d \n", student());
  printf("%d \n", student());
  return 0;
}
```
With this, now when we get our print statements back we will receive 1 and 2.

### What is a static function?
By default, functions are *global* in C. If we declare a funciton with `static`, the scope of that function is reduced to the file containing it.

The syntax looks like this:
```
static void fun(void) {
  printf("I am a static function.");
}
```

## Conclusion
This concludes the basics of C taught through [**learn-c.org**](https://learn-c.org)'s tutorial series! I highly suggest you go and check out their website as they provide live exercises for you to try. I hope these notes can be used as a nice refresher or taught you something along the way. Goodluck on all your programming endeavors!