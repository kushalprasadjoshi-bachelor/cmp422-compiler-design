# Lab 2: Lexical Analyzer for Token Recognition Using Flex

## 1. Objective

**Implement a lexical analyzer using Flex that reads source code and recognizes tokens such as identifiers, separators, operators, and handles unknown symbols.**

---

## 2. Technical Theory

A lexical analyzer, or scanner, reads source code and groups characters into tokens that represent the smallest meaningful units of a language. Flex generates the scanner from regular expressions for identifiers, separators, operators, and other symbols. The scanner matches each lexeme, classifies it, and prints the token type. Characters that do not match any defined pattern are reported as unknown symbols, helping detect unsupported or invalid input early in the compilation process.

The lexical analyzer reads from standard input and scans tokens sequentially. In this example, the analyzer correctly identifies:

- `printf` and other names as identifiers.
- `(`, `)`, and `;` as separators.
- `=` and `+` as operators.

String quotes are reported as unknown symbols because the current lexer rules do not define string literals or quote handling.

---

## 3. Lab Environment

- **OS:** Linux / Windows with MinGW
- **Compiler:** GCC (GNU Compiler Collection)
- **Text Editor:** VS Code

---

## Source Code

```
%{
#include <stdio.h>
#include <string.h>

int line_no = 1;
%}

/* Definitions */
DIGIT      [0-9]
ID         [a-zA-Z_][a-zA-Z0-9_]*
KEYWORD    int|float|if|else|return
OP         ==|!=|<=|>=|\+|\-|\*|\/|>|=
SEPARATOR  [;,(){}]

%%

{KEYWORD}   { printf("Keyword: %s\n", yytext); }
{ID}        { printf("Identifier: %s\n", yytext); }
{DIGIT}+    { printf("Number: %s\n", yytext); }
{OP}        { printf("Operator: %s\n", yytext); }
{SEPARATOR} { printf("Separator: %s\n", yytext); }

\n          { line_no++; }

[ \t]       ;

.           { printf("Unknown symbol: %s\n", yytext); }

%%

int main() {
    printf("Enter the code (Ctrl+Z or Ctrl+D to end input):\n");
    yylex();
    return 0;
}

int yywrap() {
    return 1;
}
```

---

## Sample Input and Output

**Input:**

```c
printf("Hello World");
z = x + y;
```

**Output:**

```
Enter the code (Ctrl+Z or Ctrl+D to end input):
printf("Hello World");
Identifier: printf
Separator: (
Unknown symbol: "
Identifier: Hello
Identifier: World
Unknown symbol: "
Separator: )
Separator: ;

z = x + y;
Identifier: z
Operator: =
Identifier: x
Operator: +
Identifier: y
Separator: ;

^Z
```

---

## Conclusion

This lab demonstrates token recognition using Flex. The analyzer can distinguish identifiers, operators, and separators, while highlighting unsupported or unrecognized characters for future enhancement.

---

_Thank you!_
