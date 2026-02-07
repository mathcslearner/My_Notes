# C Pointers

As mentioned previously, C was designed to give programmers low-level control of memory.

To obtain the address of a variable, we can use the address operator &. We can store memory addresses in pointers. Let's say we have an int i. Then to create a pointer to i, we would do

```C
int i = 9;
int *p = &i;
```

Note the syntax (content type) \*(var name) for pointer types.

The value of the pointer itself (the address) is not very important: we care mainly about the content it is pointing at. To get the content of the memory box the pointer is pointing at, we use the dereference operator \*. For example, \*p.

When we initialize a pointer and we don't know what to point it at, we can do a null pointer: int \*p = NULL. This is better than not initializing it, so int \*p;

Before dereferencing a pointer, it's very important to check it's not a null pointer by doing if(p), since NULL is false.

For structures specifically, there is a special syntax for dereferencing a struct field, by using ->. Here's an example:

```C
struct posn {
  int x;
  int y;
};

int main(void) {
  struct posn my_posn = {0, 0};
  struct posn *ptr = &my_posn;
  
  (*ptr).x = 3;                   // awkward
  ptr->y = 4;                     // much better
  
  trace_int(ptr->x);
  trace_int(ptr->y);
}
```

In a C function, when we pass it a parameter, let's say x, we are passing a copy of x which is local to the function. This means that modifying x within the function does not modify it outside. This is called pass-by-value. For example:

```C
// inc(i) adds 1 to i ?!?
void inc(int i) {
  ++i;
}

int main(void) {
  int x = 5;
  inc(x);
  trace_int(x);
}
```

In this example, the value of x in the main function is still 5.

If we wanted to use the inc() function to change the value of x in the main function, we would have to do pass-by-reference. To do so in C, we pass the pointer to the parameter. Here's the previous example but using pass-by-reference:

```C
// inc(p) increments the value of *p
// effects: modifies *p
// requires: p is a valid pointer
void inc(int *p) {
  assert(p);
  *p += 1;
}

int main(void) {
  int x = 5;
  trace_int(x);
  inc(&x);            // note the &
  trace_int(x);
}
```

Pointers therefore allow us to mutate variables by passing in a pointer parameter to another function.

When we call a function, a copy of each argument value is placed onto the stack frame. For structures, the entire structure is copied, which can be inefficient. This is why we prefer to pass a pointer to a structure instead. This also allows us to mutate the contents of the struct, as shown before.

Let's say we don't want to mutate the content that the pointer is pointing at, when we pass in the pointer in a function. We can enforce it using the const keyword. It can get confusing so here's a recap of all the different possibilities:

- int \*p: p can point at any integer, and we can modify the int using \*p
- const int \*p: p can point at any integer, but we cannot modify the int using \*p
- int \* const p: p always points at the same integer, but that int can be modified using \*p
- const int \* const p: p always points at the same integer, and that int cannot be modified using \*p

The rule is "const applies to the type at the left of it, unless it appears in the first position, then it applies to the right of it".

Recall that in functional programming, functions are first class values, so they can be passed as arguments and returned by functions. A common example to illustrate this concept is to show how we can create a map function for a list of numbers.

We can pass in functions using function pointers. The syntax to define a function pointer with name fp is

```C
return_type (*fp) (param1_type ...)
```

Here's an example of a function pointer in use:

```C
// io_apply(f) reads in each int [n] from input
//   and prints out f(n)
// requires: f is a valid pointer
// effects: produces output
//          reads input
void io_apply(int (*f)(int)) {
  assert(f);
  int n = 0;
  while (scanf("%d", &n) == 1) {
    printf("%d\n", f(n));
  }
}

// sqr(i) calculates i^2
int sqr(int i) {
  return i * i;
}

int main(void) {
  io_apply(sqr);
}
```

