  Hello everyone, in this resume i'm going to explain some basic things
about stack based buffer overflows. First, we need to to understand how 
it works in theory.

  When a computer program is executed, it is loaded into the system's
memory. Inside the computer's memory we have three sections. 

``` 
  1 - code
  2 - stack
  3 - heap
```

  The code section is where the program instructions are loaded. 
  The stack section is where the program's local variables are loaded.
  The heap is also a region of memory used for dynamic allocation. 

  For now, we are going to learn about the stack region using as 
example a simple program written in C programming language.

```
#include <stdio.h>

int main(void) {

     char buffer[100];
     
return 0;
}

```

  When we talk about local variables we are talking about variables
declared inside a function scope. In this example, we can see that 
we declared a variable of type char that holds a hundred spaces in 
computer memory. Each space is of 1 byte of size. In C language
we usually call this structure a buffer.

  Ok, let's move on to the point. Because this variable is declared
inside a function in C language called main, we can call it a local 
variable, which means that it will be stored in the memory location
called stack. 

