## Overview

We can use a mountain analogy to understand. 

At the beginning, a program is just some raw text, a string of characters. We can view this as the bottom of the mountain. We can analyze the program and transform it into a representation where we understand what the author is instructing the computer to do through the program. At this moment, we are at the top of the mountain.

Then, we can go down to the other side of the mountain. Transform the high-level representation of the program's meaning into instructions that the computer itself can execute.

Here is a nice picture to visualize the mountain analogy and see at a glance the different phases' relation with each other.

![[Pasted image 20260801180235.png]]

Let's walk through each of the phases by using an example. Suppose the user types in their program's source code, the following line:

```
var average = (min + max) / 2;
```

From the point of view of the computer, it's effectively a bunch of characters:

```
v/a/r/ /a/v/e/r/a/g/e/=/(/m/i/n/+/m/a/x/)/2/;
```

The first step is **scanning**, also known as lexical analysis. A scanner takes in the list of characters and splits them into chunks called **tokens**. Some tokens might be characters, while others might be numbers, words, etc. A token is a meaningful group of characters. It might give something like this:

```
var | average | = | ( | min | + | max | ) | / | 2 | ;
```

Notice how it's already so much more readable.

The second step is **parsing**. Every programming language has a grammar that dictates the syntactic structure of sentences in that language. In particular, grammars are often recursive and involve expressions nested within each other. A parser takes the sequence of tokens and builds out a tree structure mirroring the nested nature of the grammar (abstract syntax tree, AST). For our example, it might look something like

![[Pasted image 20260801181203.png]]

The parser also reports syntax errors back to us.

The previous two steps usually occur across all implementations. For the ones coming next, it really depends on what language it is.

In **Static analysis** , the language will first do binding: for each identifier, find where it was defined, and connect the two together. This is where scope matters. If the language is statically typed, we also do type checking to make sure that operations are legal to do.

The results from static analysis can be stored on the syntax tree, in a symbol table, or in other data structures.

The previous three steps are commonly known as the frontend of the implementation (this ties in with the "first half" of the mountain climbing). The idea is that the frontend is specifically tied in with the source language of the program

There are two steps we can do in the "middle end" of the implementation. In the middle, code can be stored in intermediate representation (IR) that allows us to support many source languages and target platforms. This means we can just write frontend and backend implementations for specific languages/platforms, and use the IR to connect them together.

We can also optimize the code the user wrote by keeping the meaning but implementing it in a more efficient way. This is known as compile-time optimization. For example, constant folding: if some expression is always the same value, replace it with its result directly rather than calculating it at runtime.

Now we're getting to the backend side of language implementation.

The first phase is **code generation**. The idea is to convert the AST or the IR we got from the frontend/middle end into machine code the computer can execute by itself, usually Assembly instructions the CPU runs.

Generating real machine code for the OS to load onto the chip will make it run very fast, but generating it is a lot of work. It is also very not portable across different computer architectures. That's why some languages make their compilers generate virtual machine code (bytecode), which are closer to the language's semantics than the computer architecture.

If you're producing bytecode, you have to translate it for the chip. The first option is to write a compiler for the bytecode itself. (essentially using the bytecode as an IR). The second option is to write a virtual machine (VM) that emulates a chip supporting bytecode, and you run the bytecode on that VM. This is very portable, at the expense of it being slow. For example, if your VM is in C then you can run your language on any platform with a C compiler.

The last phase is **runtime**. We just run the machine code/bytecode we have and run it in either the OS or the VM. While the program is running, we might provide services, like garbage collectors. The code related to that is inserted into the executable.

There are some shortcuts you can take if you want.

The first one is **single-pass compilers**. The idea is to not allocate any syntax trees or IRs, directly going through the pipeline once. As soon as you parse a bit, you need to know how to compile it (since you're not going back after). This creates limitations, for ex. in C you can't call a function above its definition.

The second one is **tree-walk interpreters**. The idea is to begin executing code right after parsing it to an AST by traversing the syntax tree and evaluating it. This is not usually used in major PL because it's slow.

The third one is **transpilers**. First, write a frontend for your language. In the backend, produce a string of valid source code in another language. Then compile that code using that language's compiler. (Essentially converting the source code from one language to another). For ex., you can transpile your code to C since C runs pretty much everywhere. To run your stuff in a browser, you need to transpile it to JS, etc.

The last one is **just-in-time compilation**. This is what the JVM does. When the program is loaded on the user's machine, compile it to native code for their architecture (during runtime).

Maybe now's a good time to talk about the difference between compilers and interpreters.

- Compiling is a technique to translate a source language to another form (bytecode, machine code, another language, etc.)
- A language implementation being a compiler means that it translates source code, but doesn't execute it. The user has to run it themselves
- When it is an interpreter, it takes the source code and executes it
