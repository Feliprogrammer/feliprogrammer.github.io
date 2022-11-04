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

  Now, let's talk about the stack. we can compare the stack region
as a pile of plates that are cleaned after a meal lol. The last plate
is put above all other plates and if we had another plate, it would
be put above again.

```
   Top of stack                 bottom of stack 
   ---------------------------------------------
   | last_plate | one plate bellow | and so on |
   ---------------------------------------------

```

  Before, the main function gets called, we need to understand 
that the stack is already filled with a bunch of local variables
from other functions of the program. So it will be represented
this way: 

```
   Top of stack                 bottom of stack 
   ---------------------------------------------
   | function2 variables | function1 variables |
   ---------------------------------------------

```

   Also, we need to understand that the last function's local 
variables will be placed in the top of our stack. 

   Usually before the program jumps from a function to another 
function, it stores a value on stack called return address. This 
address is stored so that the program's function before the 
last function knows where it stopped and then continue from there.

   So, the stack looks like this... 

```
   Top of stack                                  bottom of stack 
   --------------------------------------------------------------
   | function2 variables | return address | function1 variables |
   --------------------------------------------------------------

```

   Let's recap, before a function is called in c programming language,
the return address is stored in stack, then when we enter inside the
function scope, the program saves a value into the stack called frame
pointer. So the stack looks like this. 

```
   Top of stack                                  bottom of stack 
   --------------------------------------------------------------
   | function2 variables | frame pointer | return address | f1  |
   --------------------------------------------------------------

```
   So, what's the point of learning about it?? Most of it is done
in low level context. What about our buffer?? Why talk about it ?? 

   Now, things starts to get interesting, our buffer variable
is actually our (function2 variables, image above).

   Every variable has its place in memory determined by a fixed
size. This size, in the context of the C programming language is 
handled by the programmer. So, the programmer needs to especify 
the size of it in memory. 

   In C programmming language, when we declare a variable, 
the operating system goes to the system's memory and reserves
a space of the size of that variable. But it does not check 
wheter the variable is actually going to use that fixed space.

   OK, so what is the problem?? The problem is that a programmer
can try to store a value bigger than the fixed size defined, and
because it does not check the value being stored it can explode
the memory lol, i mean, it can store a value that is not supported
by the memory space, causing what we call a overflow.

   In our case scenario, we declared a local variable called buffer
with a hundred positions, the operating system then reservers a 
hundred bytes of memory space for that local variable.

```
   Top of stack                            bottom of stack 
   ------------------------------------------------------
   | buffer | frame pointer | return address | function1 |
   ------------------------------------------------------

```

   
