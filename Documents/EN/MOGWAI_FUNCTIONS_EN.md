# GLOSSARY

## LANGUAGE FUNCTIONS

### `mogwai.doworks`

Forces **MOGWAI** to process pending events and timers.
***

### `mogwai.reset`

Forces **MOGWAI** to perform a runtime reset.
***

### `mogwai.info`

Returns a record containing various information about the runtime and the system it runs on.

```
mogwai.info ?d
```

Will display:

```
name:             "MOGWAI CLI"
version:          "6.48.0.0"
platform:         "WINDOWS"
architecture:     "X64"
OSdescription:    "Microsoft Windows 10.0.26200"
framework:        ".NET 7.0.20"
runtimeID:        "win10-x64"
prompt:           "MOGWAI RUNTIME 6.48.0.0 © 2016-2025 Stéphane SIBU ...
keywords:         ('mogwai.doworks' 'mogwai.reset' 'mogwai.info' 'mo ...
skills:           ("SQLDB" "SERIAL")
extensions:       ()
debug:            false
keepAlive:        true
isTask:           false
fileSupport:      true
```

| Key | Meaning |
|-----|---------|
| `name:` | Runtime name.|
| `version:` | Runtime version.|
| `platform:` | Name of the platform on which the runtime runs.|
| `architecture:` | Platform architecture.|
| `OSdescription:` | Full platform description.|
| `framework:` | .NET runtime version |
| `runtimeID:` | Platform runtime ID.|
| `prompt:` | **MOGWAI** runtime prompt.|
| `skills:` | List of **MOGWAI** runtime skills.|
| `extensions:` | List of loaded extensions.|
| `keepAlive:` | true if the **MOGWAI** runtime keeps its execution context from one session to another.|
| `isTask:` | true if the **MOGWAI** runtime is a child task.|
| `fileSupport:` | true if the **MOGWAI** runtime can perform file operations.|
***

### `mogwai.exit`

Forces the runtime to stop the current execution without raising an error.
***

### `mogwai.halt`

Forces the runtime to stop the current execution and raises error 17 "halt encountered".
***

### `mogwai.cclear`

Clears the cache of procedures included via the `include` command.
<br>Ensures that the included code is the latest version.
***

### `mogwai.memory`

Returns the amount of memory allocated by the system for the runtime.
***

### `mogwai.explicit`

If `true` is passed as parameter, all variables must be declared before being used.
<br>Variables are declared with the `def` function.
>By default `mogwai.explicit` is disabled. 
***

### `mogwai.isTask`

Returns `true` if the runtime is a child task (see task management).
***

### `env.machineName`

Returns the name of the machine on which the runtime is running as a string.
***

### `reveal`

Takes a user function as parameter (name or code) and returns the corresponding 100% RPN version. This 100% RPN version will be the code actually used for execution (all syntactic sugar transformed into final code).

```
to 'test' do « 1 10 for 'i' do { i ? } »

'test' reveal ?

# Will display the following code:
# « 1 10 'i' { i ? } FOR »
```
***

### `args`

Returns the list of arguments passed to the engine during its creation by the host as a list of strings.
***
 
### `defunc`

100% RPN function that allows declaring a function.

```
to 'square' do « dup * »

# Is identical to

« dup * » 'square' defunc
```
***

### `funcs`

Returns the list of defined user functions.
***

### `def`

Allows declaring a variable before using it.

If `mogwai.explicit` is enabled, variables must be declared before use (disabled by default).

When a variable is explicitly declared, it always keeps the assigned type and cannot change type on the fly unless using the `.any` type.

To declare a variable, you must provide its type and name to the `def` function.

```
.string 'myString' def
.number 'myNumber' def
```
***

### `const`

Allows declaring a constant. Its value is immutable.

```
500 'MY_CONSTANT' const
```
***

### `sto` or `->`

Stores a value in a variable.

```
50 'A' sto
```

A syntactic sugar version exists, more readable:

```
50 -> 'A'
```
***

### `sto+` or `->+`
Adds a value to a variable.

```
50 'A' sto+
```

Syntactic sugar version:

```
50 ->+ 'A'
```
***

### `sto-` or `->-`

Subtracts a value from a variable.

```
50 'A' sto-
```

Syntactic sugar version:

```
50 ->- 'A'
```
***

### `sto*` or `->*`

Multiplies a variable by a value:

```
50 'A' sto*
```

Syntactic sugar version:

```
50 ->* 'A'
```
***

### `sto/` or `->/`

Divides a variable by a value:

```
50 'A' sto/
```

Syntactic sugar version:

```
50 ->/ 'A'
```
***

### `++`

Increments a variable.

```
'A' ++
```
***

### `--`

Decrements a variable.

```
'A' --
```
***

### `apply` or `-->`

Applies a process to a variable.

For example, `A 1 + -> 'A'` can be replaced by `{ 1 + } --> 'A'` or `{ 1 + } 'A' apply`
***

### `rcl`

Pushes the value of a variable whose name is passed as parameter onto the stack.

```
100 -> '$A'
'$A' rcl ?

# Displays 100
```
***

### `purge`

Deletes a variable whose name is passed as parameter.

```
'$A' purge
```
***

### `exists`

Returns `true` if the variable whose name is passed as parameter exists.

```
'$A' exists
```
***

### `eval`

Evaluates an object on the stack.

The behavior differs depending on the type of object evaluated:
- Functions and code blocks are executed. 
- Strings are updated with control characters and replacement blocks.
- Dynamic elements of a list are replaced by their current value.
- Dynamic elements of a record are replaced by their current value.

```
"Mr. X" -> 'name'
"The name is {! Name}" eval ?

# Displays "The name is Mr. X"

[x: 50 name: name] eval

# Pushes [x: 50 name: "Mr. X"] onto the stack

```
***

### `include`

Includes and immediately executes code from a file.

```
"my code.mog" include
```
***

### `import`

Imports an extension library in ***MOGWAI*** format.

It is a .dll file (a .NET library) but you only specify the filename without the extension.

```
"maths" import
```
***

### `imports`

Lists the imports performed and available.
***

### `import.keywords`

Lists all available functions in a loaded extension (via the import function).
***

