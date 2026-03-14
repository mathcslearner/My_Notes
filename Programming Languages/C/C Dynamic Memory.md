# C Dynamic Memory

Recall that in C, arrays can only be constant length. This means we cannot create an array of length based on a variable which we receive. That is, our arrays cannot have dynamic length.

Dynamic memory solves this problem (and a bunch of others!). 

The heap is a section of memory which is available to our program. Upon request, the heap allocates memory to the program for any purpose we want. After we finish using it, we need to free it (i.e. return the memory to the heap), otherwise we run into memory leaks.

Here are some advantages:

- The size of memory we need can be dynamic (from a variable)
- A heap allocation can be resized
- Memory allocated by the heap in a helper function persists even after the function returns

When we call malloc (memory allocation), the initial values in the borrowed memory are uninitialized and are random.

Here's an example of how to borrow memory dynamically from the heap using the malloc function. We can see how after using the memory, we need to call the free function on it at some point:

```C
int *build_array(int len) {
  assert(len > 0);
  int *a = malloc(len * sizeof(int));
  for (int i = 0; i < len; ++i) {
    a[i] = i;
  }
  return a;
}

int main(void) {
  int *my_array = build_array(10);
  array_print(my_array, 10);
  free(my_array);
}
```

For good practices, use sizeof in the malloc call. From the build_array function we see that we can essentially treat the dynamic memory as an array, and we can use operations such as indexing \[] on it to change the values stored in the borrowed memory.

Here is a clean visualization of how the heap works:

![[Pasted image 20260314181247.png]]

If the heap is full, then the call to malloc will return a null pointer. Therefore it is best to check whether the memory we borrow is valid or not.

After we free a block of memory using the free() function, the pointer to that memory is no longer valid. That means we can no longer access the block of memory through the pointer. As such the pointer can be considered as "dangling". Good practice would be to assign the pointer to NULL.

Also note that we cannot call free() on memory which was not returnedby malloc, or memory which has already been freed.

Another thing to be careful about is to not overwrite pointers we obtain from malloc. If we do so, then we lose the access to that memory forever and it can never be freed, which is a memory leak.

Let's consider a scenario where we want to store integers in the heap, but we do not know exactly how many there are (maybe because we are reading from input, etc.). We might first allocate space for 100 integers in the heap, and then we run out of memory after the first 100 integers. 

The solution would be to resize the array by creating a new larger array, copying the items from the old to the new array, and then freeing the old array. Here's an example:

```C
// my_array has a length of 100
  int *my_array = malloc(100 * sizeof(int));
  for (int i = 0; i < 100; ++i) {
    my_array[i] = i;
  }
  
  // oops, my_array now needs to have a length of 101
  int *old = my_array;
  my_array = malloc(101 * sizeof(int));
  for (int i = 0; i < 100; ++i) {
    my_array[i] = old[i];
  }
  free(old);
```

Note how it was important to first save the pointer to the old array in a temporary variable before assigning my_array to a new malloc pointer, otherwise we would lose the access to the memory forever.

This feature is pretty convenient so there is a built-in function for it, realloc(). Here is the same example with realloc:

```C
// my_array has a length of 100
  int *my_array = malloc(100 * sizeof(int));
  for (int i = 0; i < 100; ++i) {
    my_array[i] = i;
  }
  
  // oops, my_array now needs to have a length of 101
  my_array = realloc(my_array, 101 * sizeof(int));
```

A useful application of realloc is to read in a string from input. A popular strategy is to first start with an array of certain length in the heap. When the array becomes full, we call realloc to resize the array to double the length of the array. When doing this, we also keep track of the actual length of the string. At the end, we need to shrink the array down to the actual length (+ null terminator) of the string to avoid wasting memory, using realloc.

Through amortized analysis, the total runtime for reading strings becomes O(n).

Now that we know how dynamic memory works, we can use it to create opaque structures in modules.

The idea is that the client cannot create the structure by itself. Instead, it must call the provided create function, which will return a pointer. This effectively means the client can only interact with the structure using the provided functions.

The create function simply calls malloc to put the structure in the heap memory. The destroy function frees the memory.
