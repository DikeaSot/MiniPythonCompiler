# MiniPython Compiler

Assignment for Compilers AUEB course. Winter Semester 2019-20

---

## Project Description

The aim of this assignment is to implement a compiler for miniPython language.

## MiniPython Language

MiniPython is a subset of Python and so, the content of a program in miniPython is the same with the content of the same program in Python.

You will be using the tool [SableCC](http://www.sablecc.org/), which will make it easier for you to construct the compiler.
Additional information can be found on the book Modern Compiler Implementation in Java, Andrew W. Appel.

### Supported Commands

- Integer, decimal and string assignment to variables
- List assignment to variables
- Function declaration with simple arguements or with default values
- If, print, while, for commands
- Function calls
- Single-line commands

[Code Example](https://github.com/DikeaSot/MiniPythonCompiler/blob/main/Part%202/src/example.py "example.py")

[Language BNF](#bnf-for-minipython)

## Assignment Structure

There are two (2) phases for this assignment

- **Phase 1**
  - [Lexical Analysis](#lexical) for miniPython
  - [Syntax Analysis](#syntax) for miniPython

- **Phase 2**
  - [Abstract Syntax Trees (AST)](#ast) for miniPython
  - [Symbol Semantic](#semantic) for miniPython

## Tips

[Here](http://docs.python.org/tutorial/, "Python Guide") you can find a guide for Python programming language.

---

## <a name="bnf-for-minipython"></a> BNF for miniPython

## NON-TERMINALS

---

Goal
:   ::= ([Function](#Function)\|[Statement](#Statement))\* \<EOF>

[Function](#Function)
:   ::= [\"def\"]{.string} [Identifier](#Identifier) [\"(\"]{.string}
    [Argument](#Argument)? [\")\"]{.string} [\":\"]{.string}
    [Statement](#Statement)

[Argument](#Argument)
:   ::= [Identifier](#Identifier) ([\"=\"]{.string} [Value](#Value) )? (
    [\",\"]{.string} [Identifier](#Identifier) ([\"=\"]{.string}
    [Value](#Value) )?)\*

[Statement](#Statement)
:   ::= **tab\*** [\"if\"]{.string} [Comparison](#Comparison)
    [\":\"]{.string} [Statement](#Statement)
:   \|**tab\*** [\"while\"]{.string} [Comparison](#Comparison)
    [\":\"]{.string} [Statement](#Statement)
:   \|**tab\*** [\"for\"]{.string} [Identifier](#Identifier)
    [\"in\"]{.string} [Identifier](#Identifier) [\":\"]{.string}
    [Statement](#Statement)
:   \|**tab\*** [\"return\"]{.string} [Expression](#Expression)
:   \|**tab\*** [\"print\"]{.string} [Expression](#Expression)
    ([\",\"]{.string} [Expression](#Expression))\*
:   \|**tab\*** [Identifier](#Identifier) ( [\"=\"]{.string} \|
    [\"-=\"]{.string} \| [\"/=\"]{.string} ) [Expression](#Expression)
:   \|**tab\*** [Identifier](#Identifier) [\"\[\"]{.string}
    [Expression](#Expression) [\"\]\"]{.string} [\"=\"]{.string}
    [Expression](#Expression)
:   \|**tab\*** [\"assert\"]{.string} [Expression](#Expression)
    ([\",\"]{.string} [Expression](#Expression) )?
:   \|**tab\*** [Function Call](#Function-Call)

[Expression](#Expression)
:   ::= [Expression](#Expression) ( [\"+\"]{.string} \| [\"-\"]{.string}
    \| [\"\*\"]{.string} \| [\"/\"]{.string} \| [\"%\"]{.string} \|
    [\"\*\*\"]{.string} ) [Expression](#Expression)
:   \| ( [\"++\"]{.string} \| [\"\--\"]{.string} )
    [Expression](#Expression)
:   \| [Expression](#Expression)( [\"++\"]{.string} \|
    [\"\--\"]{.string} )
:   \| [Identifier](#Identifier) [\"\[\"]{.string}
    [Expression](#Expression) [\"\]\"]{.string}
:   \| [Function Call](#Function-Call)
:   \| [Value](#Value)
:   \| [Identifier](#Identifier)
:   \| [\"(\"]{.string} [Expression](#Expression) [\")\"]{.string}
:   \| [\"\[\"]{.string}[Value](#Value) ( [\",\"]{.string}
    [Value](#Value))\*[\"\]\"]{.string}

[Comparison](#Comparison)
:   ::= [Comparison](#Comparison) ( [\"and\"]{.string} \|
    [\"or\"]{.string}) [Comparison](#Comparison)
:   \| [\"not\"]{.string} [Comparison](#Comparison)
:   \| [Expression](#Expression) ( [\"\>\"]{.string} \|
    [\"\<\"]{.string} \| [\"\>=\"]{.string} \| [\"\<=\"]{.string}
    \|[\"!=\"]{.string} \| [\"==\"]{.string}) [Expression](#Expression)
:   \| [\"true\"]{.string}
:   \| [\"false\"]{.string}

[Function Call](#Function-Call)
:   ::= [Identifier](#Identifier) [\"(\"]{.string} [Arglist](#Arglist)?
    [\")\"]{.string}

[Arglist](#Arglist)
:   ::= [Expression](#Expression) ( [\",\"]{.string}
    [Expression](#Expression) )\*

[Value](#Value)
:   ::= [Identifier](#Identifier) [\".\"]{.string} [Function
    Call](#Function-Call) \| [Number](#Number) \|
    \"\<STRING_LITERAL>\"\|\'\<STRING_LITERAL>\' \| [\"None\"]{.string}

[Number](#Number)
:   ::= \<INTEGER_LITERAL>

[Identifier](#)
:   ::= \<IDENTIFIER>

\<STRING_LITERAL> = A word that contains letters (lower and upper case) and blanks\
\<INTEGER_LITERAL> = An integer or a decimal number\
\<IDENTIFIER> = Variable name

[More information about operators' priority](http://www.mathcs.emory.edu/~valerie/courses/fall10/155/resources/op_precedence.html)

## <a name="lexical"></a> Lexical Analysis

