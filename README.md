# miniC Compiler

A compiler for the miniC programming language that translates miniC source code to native machine code using LLVM. This project implements a complete compilation pipeline including lexical analysis, parsing, semantic analysis, and code generation.

## Synopsis

**miniC** is a simplified subset of the C programming language designed for educational purposes and compiler construction. This compiler (`mccomp`) reads miniC source files and generates executable machine code through LLVM IR (Intermediate Representation).

The miniC language supports:
- **Basic data types**: `int`, `float`, `bool`, and `void`
- **Variables**: Global and local variable declarations
- **Functions**: Function declarations with parameters and return values
- **Control flow**: `if`/`else` statements, `while` loops
- **Expressions**: Arithmetic operations (`+`, `-`, `*`, `/`, `%`), comparison operators (`<`, `<=`, `>`, `>=`, `==`, `!=`), logical operators (`&&`, `||`, `!`), and assignment
- **External functions**: Support for declaring and calling external C functions (e.g., standard library functions)

The compiler performs complete semantic analysis including:
- Type checking with automatic widening (bool → int → float)
- Scope resolution for variables and functions
- Function signature verification
- Return statement validation
- Detection of undefined variables and duplicate declarations

### Example miniC Program

```c
// Compute factorial of a number
int factorial(int n)
{
    int i;
    int result;

    result = 1;
    i = 1;

    while (i <= n) {
        result = result * i;
        i = i + 1;
    }
    
    return result;
}
```

## Architecture

The compiler follows a traditional multi-phase architecture:

1. **Lexer**: Tokenizes the input source code into a stream of tokens
2. **Parser**: Implements a recursive descent parser based on an LL(1) grammar to build an Abstract Syntax Tree (AST)
3. **Semantic Analyzer**: Validates the AST for semantic correctness (type checking, scope resolution, etc.)
4. **Code Generator**: Traverses the AST to generate LLVM IR, which is then compiled to native machine code

## Language Grammar

The miniC grammar is defined in `grammar.txt`. Key productions include:

- **Program Structure**: External declarations followed by function/variable declarations
- **Type System**: Support for `int`, `float`, `bool`, and `void` types
- **Expressions**: Follows C operator precedence with logical OR at the lowest precedence down to unary operators
- **Statements**: Expression statements, blocks, conditionals, loops, and returns

See `grammar.txt` for the complete formal grammar specification.

## Building the Compiler

### Prerequisites

- C++ compiler with C++17 support (e.g., `clang++`)
- LLVM development libraries installed
- `llvm-config` utility in your PATH

### Compilation

```bash
make
```

This generates the `mccomp` executable.

## Usage

```bash
./mccomp <source-file.c>
```

The compiler will:
1. Parse and analyze the source file
2. Generate LLVM IR
3. Output an object file that can be linked with other code

## Error Handling

The compiler provides detailed error messages with line and column numbers for both syntax and semantic errors:

```
3:12: syntax error: Expected ';' but found '}'
5:8: semantic error: Undefined variable 'x' referenced
```