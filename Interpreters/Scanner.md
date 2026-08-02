# Scanning

As a reminder, a scanner takes in raw source code as a list of characters and groups into chunks called tokens, which form meaningful "words" for our language's grammar.

A lexeme is a list of characters that represents something. For example, given

```
var language = "bagel";
```

the lexemes are var, language, =, "lox", and ;. More specifically a lexeme is the raw substring. When combining it with some other data we get from scanning, we end up with tokens.

Often times tokens will represent specific keywords from the language. Instead of letting the parser do string comparison to the lexeme, we should encode the different kinds of lexemes in an enum. Then we can pass that TokenType info straight to the parser, so it immediately knows what kind of token it is and what it represents. This also gives type safety.

For ex.

```Java
enum TokenType {
  // Single-character tokens.
  LEFT_PAREN, RIGHT_PAREN, LEFT_BRACE, RIGHT_BRACE,
  COMMA, DOT, MINUS, PLUS, SEMICOLON, SLASH, STAR,

  // One or two character tokens.
  BANG, BANG_EQUAL,
  EQUAL, EQUAL_EQUAL,
  GREATER, GREATER_EQUAL,
  LESS, LESS_EQUAL,

  // Literals.
  IDENTIFIER, STRING, NUMBER,

  // Keywords.
  AND, CLASS, ELSE, FALSE, FUN, FOR, IF, NIL, OR,
  PRINT, RETURN, SUPER, THIS, TRUE, VAR, WHILE,

  EOF
}

```

For literal values like numbers and strings, since the scanner is walking thru them anyways, it can also directly convert the list of characters to the object.

For error handling, we need to report the line number. To do so, we can package the information into the token itself, along with the token type, the string, and the object (if it's a literal).

Notice how this token is now much more useful compared to the raw lexemes we could be passing to the parser.

The core of the scanner is a loop. Start at the first character, figure out what lexeme it's in, consume the characters in that lexeme. At the end, emit a token. Then, start from the next available character. Loop until you reach the end of the input; you now have a list of tokens.

The rules for determining how characters group into lexemes are a lexical grammar. The grammar is simple enough the language is a regular language.

In theory, we could use regexes for scanning since it's simple enough. In practice, let's just use conditionals to probe our way through.

At every turn, we scan a single token. Prepare a helper function for taking in input (reading the next token and advancing), and another one for producing output (the token).

Also, in the scanner, add a default function for when the user inputs an invalid character that will throw an error at the end.

For lexemes that are only a single character, it's straightforward, so we can check if the token is one of these first; if yes, then use the addToken helper to add it to the list.

For characters like '!' that could either represent a single '!' or a double '!=', the idea is that within that case, you look at the next token to see if it is an =. If it is, consume that one too and package the two into a single token, otherwise just return the token for the one character.

For '/', it can be used as both a division sign or a comment. We can do something similar: if the next token is also a '/', it's a comment, so consume all of it and don't need to product token since we don't care. Use a helper to peek (lookahead) at the next character.

Scanner should also have support for skipping over whitespace.

For string literals, once you hit a ", you should keep advancing forward while the string has not ended, then package it all into the token along with its literal.

For numbers, it's similar, but you also have to check if there's the decimal point.

The last thing to implement is reserved words and identifiers.

One important principle is called maximal munch. The idea is that if a list of characters could match two rules, you should assume it's the longest one when scanning.

For example, if you see or, it could be the keyword or, or it could be from the identifier orchid. If you assume it's or now, that would be bad. Therefore you cannot detect if it's a keyword until you've reached the end of the identifier.

Make the scanner store a hashmap of keyword lexemes and token types. After scanning the identifier, check if it is in the hashmap. If yes, then package it as that token. If not then it's just a generic user-identified identifier.