### `get`

Returns the value of a key in a record, an element of a list or a data:

| Action | Result |
|--------|--------|
| `(1 2 3 4) 1 get` | will return 2|
| `[x: 10 y: 20] x: get` | will return 10|
| `[x: 10 l: (1 2 3)] (l: 1) get` | will return 2|
| `DATA:FFEA10 1 get` | will return 234 (0xEA)|

Returns the value of a property of an object, or executes an object's method:

| Action | Result |
|--------|--------|
| `@1 name: get` | will return the value of the 'name' property of object @1.|
| `@1 display: get` | will execute the display function (if display is a method) of object @1.|
***

### `set`

Modifies the value of a key in a record, list or data:

| Action | Result |
|--------|--------|
| `(1 2 3 4) 0 10 set` | will return `(10 2 3 4)`|
| `[x: 10 y: 20] x: 100 set` | will return `[x: 100 y: 20]`|
| `DATA:FFEA10 0 0xAA set` | will return `DATA:AAEA10`|

Modifies the value of a property of an object:

| Action | Result |
|--------|--------|
| `@1 name: "SMITH" set` | will assign "SMITH" to the 'name' property of object @1.|
***

### `size`

Returns the size of a record, list, data, binary or string.
***

### `resize`

Modifies the size of a binary.

```
BIN:11011 8 resize

# Will push BIN:00011011 onto the stack
```
***

### `keys`

Returns a list composed of the keys of a record.

```
[x: 10 y: 50 z: 100] keys 

# Will push (x: y: z:) onto the stack
```
***

### `first`

Returns the first element of a string, list or data.
***

### `last`

Returns the last element of a string, list or data.
***

### `butfirst`

Returns all elements except the first of a string, list or data.
***

### `butlast`

Returns all elements except the last of a string, list or data.
***

### `contains`

Returns `true` if an element is present in a string, record, list or data.

| Action | Result |
|--------|--------|
| `"TOTO" "T" contains` | will return `true`.|
| `[x: 50 y: 100] x: contains` | will return `true`.|
| `(10 "EEE" 20 50) 20 contains` | will return `true`.|
| `DATA:FF00FFAB 0xFF contains` | will return `true`.|
***

### `where`

Returns a list of all locations of an element in a string, list or data.

| Action | Result |
|--------|--------|
| `"HELLO WORLD" "O" where` | will return `(4 7)`|
| `(10 100 40 10 24) 10 where` | will return `(0 3)`|
| `DATA:45ED23FF0645DD 0x45` | where will return `(0 5)`|
***

### `split`

Returns a list composed of elements of a string separated by a string containing the separator (which can consist of multiple characters).

| Action | Result |
|--------|--------|
| `"X1;X45;Z34;12" split` | will return `("X1" "X45" "Z34" "12")`|
***

### `join`

Recomposes a string from elements of a list and a separator.

> Inverse function of split.

| Action | Result |
|--------|--------|
| `("X1" "X45" "Z34" "12") ";" join` | will return `"X1;X45;Z34;12"`|

### `like`

Returns `true` if a string matches a particular pattern.

```
?           = Any single character
*           = Zero or more characters
#           = Any digit (0 to 9)
[charlist]  = Any single character in charlist
[!charlist] = Any single character not in charlist
```

```
"MR SMITH 62" "M? SM*H ??" like

# Pushes true onto the stack
```
***

### `right`

Returns the last n characters of a string.

```
"Hello world!" 6 right

# Pushes "world!" onto the stack
```
*** 

### `left`

Returns the first n characters of a string.

```
"Hello world!" 6 left

# Pushes "Hello " onto the stack
```
***

### `extract`

Extracts multiple elements from a list or data by specifying which elements to extract in a list.

```
(10 20 30 40) (0 1 3) extract 

# will return (10 20 40)

DATA:FF45AB23 (0 1 3) extract

# will return DATA:FF4523

[x: 10 y: 20 z: 100] (x: z:) extract 

# will return [x: 10 z: 100]
```
***

### `sleep`

Suspends the runtime for a time expressed in milliseconds. 
> Warning, sleep blocks the processing of all event and timer type messages.
***

### `wait`

Like `sleep`, suspends the runtime for a time expressed in milliseconds without blocking the processing of event and timer type messages.
***

### `rand`

Returns a random number between 0 and 1.
***

### `sub`

Extracts a part of a list or data by specifying the start and extent.

```
(10 20 30 40 50) 1 3 sub
# Pushes (20 30 40) onto the stack

DATA:05FFEDAB2312 2 3 sub 
# Pushes DATA:EDAB23 onto the stack

BIN:1001111001111 2 4 sub 
# Pushes BIN:0011 onto the stack
```
***

### `break`

Forces exit from a for, while, foreach, forever and during loop.
***

### `return`

Forces exit from a function.
***

### `run`

Starts execution of a program whose code is in a file.

```
"my program.mog" run
```
***

### `new`

Pushes a ***MOGWAI*** object onto the stack by specifying its type.

```
.list new
# Pushes an empty list onto the stack.

.string new 
# Pushes an empty string onto the stack.

.record new 
# Pushes an empty record onto the stack.
```
***

### `isSupported`

Returns `true` if a function whose name is passed as parameter is supported by the current platform.

```
'sto' isSupported 
# Pushes true onto the stack.

'serial.open' iSupported 
# Pushes true onto the stack under windows, osx and linux.
```
***

### `isSkill`

Returns `true` if the skill whose name is passed as parameter is supported by the runtime.

```
"SQLDB" isSkill
# will return true if SQLite support is enabled.
```
***

### `skills`

Returns the list of runtime skills. Each skill is provided as a string.
***

### `flags`

Returns the list of all active flags.
***

### `flag.set`

Activates the flag whose name is passed as parameter.
***

### `flag.clear`

Deactivates the flag whose name is passed as parameter.
***

### `flag.isSet`

Returns true if the flag whose name is passed as parameter is active.
***

### `flag.isClear`

Returns true if the flag whose name is passed as parameter is inactive.
***

### `unique`

Returns a unique code as a string.
<br>Ex: "DEC378AF69F246B6A1688799F70A987A"
***

### `guid`

