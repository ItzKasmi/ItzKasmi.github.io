## Lets learn about C!

### Introduction
Today, I learned the basics of C. Luckily, it wasn't too difficult to pick up due to my experience with other languages! Even if you've never worked in C before I hope this guide will prove as a nice introduction.

#### What is C?
C, is a general purpose programming langauge. What makes C unique is that it is considered a low level language. A low level language is a programming language that closely resembles machine instructions and allows for us to manage our own applications memory. This allows for more capabilities and effeciency in our programs.

#### Why am I learning C?
I am currently learning C due to it's high use cases within reverse engineering. Since C has a close correlation to Assembly I can begin to learn various memory manipulation techniques and better understand how a program can directly interact with hardware.

Below you can follow the notes I have taken on C while following **learn-c.org**'s tutorial series. I hope these notes can benefit you as well in your programming journey. Whether you are new to C just like me or an experienced programmer!

### The Basics of C
-----
#### File structure basics
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

### Variables and Types
-----
#### Data Types
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

#### Defining Variables
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