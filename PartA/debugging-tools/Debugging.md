# Debugging

debugger: program that runs other program with the user being allowed to excercise control over how these programs run

during program execution, users can
- insert breaks
- examine variables

## GNU(GDB) debugger

debugger of UNIX system to debug C/C++ programs

- can only be used to find runtime or logical errors
- compile time errors are detected by the compiler

#### To provide information for debugger

`g++ filename -g -o executable_name`

- when complie with `g++` command, add `-g`

### Shell command

- `[]`: optional parameter

#### Step into, over, out, return

```
def main():
	full_name = name_is()

def name_is():
	name = family_name() + "name" // current debugger location
	return name

def family_name():
	return "family"
```

- step into: enter the code of the current line if it is a function call
	- ex: run `family_name()`, currnet line become `return "family"` at  `family_name()`

- step over: execute the current line of code and pause on the next line
	- not enter any function calls that are present on the current line
	- ex: run `return name`, ignore `family_name()` method being invoked

- step out: run until next return
	- return to the caller’s context
	- ex: execute rest of `name_is()` and return to `full_name = name_is()` in `main()`

#### `gdb`

`gdb executable_name`

- start gdb

#### `quit`

- terminate reading commands

#### `help`

`help [command_name]

- get online help from gdb

#### `break`

- can only set where there is something to execute
	- blank, header import 이런 곳을 breakpoint로 지정하려고 하면 바로 다음 executable 한 코드가 있는 곳으로 대신 지정됨

`break [file:]function_in_file`

- set breakpoint at function

`break [file_name:]line_number`

- set breakpoint at a line number in a file

#### `run`

`run [arglist]`

- start program 

#### `bt`

- backtrace; display program stack

#### `print`

`print expr`

- display the value of an exepression

#### `c`

- continue running program; after stopping like breakpoint

#### `next`

- execute next program line; after stopping
- step over any function calls in the line

#### `step`

- execute next program line; after stopping
- step into any function calls in the line

#### `list`

`list [file:]function`

- type the text of the program in the vicinity(부근) of where it is presently stopped

#### `clear`

- clear breakpoint