Returns a unique code in UUID (or GUID) format as a string.
<br>Ex: "392BDA7A-9BEB-43B2-ACC7-05C8A06B0F44"
***

### `json->`

Creates a list or record from a json formatted string.
***

### `->json`

Creates a json formatted string from a list or record.
***

### `escape`

Escapes a string passed as parameter.

Quotes are replaced by `\"`, line breaks by `\r` and/or `\n`, etc… 
***

### `help`

Returns a string containing the help for a function whose name is passed as parameter.

> Very few functions can respond to this command at the moment (but this will change).

```
'sto' help ?
```

Will display for example:

```
*********************
** sto (primitive) **
*********************
Stores a value in a variable.
50 'A' sto
"Hello" 'message' sto
(1 2 3) 'mylist' sto
```
***

### `?help`

Displays the help for a function whose name is passed as parameter.
***

### `error.last`

Returns the code of the last raised error.
***

### `error.reset`

Resets the code of the last raised error to zero.
***

### `error.throw`

Artificially raises the error whose code is passed as parameter.
***

### `+`

Adds 2 objects together.

Possible combinations are:
- 2 numbers
- 1 list and an object
- 2 strings
- 1 data and a byte
- 2 data
- 2 lists
***

### `-` 

Subtracts 2 numbers.
***

### `*`

Multiplies 2 numbers.
***

### `/`

Divides 2 numbers.
***

### `<`

Returns `true` if the first parameter is less than the second.
***
 
### `>`
Returns `true` if the first parameter is greater than the second.
***

### `<=`
Returns `true` if the first parameter is less than or equal to the second.
***

### `>=`
Returns `true` if the first parameter is greater than or equal to the second.
***

### `==`
Returns `true` if the first parameter is equal to the second.
***

### `!=`
Returns `true` if the first parameter is different from the second.
***

### `and`
Performs the logical AND operation between the first and second parameter.
***

### `or`
Performs the logical OR operation between the first and second parameter.
***

### `xor`
Performs the logical XOR operation between the first and second parameter.
***

### `not`
Performs the logical NOT operation between the first and second parameter.
***

### `isnull`
Returns `true` if the object passed as parameter is `null`.
***

### `isRecord`
Returns `true` if the object passed as parameter is of type `.record`
***

### `isList`
Returns `true` if the object passed as parameter is of type `.list`
***

### `isData`
Returns `true` if the object passed as parameter is of type `.data`
***

### `isBoolean`
Returns `true` if the object passed as parameter is of type `.boolean`
***

### `isNumber`
Returns `true` if the object passed as parameter is of type `.number`
***

### `isString`
Returns `true` if the object passed as parameter is of type `.string`
***

### `isName`
Returns `true` if the object passed as parameter is of type `.name`
***

### `isKey`
Returns `true` if the object passed as parameter is of type `.key`
***

### `drop`
Removes the first element from the stack.
***

### `drop2`
Removes the first 2 elements from the stack.
***

### `dropn`
Removes the last n elements from the stack. The number of elements to remove is passed as parameter.
***

### `swap`
Swaps the first 2 elements of the stack.
***

### `dup`
Duplicates the first element of the stack.
***

### `dup2`
Duplicates the first 2 elements of the stack.
***

### `dupn`
Duplicates the last n elements of the stack. The number of elements to remove is passed as parameter.
***

### `pick`
Returns the value of the nth element of the stack (0 being the first element) without removing it from the stack.
***

### `over`
Takes the 2nd element of the stack (without removing it) and adds it to the stack.
***

### `roll`
Removes the nth element of the stack (passed as parameter) and adds it to the stack.
***

### `rolld`
Removes the first element of the stack and inserts it at the position passed as parameter.
***

### `rot`
Swaps the 1st and 3rd elements of the stack.
***

### `depth`
Returns the number of elements in the stack.
***

### `clear`
Clears the stack.
***

### `sign`
Returns a list containing the type of the n elements of the stack without modifying the stack.

```
# Place elements on the stack
10 "EEE"

# Request the type of these 2 elements
2 sign

# The list (.string .number) is pushed onto the stack
```
***

### `->type`
Returns the type of the object passed as parameter.
***

### `compress`
Returns a data that is the result of compressing a data passed as parameter.
***

### `decompress`
Returns a data that is the result of decompressing a data passed as parameter. The data passed as parameter is normally the result of the `compress` function.
***

### `vars`
Returns the list of all existing global variables.
***

### `lvars`
Returns the list of all existing local variables.
***

## `IF`
100% RPN function corresponding to the syntactic sugar `if`.

```
{ A 100 == } { "A equals 100" ? } IF

# If A has value 100, will display "A equals 100"
# otherwise will do nothing.
```
***

### `IFELSE`
100% RPN function corresponding to the syntactic sugar `if … else`.

```
{ A 100 == } { "A equals 100" ? } { "A is different from 100" ? } IFELSE

# If A has value 100, will display "A equals 100"
# otherwise will display "A is different from 100"
```
***

### `SWITCH`
100% RPN function corresponding to the syntactic sugar `switch`.

```
{
    { A 100 == } { "A = 100" ? } 
    
    { A 100 > } { "A > 100" ? }

} SWITCH
```
***

### `REPEAT`
100% RPN function corresponding to the syntactic sugar `repeat`.

```
3 { "Hello!" } REPEAT
```
***

### `WHILE`
100% RPN function corresponding to the syntactic sugar `while`.

```
{ A 10 < } { A ? 'A'  ++ } WHILE
```
***

### `DOWHILE`
100% RPN function corresponding to the syntactic sugar `do … while`.

```
{ A 10 < } { A ? 'A' ++ } DOWHILE
```
***

### `FOREACH`
100% RPN function corresponding to the syntactic sugar `foreach`.

```
(1 2 3 4) 'A' { A ? } FOREACH
```
***

### `FOREVER`
100% RPN function corresponding to the syntactic sugar `forever`.

```
{ A ? 'A' ++ } FOREVER
```
***

### `FOR`
100% RPN function corresponding to the syntactic sugar `for`.

```
1 10 'A' { A ? } FOR
```
***

### `FORSTEP`
100% RPN function corresponding to the syntactic sugar `for … step`.

