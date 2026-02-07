# C Memory Model

One of the most important features of C, compared to languages such as Python, is the responsibility to manage memory. As such it is important to understand how memory works.

One bit can be stored as 0 or 1. The smallest unit of memory is a byte, which is 8 bits. Each byte has an address which indicates its position. We can imagine computer memory as a collection of mailboxes, which have an address and a byte (the content).

When we define a variable, C finds space in memory to store it, keeps track of the address and stores the value at that location. When we use the variable, C fetches the content from the address.

To find the number of bytes required to store a variable, we can use sizeof(). For example, an int is 4 bytes. This means C finds 4 consecutive bytes of memory to store the value, and keeps track of the address of the first byte.

Since C uses 4 bytes to store a bit, you can only store $2^{32}$ distinct values. Trying to represent any value outside of those will lead to overflow, which gives undefined behavior.

The amount of memory reserved for a struct is at least the sum of the memory for the individual fields. We must use sizeof() to determine the exact space.

When we run code, there are 5 sections of memory reserved for the program: code, read-only data, global data, heap, stack.

The code section is where the machine code obtained from the source code after compiling is stored.

The read-only data section reserves space for global constants, while the global data section reserves space for global mutable variables. This space is reserved before the execution.

The stack section is how we model function calls. Recall that a stack is a LIFO data structure where we can push and pop items.

When we call a function, it is pushed onto a stack, so it is at the top. When a function returns, it is popped off the stack and the control flow returns to the new top function.

As a simple example, consider this code:

```C
void blue(void) {
  return;
}

void green(void) {
// Snapshot at this moment
  return;
}

void red(void) {
  green();
  blue();
  return;
}

int main(void) {
  red();
}
```

At the instant marked in the code, the call stack looks like (main, red, green) where green is at the top. When we return from green and enter the blue function, call stack would look like (main, red, blue).

Now, after a function call is popped, we need to know which line to return to in the next "top function". That's why C stores the return address.

The function calls we push onto the call stack are known as stack frames. Each one contains:

- argument values
- local variables
- return address

Here's an example of how to model stack frames:

```C
int pow4(int j) {
    printf("inside pow4\n");
    int k = j * j;
    // Snapshot
    int l = k;
    return k * l;
}

int main(void) {
    printf("inside main\n");
    int i = 1;
    printf("%d\n", pow4(i + i));
}
```

The stack frames look like:

=========================  
pow4:  
  j: 2
  k: 4  
  l: ???  
  return address: main:12
=========================  
main:
  i: 1
  return address: OS
=========================

When we have a recursive function, we see that each recursive call creates a new stack frame. For example:

```C
int sum_first(int n) { 
  if(n == 0) {   // Snapshot here
    return 0; 
  } else {
    return n + sum_first(n - 1);
  }
}

int main(void) {
  int a = sum_first(2);
  //...
}
```

has stack frame

==============================  
sum_first:
  n: 0 
  return address: sum_first:5  
==============================  
sum_first: 
  n: 1
  return address: sum_first:5 
==============================  
sum_first:
  n: 2  
  return address: main:10
==============================  
main: 
  a: ???  
  return address: OS 
==============================

When we have an infinite recursion, new stack frames keep getting created, so the call stack keeps growing. When it's too large, it collides with the other memory sections and causes a crash. This is known as stack overflow.
