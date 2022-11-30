  Hello everyone, in this resume i'm going to explain some basic things
about stack based buffer overflows. First, we need to to understand how 
it works in theory.

  When a computer program is executed, it is loaded into the system's
memory. Inside the computer's memory we have three sections. 

>  code: The code section is where the program instructions are loaded.
  <br>
>  stack:  The stack section is where the program's local variables are loaded.
  <br>
>  heap:  The heap is also a region of memory used for dynamic allocation.

  For now, we are going to learn about the stack region using as 
example a simple program written in C programming language.

{% highlight C %}

#include <stdio.h>

int main(void) {

   char buffer[100];
     
return 0;
 }
{% endhighlight %}

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

   So, what we need to do to cause a (Stack) Buffer overflow? 
   and, what can i do with it? 

   Answer1: We simply need to fill the buffer with a value
bigger than it supports.
   Answer2: This question will be answered soon. 


   So, if we fill our buffer with A's it will overwrite
watever comes below it, because the stack grows towards
lower memory addresses. 

```
   Top of stack                            bottom of stack 
   ------------------------------------------------------
   | buffer | frame pointer | return address | function1 |
   ------------------------------------------------------
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```

   Ok, now we can overwrite the frame pointer, and return address,
what can we do with it?

   Answer: To understand it, we first need to understand the 
return address again. When the program calls let's say, function
a calls function b, the function a stores the address of its
next instruction in the stack, so that when the function b
returns, the function a retrieves this value from the stack, 
and continue to execute its instructions. BLABLABLA........

  So what the program does with the return address in hand? 
  It simply jumps to the location pointed by it. 

  Now, we are going to answer the question, what can we
do with it? 

  Answer: We can overwrite the return address making it point 
to our buffer, that way, instead of executing the remaining instructions
from function a, the program will jump to address of our buffer, 
executing watever exists there, in our case, a bunch of A's. 

  It will cause our program to crash, because a bunch of A's
are not instructions to be executed. So by now i think you 
are thinking, instead of a bunch of A's we can insert any code
to be executed?? Exactly...

  BOOM, Now we are able to execute whatever we want by means of 
changing to value of the return address and inserting the code
in the buffer. This code usually used in this scenario is called
shellcode, it's simply a code that gives a terminal session. 

```
   Top of stack                            bottom of stack 
   -------------------------------------------------------
   | buffer | frame pointer | return address | function1 |
   -------------------------------------------------------
    shellcode  baddress  baddress  baddress 
```

# References

[https://en.wikipedia.org/wiki/Buffer_overflow](https://en.wikipedia.org/wiki/Buffer_overflow)
<br>
[https://inst.eecs.berkeley.edu/~cs161/fa08/papers/stack_smashi](https://inst.eecs.berkeley.edu/~cs161/fa08/papers/stack_smashing.pdf)