```
1 100 5 'A' { A ? } FORSTEP
```
***

### `DURING`
100% RPN function corresponding to the syntactic sugar `during`.

```
2500 { A ? 'A' ++ } DURING
```
***

### `LATER`
100% RPN function corresponding to the syntactic sugar `after`.

```
5000 « "Hello" ? » LATER { } FOREVER
```
***

### `GUARD`
100% RPN function corresponding to the syntactic sugar `guard … else`.

```
{ "MyFile" file.read } { "File access error!" ? } GUARD
```
***

### `TRAP`
100% RPN function corresponding to the syntactic sugar `trap`.

```
{ "MyFile" file.read } TRAP
```
***

### `print` or `??`
Displays a string on screen without a line break.

```
"Hello " print "world!" println

# Will display:
# Hello world!
```
***

### `println` or `?`
Displays a string on screen with a line break.

***

### `?vars` or `?v`
Displays the list of global variables on screen with a "clear" presentation.

```
100 -> '$A' "HELLO" -> '$B' (10 20 30) -> '$C' ?vars
```
Will display:

```
$A = 100
$B = "HELLO"
$C = (10 20 30)
```
***

### `?stack` or `?s`
Displays the stack on screen. The first element is displayed last.

```
10 20 30 40 50 ?stack
```

Will display:

```
4 : 10
3 : 20
2 : 30
1 : 40
0 : 50
```
***
 
### `?dump` or `?d`
Displays lists, records and data on screen in a "clearer" version.

```
(10 20 30 40 50) ?dump
```

Will display:

```
0 : 10
1 : 20
2 : 30
3 : 40
4 : 50
```

```
[x: 100 y: 50 z: "HELLO"] ?dump
```

Will display:

```
x:  100
y:  50
z:  "HELLO"
```

```
DATA:5612FFEA1789AD34C5FAFEFF01021020ABACA0 ?dump
```

Will display:

```
00000000  56 12 FF EA 17 89 AD 34 C5 FA FE FF 01 02 10 20  | V.ÿê.?­4Åúþÿ...   |
00000010  AB AC A0
```
***

### `cls`
Clears the screen.
***

### `input`
Waits for keyboard input (ending with validation by the `ENTER` key) and returns the corresponding string.
***

### `prompt`
Like the `input` function but displays a prompt message passed as parameter.

```
"What is your first name? " prompt
"Your first name is: " swap + ?
```

Will display the prompt, then you can enter (for example STEPHANE)

```
What is your first name? STEPHANE
```

Then once the input (STEPHANE) is validated:

```
Your first name is: STEPHANE
```
***

### `console.show`
Shows the output console (if managed by the host).
> Has no effect in **MOGWAI CLI**.
***

### `console.hide`
Hides the output console (if managed by the host).
> Has no effect in **MOGWAI CLI**.
***

### `->list`
Builds a list from elements present on the stack. You must pass the number of elements to take as parameter. An error is raised if the stack does not contain enough elements.

```
10 20 30 40 50 5 ->list ?

# Pushes (10 20 30 40 50) onto the stack
```
***

### `->int`
Converts a number passed as parameter to an integer.
***

### `->str`
Converts an object passed as parameter to a string.
***

### `->format`
Converts a number to a string using a format.

```
50 "000" ->format ?
# Will display 050

50.8 "000.000" ->format ?
# Will display 050.800
```
***

### `->vars`
Extracts values and assigns them to locally created variables.

With a record, extracts the values of all keys and creates the corresponding local variables for the extracted keys:

```
[x: 10 y: 20 z: 50] ->vars 
"x={! x}" eval ? 
"y={! y}" eval ? 
"z={! z}" eval ?
```

Will display:

```
x=10
y=20
z=50
```
***

With the stack, extracts values and creates the corresponding local variables:

```
20 30 40 ('a' 'b' 'c') ->vars 
"a={! a}" eval ? 
"b={! b}" eval ? 
"c={! c}" eval ?
```

Will display:

```
a=20
b=30
c=40
```
***

### `->safeVars`
Verifies that the values present on the stack are as expected. 
You can check their number and type, and automatically assign local variables with stack values. An error is raised in case of non-compliance.

```
"EEE" 50 [x: .string y: .number] ->safeVars 
"x={! x}" eval ? 
"y={! y}" eval ?
```

Will display:

```
x=EEE
y=50
```
***

### `->params`
Allows passing named parameters (key/value pairs in a record) and verifying that expected parameters are present and their types match. 
If everything is correct, local variables corresponding to expected parameters are automatically created with their corresponding values.

```
[x: 100 y: "HELLO"] [x: .number y: .string] ->params 
"x={! x}" eval ? 
"y={! y}" eval ?
```

Will display:

```
x=100
y=HELLO
```
***

### `->num`
Converts a string to a number when possible. 
If impossible, an error is raised.
***

### `->name`
Converts a string or key to a name.
***

### `->key`
Converts a string or name to a key.
***

### `->data`
Takes n elements from the stack and makes a data. 
The number of elements to take is passed as parameter.

```
0xFF 0xAB 0x45 3 ->data

# Pushes DATA:FFAB45 onto the stack
```
***

### `->hex`
Converts a number to a string in hexadecimal format.

```
255 ->hex 

# Pushes "FF" onto the stack
```
***

### `hex->`
Converts a hexadecimal format string to a number.

```
"FF" hex->

# Pushes the number 255 onto the stack
```
***

### `->bin`
Converts a number to a binary object.

```
278 ->bin

# Pushes BIN:100010110 onto the stack
```
***

### `->upper`
Converts a string to uppercase.
***

### `->lower`
Converts a string to lowercase.
***

### `->program`
Converts a list to a function.

```
( 2 2 + ) ->program

# Pushes « 2 2 + » onto the stack
```
***

### `->keyword`
Converts a string to a **MOGWAI** function name (keyword).

>Warning, the keyword is placed on the stack and is not automatically executed.<br>
To execute it, you must use the eval function.

```
10 '$A' "sto" ->keyword eval
$A

# Is equivalent to writing:
# 10 '$A' sto 
# $A
# Which pushes the content of $A onto the stack, the number 10
```
***

