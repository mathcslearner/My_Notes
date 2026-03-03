# C Modularization

First, it might be good to understand what modularization actually is (this is a principle universal for software engineering, not just C). In fact, in C, "modules" are really libraries.

The main idea is that we cannot just keep the code for a program in the same file. For larger programs, it will completely break down and be impossible to work with. The idea is to use modularization to divide up the program into well defined modules.

Simply put, a module provides a collection of data structures/variables/functions which serve a common aspect or purpose. Most commonly, each module will be in their own file/folder.

A module provides functions, which can then be used by other programs, known as client. We can then have the main program require a module, which requires functions from other modules, etc.

In C, we compile a source file(.c) into machine code (.o) before it can be run. If we use modules, then we have to link multiple machine code files together before running the program.

Here are the three main advantages of modularization:

- Re-usability: A module can be re-used by many clients for the same purpose
- Maintainability: It is easier to test the functionality of a single module rather than a large program
- Abstraction: To use a module, the client does not need to understand the module's implementation, which means we can use code without understanding how it works

To use a variable from a module in the entire program, we need to extend its scope to program scope. We can do so using the extern keyword. In fact, a declaration like extern int g; can even be used to make the variable g, which is now a global identifier, accessible in every file of the program.

We can also restrict the variables/functions in our module to only be in scope for the module itself, if they are not meant to be provided for clients. We can do so using the static keyword.

Previously we mentioned that modules are useful partially because they can be abstracted. We can do so using an interface, which provides a list of functions and the corresponding documentation. This is everything the client needs to use the module (it doesn't need the implementation).

An interface should include a description, a function declaration for each function, structure/variable declarations, and documentation/purpose for each provided function. It should also contain usage examples.

In C, we use .h files to create interface (header files). To be able to use the provided functions, we can use the directive \#include, which basically inserts the function declarations in the file.

There are standard modules/libraries in C, such as assert.h, limits.h, stdbool.h, stdlib.h, stdio.h. To include standard modules we use <> and for custom modules we use "".

When designing module interfaces we want:

- High cohesion: all functions work towards a common goal
- Low coupling: Little interaction between modules

Another thing we can do with module interfaces is information hiding by hiding implementation details:

- Safety: The client cannot tamper with data used by the module
- Flexibility to change implementation in the future

We can use opaque structures to hide information, using incomplete declarations. As an example, struct opaque; rather than struct opaque {int x;}. Then, the client can only access the data using pointers. If we do this, we also need to provide functions to create and destroy structures (using dynamic memory allocation).
