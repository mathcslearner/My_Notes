# Representing Code - ASTs

Previously, in the scanner, we turned raw source code as a string into a series of tokens. In the next part, the tokens will transform them into a better representation of the semantic meaning of the code. To do that, we first need to define that representation for code.

Consider an arithmetic expression, with operations and numbers. We could visualize the order of operations to evaluate it using a tree. Leaf nodes would be numbers, and interior nodes would be operators with their operands being the children.

Formatting an arithmetic expression this way, to evaluate it, you need to do a post-order traversal until you reduce to a number.

It seems like trees are a very useful way of representing code, so that will be our ultimate goal. First, we have to learn about context-free grammars and syntactic grammar. Each letter of the alphabet is a token, and a string is a sequence of tokens (an expression). The grammar's role is to specify what strings are valid, and which aren't.

We create a finite set of rules for the grammar, that will allow us to generate an infinite number of valid strings. Each rule has a head (its name) and a body (its description). A body is a list of symbols.

Each symbol can be either a terminal (a letter from the alphabet) or a nonterminal (the name of another rule). This allows infinite composition in the grammar. Also, you can have multiple bodies for the same head.

The standard way to specify syntactic grammars is called BNF (Backus-Naur form).

It is of the form head -> body, where symbols with quotes are terminal, without quotes are nonterminal (other rules). We c.n separate the various bodies for the same head using |. We can use parentheses and | to represent "pick one of this group"/. We can use \* to represent possible repetition, + to represent repetition at least once, ? for optional production (these are basically the same as in regex).

Here is an example of how we might use BNF to express a grammar for simple expressions (literals, unary expressions, binary expressions, parentheses):

```
expression     → literal
               | unary
               | binary
               | grouping ;

literal        → NUMBER | STRING | "true" | "false" | "nil" ;
grouping       → "(" expression ")" ;
unary          → ( "-" | "!" ) expression ;
binary         → expression operator expression ;
operator       → "==" | "!=" | "<" | "<=" | ">" | ">="
               | "+"  | "-"  | "*" | "/" ;
```

Note here NUMBER and STRING represent any number/string literal.

Using this grammar, since it is recursive, expressions can be represented in a tree, more accurately an AST (abstract syntax tree). 

Back in the scanner, we used one Token class to represent all kinds of lexemes. But for expressions, note that there will be a difference between unary, binary, literals. We could make an Expression class where nodes can have arbitrary amount of children. But we can also harness Java's type system by building a base class for expressions, then for each one (literal/unary/binary/grouping) create a subclass with the correct fields.

Something like this:

```Java
package com.craftinginterpreters.lox;

abstract class Expr { 
  static class Binary extends Expr {
    Binary(Expr left, Token operator, Expr right) {
      this.left = left;
      this.operator = operator;
      this.right = right;
    }

    final Expr left;
    final Token operator;
    final Expr right;
  }

  // Other expressions...
}
```

Note these Expr class/subclasses have no methods. This is fine because they exist as way of communication, so it's normal they have no explicit behavior (separation of concerns).

Note that since each subclass will basically just be a name and a list of typed fields, it's kind of tedious to right, so we can put together a script that generates the boilerplate for all the expressions we have. The script basically takes in a description of each type and its fields, and based on that, prints the boilerplate code for its subclass.

Because of the way we formatted our Expr class, when we later on get to the interpreter, for each chunk of code (tree) that it receives, it needs to find what type it is, and execute it based on what it is. Doing if statements like this:

```Java
if (expr instanceof Expr.Binary) {
  // ...
} else if (expr instanceof Expr.Grouping) {
  // ...
} else // ...
```

is pretty bad, so we need to figure out something else.

This is actually a fundamental problem known as the expression problem. We have a bunch of types, a bunch of operations, and each pair of type/operation needs a specific implementation. See the table:

![[Pasted image 20260807204610.png]]

For OOP languages like Java, they assume all the code in one row stays together in a class, and you define the methods inside the class. This makes it easy to add new rows, and you don't need to touch the other classes. But if you want to add a new column, you need to add a method to every existing class, which is tedious.

In functional languages in the ML family, classes don't exist. To implement an operation for types, you define a single function that matches based on the type to implement the operation. This makes it easy to add new operations, but adding new types is hard.

Notice how it's not easy to add both rows and columns. This is the expression problem. We will attempt to solve it using the visitor pattern. The main idea of this pattern is to approximate functional style in OOP language, for adding new columns easily. You define the behavior of a new operation in a single place, using a layer of indirection.

Here's a simple example:

Imagine you have these classes

```Java
 abstract class Pastry {
  }

  class Beignet extends Pastry {
  }

  class Cruller extends Pastry {
  }
```

We want to define new operations without adding new methods to each class every time. First, define a visitor interface:

```Java
 interface PastryVisitor {
    void visitBeignet(Beignet beignet); 
    void visitCruller(Cruller cruller);
  }
```

An operation is then a class implementing this interface. This keeps the code for the operation in a single place.

Given a pastry, we want to route it to the correct method on the visitor based on its type. Use polymorphism:

```Java
  abstract class Pastry {
    abstract void accept(PastryVisitor visitor);
    
    class Cruller extends Pastry {
	    @Override
	    void accept(PastryVisitor visitor) {
	      visitor.visitCruller(this);
	    }
	}
	
	class Beignet extends Pastry {
		@Override
		void accept(PastryVisitor visitor) {
			visitor.visitBeignet(this);
		}
	}
  }
```

Using this one accept call one time in each class, you can use as many visitors as we want.

This means we need to add this visitor pattern to our expression classes so that we can easily add an interpret operation for all of them, allowing easy matching.

For debugging our parser/interpreter later, it is useful to look at the parsed syntax tree and check if it has the structure we want. One thing we want is, given a syntax tree, produce an unambiguous string representation of it. This string should show the nesting structure of the tree. One way to make it unambiguous is to use Lisp/Racket syntax, where each expression is parenthesized in prefix order. 

Note that this printing is effectively an operation, and we can now test the visitor pattern we implemented prior to see how easy it is to define new ones for all types of operations.