### `->u8`
Converts a number to an unsigned 8-bit integer. 
The result is returned as a data.
***

### `->i8`
Converts a number to a signed 8-bit integer. 
The result is returned as a data.
***

### `->u16`
Converts a number to an unsigned 16-bit integer. 
The result is returned as a data.
***

### `->i16`
Converts a number to a signed 16-bit integer. 
The result is returned as a data.
***

### `->u32`
Converts a number to an unsigned 32-bit integer. 
The result is returned as a data.
***

### `->i32`
Converts a number to a signed 32-bit integer. 
The result is returned as a data.
***

### `->u64`
Converts a number to an unsigned 64-bit integer. 
The result is returned as a data.
***

### `->i64`
Converts a number to a signed 64-bit integer. 
The result is returned as a data.
***

### `->utf8`
Converts a data to a UTF-8 encoded string.
***

### `->ascii7`
Converts a data to a 7-bit ASCII encoded string.
***

### `->ascii`
Converts a data to an 8-bit ASCII encoded string.
***

### `->base64`
Converts a data to a base 64 encoded string.
***

### `->md5`
Returns the md5 hash of a data. 
The hash is provided as a data.
***

### `->sha1`
Returns the sha1 hash of a data.
The hash is provided as a data.
***

### `shift`
Performs a bit shift on a number or binary object. 
The shift is passed as parameter. 
A positive shift shifts bits to the right, negative to the left.

```
500 2 shift
# Pushes 125 onto the stack

BIN:01101111 2 shift
# Pushes BIN:00011011 onto the stack

BIN:01101111 -2 shift
# Pushes BIN:10111100 onto the stack
```
***

### `~`
Inverts each bit of a number passed as parameter.
***

### `&`
Binary AND between 2 numbers passed as parameters.
***

### `|`
Binary OR between 2 numbers passed as parameters.
***

### `^`
Binary XOR between 2 numbers passed as parameters.
***

### `reverse`
Reverses the order of elements in a list, data, binary object or string.

```
DATA:A0B5 reverse
# Pushes DATA:B5A0 onto the stack

(1 2 3) reverse
# Pushes (3 2 1) onto the stack

"ABC" reverse
# Pushes "CBA" onto the stack

BIN:000111 reverse
# Pushes BIN:111000 onto the stack
```
***

### `up`
Sets a particular bit of a binary object.

```
BIN:110001 2 up
# Pushes BIN:110101 onto the stack
```
***

### `down`
Clears a particular bit of a binary object.

```
BIN:110101 2 down
# Pushes BIN:110001 onto the stack
```
***

### `classes`
Returns the list of all defined user classes.
***

### `defClass`
Allows defining a class.
***
 
### `ALLOC`
100% RPN function for the syntactic sugar `alloc … to …`.

```
'MyClass' '$MyVariable' ALLOC

# Creates a new instance of class 'MyClass'
# and stores its reference in variable '$MyVariable'
```
***

### `props`
Returns the list of properties of a class instance passed as parameter.
***

### `?instances`
Lists all declared instances (with `alloc` or `ALLOC`) with their usage counter. 
When the counter reaches zero, the instance is automatically deleted. 
> This function is used during debugging of your program.
***

### `sin`
Returns the sine of an angle passed as parameter. The angle is in radians.
***

### `cos`
Returns the cosine of an angle passed as parameter. The angle is in radians.
***

### `tan`
Returns the tangent of an angle passed as parameter. The angle is in radians.
***

### `asin`
Returns the angle in radians whose sine is passed as parameter.
***

### `acos`
Returns the angle in radians whose cosine is passed as parameter.
***

### `atan`
Returns the angle in radians whose tangent is passed as parameter.

### `PI`
Returns the number PI.
***

### `->deg`
Returns the angle in degrees of an angle in radians passed as parameter.

```
PI 3 / ->deg
# Pushes 60 onto the stack
```
***

### `->rad`
Returns the angle in radians of an angle in degrees passed as parameter.

```
60 ->rad
# Pushes 1.0471975511965976 onto the stack
```
***

### `abs`
Returns the absolute value of the number passed as parameter.
***

### `sqrt`
Returns the square root of the number passed as parameter.
***

### `floor`
Returns the largest integral value less than or equal to the number passed as parameter.
***

### `pow`
Returns a number passed as parameter raised to the power passed as parameter.

```
50 3 pow
# Pushes 125000 onto the stack
```
***

### `mod`
Returns the remainder of integer division of one number by another.

```
65 3 mod ?
# Pushes 2 onto the stack
```
***

### `min`
Returns the smallest number present in a list. 
> Only numbers are considered, if other types are present in the list they are ignored.

```
(1 56 34 9 27) min
# Pushes 1 onto the stack
```
***

### `max`
Returns the largest number present in a list. 
> Only numbers are considered, if other types are present in the list they are ignored.

```
(1 56 34 9 27) max
# Pushes 56 onto the stack
```
***

### `sum`
Returns the sum of all numbers present in a list.
> Only numbers are considered, if other types are present in the list they are ignored.

```
(1 56 34 9 27) sum
# Pushes 127 onto the stack
```
***

### `mean`
Returns the average of all numbers present in a list.
> Only numbers are considered, if other types are present in the list they are ignored.

```
(1 56 34 9 27) mean ?
# Pushes 25.4 onto the stack
```
***

### `console.locate`
Requests the runtime host to position the cursor at the coordinates passed as parameter. 
The host is not obligated to respond. 
> MOGWAI CLI handles this function.

```
5 7 console.locate
```
***

### `console.cursor`
Returns the current cursor coordinates on the host screen. 
If the host does not handle this information, coordinates 0 0 are returned. 
> **MOGWAI CLI** handles this function.
***

### `console.foreground`
Requests the host to change the character display color by passing the color name to use as parameter.

Colors defined in **MOGWAI CLI** are:
- `'BLACK'`
- `'WHITE'`
- `'BLUE'`
- `'RED'`
- `'YELLOW'`
- `'GREEN'`

```
'RED' console.foreground
```
***

### `console.background`
Requests the host to change the screen background color.
> In **MOGWAI CLI**, uses the same colors as for `console.foreground`.
***

