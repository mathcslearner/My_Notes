# C Arrays and Strings

## Arrays

An array is a data container which stores multiple elements of the same type. Once created, the length is fixed. Here is an example of how to initialize:

```C
int a[6] = {1, 1, 2, 5, 14, 42}
char word[5] = {'m', 'a', 't', 'h', '\0'}
```

The bracket represents the length of the array. To access a single element, we can use indexing, eg a\[0] for first, a\[1] for second, etc. To modify them, just mutate them individually. To iterate over loops we can loop over the index.

Note: In C, the array does not store its length like in Python, JS, etc. We must keep track it of separately.

When we pass an array to a function using its name, we are by default passing its address, so we don't need to use pointers. That's also why we need to assert every array is non null.

When initializing an array, uninitialized elements are initialized to zero. Example:

```C
int b[5] = {3, 7, 51} // equivalent to b[5] = {3, 7, 51, 0, 0}
```

Note we can only mutate individual elements, and we cannot assign a new value to the array.

In the C memory model, the array elements sit contiguously. This means it is possible to loop over an array using pointer arithmetic. In fact, a\[i] is equivalent to \*(&a\[0] + i). Be careful to only do pointer arithmetic within the array.

For an array, a, &a and &a\[0] all have the same value, but different types. Also, \*a = a\[0]. 

Items in an array will always be spaced by the sizeof a single item (which makes sense). When we have an array of int, doing +1 on a pointer is equivalent to doing + 4 bytes as sizeof(int) = 4. It turns out subtracting pointers is also valid.

If we want to do multi-dimensional data, we could do this in 2 ways (the second way is how it's done internally):

```C
int data[2][3] = {{1, 2, 3}, {4, 5, 6}}
int data[6] = {1, 2, 3, 4, 5, 6}
```

## Strings

C doesn't have a built-in string type. Instead, a string is an array of characters ended by a null terminator "\0". Here are equivalent ways to define a string:

```C
char a[4] = {'c', 'a', 't', '\0'}
char b[4] = {'c', 'a', 't'}
char c[4] = "cat"
char d[4] = "cat\0"
char e[] = "cat"
```

Looking at the last one, we see that we don't need to pass the length of the array. 

The length of a string is the number of characters which comes before '\0'. Note this is not the same thing as the length of the array!

In C, we can use strlen to find the string length.

When looping over strings, loop over characters until you hit the null terminator (which evaluates to false).

We can use printf("%s", bla) to print strings.

To read in strings, use scanf("%ns", ...). The n represents the maximum field width. The length of the string we read in must be smaller than n (to accomodate for the null terminator). Not doing so will lead to buffer overflow and safety issues.

To compare strings in a lexicographical order, we can use strcmp. It returns negative if s1 before s2, positive if s1 comes after s2, 0 if they are equal.

Here are two useful functions for string processing:

- strcpy(char \*dest, const char \*src) overwrites the contents of dest with the contents of src

- strcat(char \*dest, const char \*src) concatenates src to the end of dest

Whenever we use these, we have to ensure that the dest array is large enough, including space for the null terminator. Also, don't call it on dest, src which overlap, since it will overwrite the null terminator.

We can call strings in expressions directly using double quotes, like this:

```C
printf("%d + %d is %d\n", a, b, a + b);
strcpy(dst, "09 F9 11 02 9D 74 E3 5B D8 41 56 C5 63 56 88 C0");
int i = strlen("The only reason for making honey is so as I can eat it.
scanf("%d", &i);
char *ptr = "this is a literal; the stack stores a pointer to it.";
```

These are called string literals, and for each of them, a const char array is created in read-only data. We cannot mutate string literals.
