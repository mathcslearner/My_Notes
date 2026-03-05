# C Preprocessor Directives

When a C program is compiled, the compiler first runs a preprocessor which looks at the preprocessor directives written in the files and executes them.

They begin with # and provide useful functionalities such as header file inclusion, macro expansion, conditional compilation, include guards, etc.

The \#include \[filename] statement searches for filename and then replaces the include statement with the contents of the file, allowing us to use the functions in that file. This allows us to use modules.

Macros in C are defined using \#define identifier value. The idea is pretty simple: the preprocessor will replace every occurrence of identifier in the code with the value. For example if we do 

```C
#define MAX 10

int array[MAX]
```

then it will become

```C
int array[10]
```

This is an example of where macros differ from constant variables. Using macros results in a constant length array, while using a constant variable leads to a variable length array.

Small note: Macros can also affect each other, if you name them in a way that they depend on each other.

Next is conditional compilation. The idea is that we might want to compile certain parts of our code under specific conditions. Here's a simple example of how it works:

```C
#ifdef FEATURE1
	// code for feature 1
#endif
#ifdef FEATURE2
	// code for feature 2
#endif
#ifdef FEATURE3
	// code for feature 3
#endif
```

We see that the code for feature1, feature2, feature3 only run if the respective macros are defined. Suppose we want feature 1 and feature 2 only, then we would have to add \#define FEATURE1 and \#define FEATURE2 IN THE CODE.

This is kind of annoying, so an alternative is to define the macros from the command line. For example, to compile with feature 1 and feature 2, we can do 

```bash
clang -DFEATURE1 -DFEATURE2 *.c
```

Another use of conditional compilation is to comment out segments of code. One way to comment out code is to use multi-line comments, however the problem is they cannot be nested. On the other hand, conditional compilation nests just fine. One use case of this is that we might have a lot of debug/print statements through our code: we don't want to delete them in case they become useful later, but we don't want them to run when we run the program normally. We can conditionally compile print statements like this:

```C
#ifdef DEBUG
	stuff
#endif
```

Then if we want the print statements, we can run using -DDEBUG.

When we write larger programs, it is good practice to break programs into smaller modules. There is a problem which might arise: it is possible that when we include the desired modules, we end up including a specific interface multiple times. The functions in there are defined multiple times, which is an error for the C compiler.

It would be kind of hard to keep track of exactly which file includes which headers, so adding include guards to every header file (not the implementation file) in the program is a good solution. Here's a simple example:

```C
#ifndef UNIQUE_MACRO_NAME

#define UNIQUE_MACRO_NAME

// original header file

#endif
```

This include guard ensures that every header is only included once (because the second time we come around to it, the macro name will have been defined, so the code block doesn't compile).