### `console.inputkey`
Requests the host to provide the code of the currently pressed key.
***

### `http.get`
Performs an http get on a uri by specifying the necessary header values. 

Parameters are passed via a record:

```
[
    uri: "https://api.github.com/orgs/dotnet/repos" 
    requestHeaders: [User-Agent: ".NET Foundation Repository Reporter" token: "XXXXX"]
] http.get
```

The response is a record containing the following keys:

| Key | Usage |
|-----|-------|
| `state:` | `true` if everything went well. |
| `statusCode:` | The status code actually returned (e.g. 200). |
| `response:` | A data containing the response.<br>If an error occurs, this key is not present in the response.|

### `http.post`
Performs an http post on a uri by specifying request headers, content headers and content.

All parameters are defined in a record passed as parameter:

```
[
    uri: "https://api.github.com/orgs/dotnet/repos" 
    requestHeaders: [ ]
    contentHeaders: [ ]
    content: DATA
]
```

The content is of type data.

The response, a record, is formatted exactly like that of the `http.get` function.
***

### `->uri`
Composes a uri from a record whose keys correspond to the different parts of a uri:

```
[
    url: "https://www.google.com" 
    path: "api/v0/login" 
    query: [id: "50" name: "DOE"]
] ->uri 

# Pushes "https://www.google.com:443/api/v0/login?id=50&name=DOE" onto the stack
```
***

### `->urlEncode`
Encodes a URL string passed as parameter. 

This function can be used to encode the entire URL, including query string values. 
URL encoding converts characters that are not allowed in a URL to character-entity equivalents. 
For example, when the characters < and > are embedded in a block of text to be transmitted in a URL, they are encoded as %3c and %3e.
***

### `process.start`
Starts a process. 

Process information is provided via a record composed of the following keys:

| Key | Usage |
|-----|-------|
| `filename:` | File to execute (e.g. notepad.exe)|
| `arguments:` | Arguments to use to start the process.|
| `workingDirectory:` | Sets the current directory for the process.|
| `wait:` | If `true`, waits for the end of process execution before returning.|

> Only the `filename:` key is required.

```
[
    filename: "toto.exe" 
    arguments: "/u -K" 
    workingDirectory: "C:\...." 
    wait: true ] process.start
```
***

## DEBUGGING FUNCTIONS (used with MOGWAI STUDIO)

### `debug.write`
Requests the host and **MOGWAI STUDIO** (if connected) to display a message in the debug console.

```
"Debug message" debug.write
```
***

### `debug.clear`
Requests the host and **MOGWAI STUDIO** (if connected) to clear the debug screen.
***

### 'debug.run'
Starts a program in debug mode. The program name is a string passed as parameter.
***

### `debug.halt` or `¤`
Performs a pause. Corresponds to a breakpoint.

The program must be started in debug mode for the breakpoint to be taken into account. 
When execution reaches this instruction, the runtime pauses.
It is then possible to step through if necessary.

```
1 10 for 'i' do
{
    i ?
    100 wait
    
    # Place a breakpoint here
    debug.halt
}
```
***

### `debug.tron`
Activates tracing. The duration between each instruction is defined as parameter in milliseconds. 
If **MOGWAI STUDIO** is connected, it displays the currently executing instruction in real time.

```
250 debug.tron
```
***

### `debug.troff`
Deactivates tracing.
***

## TIME MANAGEMENT FUNCTIONS

### `now`
Returns the current date of your machine as a number representing the number of 100-nanosecond intervals that have elapsed since midnight, January 1, 0001.

For example, the number 6.389664359647076E+17 corresponds to the date 21/10/2025 at 11:39:56
***

### `->date`
Converts a numeric date to date and time components.

This function returns a record composed of the following keys:

| Key | Usage |
|-----|-------|
| `day:` | Day.|
| `month:` | Month.|
| `year:` | Year.|
| `hour:` | Hours.|
| `minute:` | Minutes.|
| `second:` | Seconds.|
| `dayOfYear:` | Day number in the year.|
| `dayOfWeek:` | Day number in the week.<br>(Sunday=0, Monday=1, …, Saturday=6)|

```
now ->date

# If today is 02/10/2025 at 11:51:29
# Pushes [day: 21 month: 10 year: 2025 hour: 11 minute: 51 second: 29 dayOfYear: 294 dayOfWeek: 2] onto the stack
```
***

### `->duration`
Returns a duration as a record composed of the following keys:

| Key | Usage |
|-----|-------|
| `days:` | Number of days elapsed.|
| `hours:` | Number of hours elapsed.|
| `minutes:` | Number of minutes elapsed.|
| `milliseconds:` | Number of milliseconds elapsed.|

Typically, to calculate the time elapsed between 2 moments, you can store the `now` at the start, then at the end subtract the start `now` from the current `now`, then use the `->duration` function to get the time elapsed between these 2 moments.

```
now 2500 wait now - abs ->duration

# For a total duration of 2 seconds and 507 milliseconds
# Pushes [days: 0 hours: 0 minutes: 0 seconds: 2 milliseconds: 507] onto the stack
```
***

### `->day`
Returns the day of the month of a numeric date passed as parameter.
***

### `->month`
Returns the month of a numeric date passed as parameter.
***

### `->year`
Returns the year of a numeric date passed as parameter.
***

### `->hour`
Returns the hour of a numeric date passed as parameter.
***

### `->minute`
Returns the minutes of a numeric date passed as parameter.
***

### `->second`
Returns the seconds of a numeric date passed as parameter.
***

### `->millisecond`
Returns the milliseconds of a numeric date passed as parameter.
***

### `->days`
Returns a duration as a number of days.
***

### `->hours`
Returns a duration as a number of hours.
****

### `->minutes`
Returns a duration as a number of minutes.
***

### `->seconds`
Returns a duration as a number of seconds.
***

### `->milliseconds`
Returns a duration as a number of milliseconds.
***
 
## TASK MANAGEMENT FUNCTIONS

### `defTask`
Allows creating a new task. Creation parameters are provided via a record.
***

### `task.wait`
Starts the child task whose name is passed as parameter and waits for the end of its execution before returning.
***

### `task.run`
Starts the child task whose name is passed as parameter, returning immediately.
***

