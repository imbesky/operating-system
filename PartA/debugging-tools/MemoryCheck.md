# Memory check

## Valgrind

memory mismanagement detector

### Usage

help to identify
- memory leaks
- deallocation error
- use of uninitialized memory
- and other memory errors

and other things; because valgrind is a wrapper around a collection of tools
- cache profiling

### How to use

compile
`g++ file_name -g --no-warnings -o executable_name`

run valgrind
`valgrind --tool=memcheck --leak-check=yes --show-reachable=yes --num-callers=20 executable_name`