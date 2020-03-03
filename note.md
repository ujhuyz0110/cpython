# Note

## Directory Overview

- `Doc`: Source of the documentation
- `Grammar`: Computer-readable language definition
- `Include`: C header files
- `Lib`: Standard library modules written in Python
- `Mac`: macOS support files
- `Misc`: Miscellaneous files
- `Modules`: Standard Library Modules written in C
- `Objects`: Core types and the object model
- `Parser`: The Python parser source code
- `PC`: Windows build support files
- `PCbuild`: Windows build support files for older Windows
- `Programs`: Source code for the Python executable and other binaries
- `Python`: The CPython interpreter source code
- `Tools`: Standalone tools useful for building or extending Python

## Python interpreter

- Python interpreter converts Python source code and execute it in one step.
- Python interpreter compiles source code into bytecode that only CPython understands.

## CPython and C

- CPython is written in pure C.
- Why? When a new language is developed, it is often written first in an order and more established language.
- PyPy is a Python compiler written in Python

## The Python Language Specification

- .rst files in `Doc/reference` are used to produced the official guides on docs.python.org
- The `Grammar` file in `Grammar/Grammar` is written in a context-notation called "Backus-Naur Form (BNF)". Python's grammar file uses the Extended-BNF specification with regular-expression syntax.
    - Anything in quotes is a string literal, which is how keywords are defined.
    - `*` for repetition
    - `+` for at-least-once repetition
    - `[]` for optional parts
    - `|` for alternatives
    - `()` for group
 - Instead of the `grammar` file, the Python compiler uses a tool called `pgen`. `pgen` reads the grammar file and converts it into a parser table.
    - `pgen` work by converting the EBNF statements into a Non-deterministic Finite Automation (NFA), which is then turned into a Deterministic Finite Automation (DFA)
 - `Tokens` file contains each of the unique types found as a leaf node in a parse tree.
    - There are two versions of tokenizers with exactly the same output and behavior. The C version is for performance while the Python version is for debugging.
    - to see tokens in action: `./python.exe -m tokenize -e test_tokens.py`
    - to see a verbose readout of the C tokenizer: `./python.exe -d test_tokens.py`
 - To visualize `pgen`, use `instaviz`.
 
## Memory Management in CPython

 - `PyArena` object is one of CPython's memory management structures. The code can be found in`Python/pyarena.c`.
 - When an interpreter is instantiated, a PyArena is created and attached one of the fields in the interpreter. There could be multiple arenas created during the life cycle of an interpreter. The arena stores a list of pointers to Python objects as a PyListObject. It uses `PyArena_AddPyObject()` to add new Python objects being created.
 - `PyArena` also allocate and reference a list of raw memeory blocks. When extra memory is needed, `PyArena` calls `PyArena_Malloc()` with the required memory size. This task is completed by `Objects/obmalloc.c`.