### `task.isCompleted`
Returns true if the child task whose name is passed as parameter has finished.
***

### `task.isRunning`
Returns true if the child task whose name is passed as parameter is currently running.
***

### `task.purge`
Deletes the task whose name is passed as parameter.
***

### `task.list`
Returns the list of names of all existing child tasks.
***

### `task.setResult`
Allows a child task to store its result. This function can only be used from within a child task's code. The result can be of any type managed by **MOGWAI**.

```
"MyResult" task.setResult
54 task.setResult
```
***

### `task.result`
Returns the result of the child task whose name is passed as parameter. By default, the result has value `null`.
***

### `task.input`
Returns the input value provided to the child task at creation. 
Can be of any type managed by **MOGWAI**. 
> This function can only be used from within a child task's code.
***

### `task.name`
Returns the name of the child task. 
> This function can only be used from within a child task's code.
***

### `task.join`
Waits for all tasks listed as parameter to finish before returning.

```
('T1' 'T2' 'T3') task.join
```
***

### task.send
Sends a value to the child task whose name is passed as parameter. The value can be of any type managed by **MOGWAI**.

```
'T1' "MyValue" task.send

'T1' 5456 task.send
```
***

### `task.publish`
Allows a child task to publish (send) a value to its parent task. 
The published value can be of any type managed by **MOGWAI**.

> This function can only be used from within a child task's code. 

```
"MyValue" task.publish

2345 task.publish
```
***

### `task.flag.isSet`
Returns `true` if a child task's flag is active. 
Pass the child task name and the flag name to test as parameters.
***

### `task.flag.isClear`
Returns `true` if a child task's flag is inactive. 
Pass the child task name and the flag name to test as parameters.
***

### `task.flag.set`
Allows activating a child task's flag. 
Pass the child task name and the flag name to activate as parameters.
***

### `task.flag.clear`
Allows deactivating a child task's flag. 
Pass the child task name and the flag name to deactivate as parameters.
***

## SERIAL PORT MANAGEMENT FUNCTIONS

Under Windows, MacOS and Linux, **MOGWAI** can use serial ports.

### `serial.open`
Opens a serial communication port. 

Parameters are provided in a record:

| Key | Usage |
|-----|-------|
| `name:` | Serial port name (required).|
| `port:` | Communication port to open (required).|
| `speed:` | Communication speed in bauds (required)|
| `parity:` | Parity to use. Possible values:<br>"N" (none)<br>"E" (even)<br>"O" (odd)<br>"M" (mark)<br>"S" (space)<br>Default value: "N".|
| `dataBits:` | Number of data bits. Values between 5 and 8.<br>Default value 1.|
| `stopBits:` | Number of stop bits. Possible values: 0, 1, 1.5 and 2<br>Default value 8.|
| `handshake:` | Flow control. Possible values:<br>"N" (none)<br>"X" (XON/XOFF)<br>"RTS" (rts/cts)<br>"RTSX" (rts/cts + XON/XOFF)<br>Default value "N".|
| `newLine:` | Characters corresponding to a new line.<br>Default value "\r\n"|
| `dtr:` | Data Terminal Ready<br>Possible values true/false.<br>Default value true.|
| `encoding:` | String encoding format.<br>Possible values: "ASCII", "UTF8"<br>Default value "ASCII".|
| `readBufferSize:` | Input buffer size.<br>Default value 1024.|
| `writeBuffersize:` | Output buffer size.<br>Default value 1024.|
| `readTimeout:` | Duration in milliseconds before a read timeout.|
***

### `serial.close`
Closes the serial port whose name is passed as parameter.
***

### ``serial.isOpen``
Returns true if the serial port whose name is passed as parameter is open.
***

### `serial.read`
Reads n bytes from the serial port whose name is passed as parameter. 
Returns a data composed of the read values.

```
'P1' 50 serial.read

# Will read 50 bytes from 'P1' and push the corresponding DATA onto the stack.
```
***

### `serial.readLine`
Reads a line from the serial port whose name is passed as parameter. 
The function returns the read string (the line).

```
'P1' serial.readLine
# Pushes the DATA corresponding to the read information onto the stack.
```
***

### `serial.write`
Writes the content of a DATA to the serial port whose name is passed as parameter.

```
'P1' DATA:FF45AB23DE serial.write
```
***

### `serial.writeLine`
Writes a string to the serial port whose name is passed as parameter. 
The string will be automatically completed with the characters defined by `newLine:` when opening the port.

```
'P1' "Hello!" serial.writeLine
```
***

### `serial.available`
Returns the number of bytes waiting in the input buffer of the serial port whose name is passed as parameter.
***

### `serial.clearInputBuffer`
Clears the input buffer of the serial port whose name is passed as parameter.
***

### `serial.ports`
Returns the list of serial ports known to the system.

```
serial.ports
# Pushes the known ports of the machine onto the stack. E.g. ("COM1" "COM7" "COM12")
```
***

### `serial.info`
Returns information about the serial port whose name is passed as parameter. 
Information is returned as a RECORD composed of the same keys as those used to open the serial port (serial.open function).
> The `available:` key is added with the number of bytes waiting in the input buffer.
***

### `serial.list`
Returns the list of open serial ports.
***

### `serial.waitAnswers`
Starts a read on a serial port by indicating the possible expected responses. 
The expected response format is the same as for the `like` function. 

Parameters are passed via a RECORD with the following keys:

| Key | Usage |
|-----|-------|
| `port:` | Port name to use.|
| `patterns:` | List of possible expected responses.|
| `timeout:` | Read timeout. Default value 1000.|
| `sanitize:` | true if you want MOGWAI to clean responses before processing them.<br>Default value true.|

If you expect for example as possible responses (multiple consecutive responses):

```
AT+BLE=1
AT+TEST=OK
OK

```
You can use as pattern:

```
("AT+BLE=1" "AT+TEST=OK" "OK")
```

If possible responses can vary slightly, you can take them into account.

If for example you can have 
```
AT+BLE=1
AT+TEST=OK
OK
```

But also sometimes:

```
AT+BLE=0
AT+TEST=NO
OK
```

And you want to handle both cases at once, you can use the following pattern:

```
("AT+BLE=1|AT+BLE=0" "AT+TEST=OK|AT+TEST=NO" "OK")
```

You can also use wildcards (see `like` function):

```
("AT+BLE=#" "AT+TEST=??" "OK")
```

The function returns all received responses as a list of strings.
 
## EVENT MANAGEMENT FUNCTIONS

### `EVENT`
This is the 100% RPN version of the syntactic sugar `event … do` function.

To respond to an event you must indicate the event name and the code to execute when triggered.

```
'MyEvent' « "Hello!" ? » EVENT

# You can also write the syntactic sugar version:

event 'MyEvent' do « "Hello!" ? »
````
***

### `event.purge`
Removes handling of an event whose name is passed as parameter.
***

### `event.list`
Returns the list of all declared events being handled.
***

### `event.fire`
Triggers an event towards the runtime. 
Pass as parameters the event name, an object that accompanies the event and will be retrieved via the local variable `eventData` in the event code.

```
'MyEvent' "Hello" event.fire
```
***

### `event.host.fire`
Triggers an event towards the host. 
Pass as parameters the event name, an object that the host will retrieve (`null` if nothing to send).

```
'MyEvent' "Hello" event.host.fire
```
***

## FILE MANAGEMENT FUNCTIONS

### `file.pack`
Serializes an object passed as parameter and stores it in a file.  
Pass the object to serialize and the filename to use as parameters.

```
(1 2 3 4) "MyFile" file.pack
# Will serialize the list into the file "MyFile".
```
***

### `file.unpack`
Deserializes an object from a file. 
Pass the filename containing the object as parameter. 

Based on the `file.pack` function example:

```
"MyFile" file.unpack will return the list (1 2 3 4)
```
***

### `file.read`
Reads the entire content of a file and returns it as a data. 
Pass the filename to read as parameter.
***

### `file.write`
Writes the content of a DATA to a file whose name is passed as parameter.

```
DATA:FF45ABEA23 "MyFile" file.write
# will write bytes 0xFF, 0x45, 0xAB, 0xEA and 0x23 to the file.
```
***

### `file.purge`
Deletes a file whose name is passed as parameter.
***

### `dir.directories`
Returns the list of all folders present in the current folder.
***

### `dir.files`
Returns the list of all files contained in the current folder.
***

### `dir.programs`
Returns the list of all programs (files with .mog extension) contained in the current folder.
***

### `dir.path`
Returns the current path as a list of folders. 
The path is broken down into as many elements as there are folders. 

```
The path "HOME\Folder1\Folder2" will be returned as ("HOME" "Folder1" "Folder2")
```
***

### `dir.make`
Allows creating a new folder in the current folder.
Pass the new folder name as parameter.
***

### `dir.purge`
Deletes the folder whose name is passed as parameter. 
The folder is in the current folder.
***

### `dir.home`
Goes back to the HOME folder.
***

### `dir.enter`
Enters a folder present in the current folder by passing its name as parameter.

```
"TheFolder" dir.enter
```
***

### `dir.rename`
Renames a folder (from the current folder) by passing the current name and new name as parameters.

```
"OldName" "NewName" dir.rename
```
***
 
## DATABASE MANAGEMENT FUNCTIONS

### `db.create`
Creates a new SQLite file in the current folder that will contain all database tables. 
Pass the filename as parameter.

```
"MyDatabase.db" db.create
```
***

### `db.open`
Opens a database. 
Pass the filename as parameter. 

This function returns a unique identifier in the form of a name that must be stored to then access functions for this database.

```
"MyDatabase.db" db.open -> '$DB' 
# In this example, $DB could contain a name like: 'DBC:CB3D468A010F423891CCBF62D477CB3A'
```
***

### `db.close`
Closes a database, pass its identifier as parameter.
***

### `db.execute`
Executes a SQL command query (for example a table creation command) to the database whose name is passed as parameter. 
You can pass parameters as a RECORD.

> Parameters can only be of type `.number`, `.string` or `.data`

```
$DB "Create Table..." db.execute

$DB "Create Table..." [p1: 50 p2: "TEST"] db.execute
```
***

### `db.executeScalar`
Executes a scalar query. 
It has the same format as db.execute. 
It pushes its result onto the stack.
***

### `db.query`
Executes a query on the database whose name is passed as parameter.

```
$DB "SELECT * FROM MyTable" db.query
# Pushes a list of records onto the stack. Each record is composed of the table fields
```
***

### `db.purge`
Deletes a database whose filename is passed as parameter.
***

### `db.list`
Returns the list of all open databases.
***

### `db.transaction.begin`
Starts a transaction.
***

### `db.transaction.commit`
Commits a transaction.
***

### `db.transaction.rollback`
Rolls back a transaction.
***

### `db.info`
Returns information as a record about the database whose name is passed as parameter.

The record has the following keys:

| Key | Usage |
|-----|-------|
| `name:` | The database name.|
| `database:` | The file containing the database.|
| `transaction:` | `true` / `false`|
***

## TIMER MANAGEMENT FUNCTIONS

### `EVERY`
This is the 100% RPN version of the syntactic sugar `timer … every … do` function.

Creates an `every` type timer.

Parameters are:
- The timer name.
- The repeat interval (in milliseconds).
- The code to execute at each iteration.

```
'MyTimer' 1000 « "Hello!" ? » EVERY
```
***

### `AFTER`
This is the 100% RPN function of the syntactic sugar version `timer … after … do`.

Creates an `after` type timer. Parameters are the same as for the `EVERY` function.

```
'MyTimer' 1000 « "Hello!" ? » AFTER
```
***

### `timer.start`
Starts the timer whose name is passed as parameter.
***

### `timer.stop`
Stops the timer whose name is passed as parameter.
***

### `timer.purge`
Deletes the timer whose name is passed as parameter.
***

#### `timer.state`
Returns `true` if the timer is running.
***

### `timer.list`
Returns the list of all declared timers regardless of their status (running or stopped).
***

### `DI`
Suspends triggering of all timers and events. 
> Warning, they are put on hold and will be executed when interrupts are enabled again.
***

### `EI`
Allows triggering of timers and events.
***

