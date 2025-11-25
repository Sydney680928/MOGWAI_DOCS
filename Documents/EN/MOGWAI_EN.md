# MOGWAI BASICS

## Table des matières

[INTRODUCTION](#introduction)<br>
[GETTING OFF TO A GOOD START](#getting-off-to-a-good-start)<br>
[DISPLAYING VALUES](#displaying-values)<br>
[SCREEN INPUT](#screen-input)<br>
[VARIABLES](#variables)<br>
[CONSTANTS](#constants)<br>
[TYPES](#types)<br>
[THE STACK](#the-stack)<br>
[TESTS](#tests)<br>
[LOOPS](#loops)<br>
[MATHEMATICAL FUNCTIONS](#mathematical-functions)<br>
[STRINGS](#strings)<br>
[CONVERSION FUNCTIONS](#conversion-functions)<br>
[LISTS](#lists)<br>
[RECORDS](#records)<br>
[BYTE ARRAYS](#byte-arrays)<br>
[BINARY NUMBERS](#binary-numbers)<br>
[TIME MANAGEMENT](#time-management)<br>
[FUNCTION DECLARATION](#function-declaration)<br>
[ERROR HANDLING](#error-handling)<br>
[MAKING A PAUSE](#making-a-pause)<br>
[EXITING A FUNCTION, A LOOP OR THE PROGRAM](#exiting-a-function-a-loop-or-the-program)<br>
[AUTOMATIC VARIABLE CREATION](#automatic-variable-creation)<br>
[OBJECT EVALUATION](#object-evaluation)<br>
[FLAGS](#flags)<br>
[FILE MANAGEMENT](#file-management)<br>
[TIMERS](#timers)<br>
[EVENTS](#events)<br>
[SKILLS](#skills)<br>
[CLASSES](#classes)

---

# INTRODUCTION

I have been developing for a very long time and have had the opportunity to use many different technologies and very varied languages, but I believe the language that "spoke" to me the most is RPL.

RPL stands for Reverse Polish Lisp, and it's the name of a language created by HP for its scientific and financial calculators.
HP 48SX calculator programmable in RPL.

RPL is very similar to FORTH. Like FORTH, it uses a stack to take parameters and store results, and like FORTH, it uses Reverse Polish Notation (RPN, not to be confused with the RPL language, of course).

Thus in RPN we don't write `2+2` to perform an addition, but `2 2 +`, it's a bit confusing at first, but with this notation there's no need for parentheses or local variables (in theory).

Of course, entering RPL programs was done directly on the machine, and the ergonomics were not ideal, but HP had implemented a whole battery of tricks and functions to make the exercise bearable.

At first glance, RPL doesn't have a very simple syntax, but by digging into the subject we quickly realize the power it can unleash.
 
## An opportunity to seize
Actually, the idea of launching the development of **MOGWAI** came the day we needed, at work, to be able to simulate a Bluetooth Low Energy peripheral.

When developing a mobile application (that's my job) that uses Bluetooth Low Energy communication to communicate with a given device, the device in question doesn't exist yet because it must first be physically designed and its internal software must then be written, tested and validated.

This whole procedure takes time and generally, to avoid losing too much time, we start developing the mobile application well before the electronic board is able to exchange any information. Keeping the BLE communication part "for the end" is not a good idea.

Indeed, for ideal integration and so that as many people as possible can use the application with their eyes closed (communication included), the BLE dimension must be integrated from the beginning.

So we developed a tool that allows us to simulate the operation of a BLE-communicating device even before it exists. This allows us to validate the exchanges to be implemented well in advance and also allows us to realize very early on all the little things that had not been properly planned.

It is therefore a very important tool for obtaining a robust mobile application in terms of BLE communication. Moreover, it allows the electronic and embedded part to validate very early on crucial choices in terms of communication via Bluetooth Low Energy.

With this type of engine, the "deep" functions of the simulator remain very generic by performing all the necessary operations under the direction of code that is modifiable at will, in real time, without recompilation, because it is the execution engine code that will take care of the entire "logic" part of the simulation. It will be enough to store for each peripheral a set of scripts adapted to its operating mode and adapted to the tests to be carried out. The flexibility obtained was enormous!

The simulator must be able to execute very varied instructions. It must be able to generate the structure of the BLE peripheral to simulate, and also perform tasks that will make it react as if it were the real peripheral. For this, you ideally need to be able to "program" the simulator. And it's for this basic use that **MOGWAI** was developed. It's an execution engine that can be included in an application that needs to be "motorized".

The BLE simulator was the ideal project to launch the development of **MOGWAI**.

## A slow maturation

The first version of **MOGWAI** was developed in .NET Standard with the C# language. The **MOGWAI** library was included in the simulator that was developed in UWP. Since the simulator had to take the role of a BLE peripheral, a machine equipped with a BLE chip capable of supporting this role was needed (generally BLE chips in desktop PCs only know how to support the Central BLE role). Raspberry PI 3s are equipped with a BLE chip capable of taking both roles. By installing Windows 10 IOT on a Raspberry PI 3, we were able to run the first version of the simulator without any problem, motorized by the first version of **MOGWAI**. This tool saved us a lot of time at the time.

As the BLE simulator's needs grew, the **MOGWAI** engine was extended, improved, and many new features were added. Today **MOGWAI** can handle serial connections, HTTP requests, SQLite databases and has more than 200 primitives.

I'm now at version 6, still developed in C# for .NET. This allows it to be used on Windows, but also on Linux and Mac OSX with X86, X64 and ARM architectures. For example, **MOGWAI** runs natively on a Raspberry PI 3 under Raspbian (Linux ARM).

## MOGWAI CLI to use the language in interactive mode

To "play" with **MOGWAI** I developed an interactive console application that allows you to use all the features of the language. This application is called [**MOGWAI CLI**](https://github.com/Sydney680928/MOGWAI_CLI).

It is quite possible to write **MOGWAI** programs with a simple notepad, but it is still more pleasant to have appropriate development tools. [**MOGWAI Studio**](https://studio.mogwai.eu.com) is an IDE dedicated to **MOGWAI**.

# GETTING OFF TO A GOOD START

There is a reflex to adopt with **MOGWAI**, which is to place the `mogwai.reset` function as the first instruction of your programs.

It ensures you have an absolutely clean execution engine, no variables, no timers, no tasks, nothing at all.

For example, the **MOGWAI CLI** application that allows you to "play" with **MOGWAI** never resets the execution context, which means that everything you create as you type lines is kept, which allows you to chain commands to perform operations step by step to test.

So don't forget, to reset everything, use the `mogwai.reset` function.
 
# DISPLAYING VALUES

There are mainly 2 functions for displaying values on the screen.

`println` displays the object in position 1 on the stack and automatically performs a line break. To gain conciseness it is possible to use `?` instead.

`print` performs the same operation without automatic line break. This function can be replaced by `??`.

```
# We display the value 15 and the string "HELLO !" on 2 lines
15 ?
"HELLO !" ?

# We display the message "IT IS 2025" in 2 parts, a string and a number.
"IT IS " ??
2025 ?
```

To clear the screen, you must use the `cls` function.

# SCREEN INPUT

To enter data on the screen there are 2 functions, `input` and `prompt`.

The simplest is `input` which waits for keyboard input ending with a carriage return (`ENTER` key). The entered information is placed on the stack as a character string.

```
# We switch to input mode and store the result in the variable $X

input -> '$X'
```

The `prompt` function works exactly like input but it also allows you to display a prompt message. This message is placed on the stack before calling the `prompt` function.

```
# We ask for the name input 
# And we store the information in the variable '$NOM"

"Quel est votre nom ? " prompt -> '$NOM'
```

# VARIABLES

Variables are defined by a name. If the name starts with the `$` symbol, it will be global, in all other cases, it will be local.

By default, a variable does not need to be declared to be used. The first assignment creates it if it doesn't already exist.

By default a variable has no predefined type, it takes the type of the last value assigned to it but it is possible to lock the type of a variable if a declaration is made prior to its use. It then takes the declared type and an error is raised if you try to assign it a value of another type.

Typed variables are declared with the def function.

```
# We assign the numeric value 50 to the local variable 'A'
50 -> 'A'

# We then assign a character string to this variable
"Hello !" -> 'A'

# We assign the numeric value 500 to the global variable '$R'
500 -> '$R'

# We declare the global variable '$Z' with the type .number 
# Which will allow it to store numbers
.number '$Z' def

# We can now only store numbers in '$Z'
1500 -> '$Z'

# Otherwise an error is raised
"Hello !" -> '$Z'

# If we want to declare a variable that accepts storing any type 
# Of values, we must use the type .any
.any '$X' def
1500 -> '$X'
"Hello !" -> '$X'
```

It is possible to make prior declaration of variables mandatory before using them. Simply use the `mogwai.explicit` function with `true` or `false` to enable or disable this requirement.

```
# We activate the mandatory declaration of variables before using them 
true mogwai.explicit
```

When a variable no longer needs to exist, it is possible to explicitly delete it using the `purge` function.

A local variable will be automatically deleted anyway when the code exits its scope.

If you try to delete a variable that doesn't exist, an error is raised.

```
# We delete the local variable A 
'A' purge
```

To place the value of a variable on the stack, simply invoke its name without apostrophes.

```
# We assign 'A' and 'B' with numbers.
20 -> 'A'
30 -> 'B'

# We perform the sum of the 2 variables and store the result in the variable 'C'
A B + -> 'C'
```

With the `rcl` function, it is possible to place the value of a variable on the stack using its name.

```
# We retrieve the value of a variable via its name (with apostrophes).
100 -> 'A'
'A' rcl

# 100 is placed on the stack.
```

To store in a numeric variable the result of a mathematical operation on itself (like adding 1 to the value of variable X), there are 4 additional assignment functions:

`->+` Adds a number to a variable.

```
100 -> 'A'
10 ->+ 'A' 
# Now A equals 110.
```

`->-` Subtracts a number from a variable.

```
100 -> 'A'
10 ->- 'A' 
# Now A equals 90.
```

`->*` Multiplies a number and a variable.

```
100 -> 'A'
10 ->* 'A' 
# Now A equals 1000.
```

`->/` Divides a number and a variable.

```
100 -> 'A'
10 ->/ 'A' 
# Now A equals 10.
```

If the variable doesn't exist, it is created with the default value 0, the operation will then be performed from this value.

If the variable is not numeric, it is initialized as if it didn't exist before.

If the variable is not of numeric type and has been declared (locked type), an error is raised.

To save time there are also 2 functions to increment and decrement a numeric variable.

`++` Increments a variable.

```
100 -> 'A'
'A' ++
# Now A equals 101.
```

`--` Decrements a variable.

```
100 -> 'A'
'A' --
# Now A equals 99.
```

The `-->` function allows you to apply processing to the value of a variable and store the result in that same variable:

```
# $A contains the value 100
# We want to calculate the square root multiplied by 5 of $A and store the result in $A

# Classic method

100 -> '$A'
$A sqrt 5 * -> '$A'

# Faster writing using the function -->
100 -> '$A'
{sqrt 5 *} --> '$A'
```

The vars function returns the list of all global variables used:

```
# We create 3 global variables $A, $B and $C

50 -> '$A'
100 -> '$B'
$A $B + -> '$C'

# We list the global variables used
vars

# Places the list ('$A' '$B' '$C') on the stack
```

The lvars function returns the list of all local variables used:

```
# We create 3 local variables A, B and C

50 -> 'A'
100 -> 'B'
A B + -> 'C'

# We list the local variables used

lvars

# Places the list ('A' 'B' 'C') on the stack
```

It is possible to check the existence of a variable with the `exists` function.

This function returns `true` if the variable name passed as parameter exists (local or global variable).

```
# We create 1 local variable A

50 -> 'A'

'A' exists

# Places true on the stack
```

# CONSTANTS

It is possible to declare constants. These are variables whose content can no longer be modified after their first assignment.

Constants are declared with the `const` function.

```
# We define a numeric constant
100 '$BUFFER_SIZE' const

# We define a constant of character string type
"XYZ45FGHH" '$USER_PASSWORD' const

# We create a new character string from $USER_PASSWORD
# Which we store in the variable 'pwd'
$USER_PASSWORD ".X1" + -> 'pwd'
```

# TYPES

**MOGWAI** manipulates objects with different types.

Each type has a name that starts with a dot. For example, the type corresponding to a character string is named `.string`.

The `->type` function allows you to retrieve the type of the object on the stack.

```
# The type of a number is .number
1567 ->type ?

# We can test the type of a variable and make decisions accordingly
234 -> 'A'
if (A ->type .number ==) then {"A is a number" ?} else {"A is not a number" ?}
```

The main types manipulated by **MOGWAI** are as follows:

| Name | Type | Example |
|------|------|---------|
| `.number` | Number (double precision real) | 154 or -56.34 |
| `.string` | Character string | "Hello world" |
| `.boolean` | Boolean value | true / false |
| `.list` | List of objects | (5 "X1" 12.78) |
| `.block` | Code block | {2 2 + ?} |
| `.program` | Function | «2 2 + ?» |
| `.name` | Symbolic name | 'A' |
| `.key` | Key used in a RECORD | latitude: |
| `.data` | Byte array | DATA:FF3456ED23 |
| `.binary` | Binary number | BIN:110011110011 |
| `.record` | RECORD (dictionary) | [x: 50 y: 200] |
| `.null` | Null value | null -> 'A' |
| `.any` | Free type (variant) | |

 
**MOGWAI** offers 9 functions that make it easier to test the most commonly used types:

| Function | Test | Result |
|----------|------|--------|
| `isnull` | 154 isnull | false |
| `isRecord` | [x: 50 y: 20] isRecord | true |
| `isList` | (1 2 3) isList | true |
| `isName` | 'A' isName | true |
| `isString` | "Hello world" isString | true |
| `isKey` | X: isKey | true |
| `isData` | DATA:50FF4E isData | true |
| `isNumber` | 564 isNumber | true |
| `isBoolean` | false isBoolean | true |

# THE STACK

**MOGWAI** is a language that uses a LIFO stack to provide parameters to functions and retrieve results. 
You can place any object manipulated by **MOGWAI** on the stack (see the TYPES chapter).

For example, when you write `2 8 +` to perform an addition, **MOGWAI** will perform a series of operations during execution:

1. Place 2 on the stack (2 is in position 1).
2. Place 8 on the stack (8 is in position 1, and 2 in position 2).
3. Execute the `+` function which will take the 2 values at the top of the stack, add them and place the result on the stack.

In the end on the stack, 2 and 8 have disappeared (we say they were consumed by the `+` function), replaced by the result of their sum (the value 10).

## Variable assignment

When you assign a value to a variable, it's exactly the same process that happens.

For example, when you write `500 -> 'A'` you use a simplified version for assignment (it's more convenient to write it this way), but in reality the **MOGWAI** parser transforms these instructions into 
`500 'A' sto`.

The `sto` function is the assignment function that takes 2 parameters from the stack, the value and the variable name.

1.	500 is placed on the stack.
2.	'A' is placed on the stack.
3.	The `sto` function takes the value and the name and assigns it the provided value.

**MOGWAI** saves you from writing certain functions 100% in RPN because it's not a simple exercise.

Here is the list of assignment functions with a simplified version:


| Simplified<br>Function | 100% RPN<br>Function | Simplified<br>Usage | 100% RPN<br>Usage |
|--------------------|-------------------|----------------|----------------|
| `->`                 | `sto`               | `10 -> 'A'`    | `10 'A' sto`   |
| `->+`                | `sto+`              | `10 ->+ 'A'`   | `10 'A' sto`   |
| `->-`                | `sto-`              | `10 ->- 'A'`   | `10 'A' sto-`  |
| `->*`                | `sto*`              | `10 ->* 'A'`   | `10 'A' sto*`  |
| `->/`                | `sto/`              | `10 ->/ 'A'`   | `10 'A' sto/`  |


## Tests with `if`

The code that performs an `if` test is actually transformed by the **MOGWAI** parser into a 100% RPN call.

For example, for the following test:

`if (A 100 ==) then {"A has a value of 100" ?} else {"A does not have a value of 100" ?}`


**MOGWAI** will transform this line into:

`{A 100 ==} eval {"A has a value of 100" ?} {"A does not have a value of 100" ?} IFELSE`

Which in practice translates into this series of operations:

1.	`{ A 100 == }` is evaluated to place the boolean result of the test to be performed on the stack.
2.	`{ "A has a value of 100" ? }` is placed on the stack.
3.	`{ "A does not have a value of 100" ? }` is placed on the stack.
4.	The `IFELSE` function takes the boolean value, if it's true it evaluates the 1st code block, otherwise it evaluates the second code block.

Admit that the non-RPN version is nicer to write and understand.

## Stack manipulation functions

The stack can be manipulated because in certain cases it's very practical. This often avoids using intermediate local variables. In the end the code is faster.

For example, if you want to perform a calculation, display the result, then perform another calculation from this result and display it too, theoretically you need an intermediate variable:

```
# We do a 1st calculation and display it.
# But we must keep a trace of the result for another calculation later.

# We do the 1st calculation and store the result in A.
2 7 + -> 'A' 

# We display the result of the 1st calculation.
A ?

# We do the second calculation from the result of the previous calculation which we display immediately.
A 200 * ?
```

By manipulating the stack we can avoid the intermediate variable and make the code more compact and faster. For this we will use the `dup` function which duplicates the 1st element of the stack:

```
# We do the 1st calculation and duplicate the result to display it.
# Then we do the second calculation from the result of the previous calculation which we display.

2 7 + dup ?
200 * ?
```

## Available stack functions

| Function | Action                                                                 |
|:--------:|------------------------------------------------------------------------|
| `dup`    | Duplicates the 1st element of the stack.                                    |
| `swap`   | Swaps the 1st and 2nd element of the stack.                      |
| `clear`  | Empties the stack.                                                          |
| `depth`  | Places the stack size at the time of the request on the stack.         |
| `drop`   | Removes the 1st element from the stack.                                    |
| `pop`    | Extracts the 1st element from the stack and stores it for the `push` function |
| `push`   | Places on the stack the object stored by the `pop` function                  |

 
## The `sign` function

It is possible to determine the type of stack elements without removing the elements from the stack. The `sign` function which takes as parameter the number of elements to inspect returns a list containing the types of the inspected elements.

```
# We place 3 values of different types on the stack

10 "EE" (1 2)

# We inspect these 3 values

3 sign

# sign places the list (.list .string .number) on the stack
# Which correspond to the types of the elements present on the stack
# In position zero in the list the type of the last element placed on the stack
```

If we try to inspect more elements than are actually present on the stack, the `sign` function returns the `null` value.

The `sign` function is very useful to verify, without modifying the stack, that the parameters present are indeed of the expected type.

# TESTS

## The `if` instruction

`if` allows you to perform tests and make decisions.

When the test is positive, a code block is executed. It is also possible to define a code block to execute when the test is negative.

```
50 -> 'A'

if (A 50 ==) then 
{
    "A has a value of 50" ?
}
else
{
    "A does not have a value of 50" ?
}
```

It is imperative that the test clause (the code placed between parentheses) places a boolean value on the stack. If this is not the case, an error is raised.

```
# This expression will work 
if (true) then {"TRUE !" ?} else {"FALSE !" ?}

# This expression will raise an error
if ("TOTO") then {"TRUE !" ?} else {"FALSE !" ?}
```

## Boolean logical operations (return `true` or `false`)

| Test      | Meaning             |
|-----------|---------------------------|
| `X Y ==`  | X equal to Y?             |
| `X Y !=`  | X different from Y?        |
| `X Y >`   | X greater than Y?         |
| `X Y <`   | X less than Y?         |
| `X Y >=`  | X greater than or equal to Y? |
| `X Y <=`  | X less than or equal to Y? |
| `X not`   | Logical inversion of X    |
| `X Y or`  | X OR Y                    |
| `X Y and` | X AND Y                    |
| `X Y xor` | EXCLUSIVE OR between X and Y  |

 
## Binary logical operations (return a number)


| Test      | Meaning             |
|-----------|---------------------------|
| `X Y &`   | Binary AND                |
| `X Y |`   | Binary OR                |
| `X Y ^`   | Binary EXCLUSIVE OR       |
| `X Y ~`   | Binary NOT               |


## The `switch` instruction

To avoid cascading `if .. else` you can use the `switch` instruction.

This instruction is composed of several test / code block pairs.

At the 1st test encountered that returns `true`, its code block is executed and only that one.

```
# We want to display a message according to the value of the variable 'a'

150 -> 'a'

switch 
{
    (a 100 <) 
    { 
        "100" ?
    }
	
    (a 200 <)
    { 
        "< 200" ?
    }
	
    (true)
    { 
        "DEFAULT" ?
    }
}
```

If you absolutely want to have a code block that executes even if no other is selected (a sort of default block), simply put a block at the end whose test cannot fail (ideally we put `true` directly in the test).

# LOOPS

## `repeat` loop

To execute a code block a certain number of times, you must use `repeat`.

```
# We will display the numbers from 1 to 10
# The variable 'I' serves as the loop counter

0 -> 'I'

10 repeat
{
    'I' ++
    I ?
}

# We will display the numbers from 1 to 10
# The variable 'I' serves as the loop counter
# We exit the loop when 'I' equals 5

0 -> 'I'

10 repeat
{
    'I' ++
    I ?

    if (I 5 ==) then {break}
}
```

## `during` loop

To execute a code block for a certain duration, you must use `during`.

The duration is expressed in milliseconds (1000 = 1 second).

```
# We will execute the code for 10 seconds

0 -> 'I'

during 10000 do 
{
    'I' ++
    I ?
}
```

## `for` loop

To use an automatically managed loop counter, you must use `for`.

```
# We will display the numbers from 1 to 10
# The variable 'I' serves as the loop counter


1 10 for 'I' do
{
    I ?
}

# We will display the numbers from 10 to 1
# The variable 'I' serves as the loop counter


10 1 for 'I' step -1 do
{
    I ?
}

# We will display the numbers from 10 to 1
# The variable 'I' serves as the loop counter
# When we reach the value 5 we exit the loop


10 1 for 'I' step -1 do
{
    I ?

    if (I 5 ==) then {break}
}
```

## `foreach` loop

To iterate each element of a list, you must use `foreach`.

```
# We display each element of the list

("L1" "L2" "L3" "L4" "L5" "L6" "L7") foreach 'item' do {item ?} 
```
 
## `forever` loop

To execute a loop indefinitely, you must use `forever`.

```
# We execute the following code indefinitely

0 -> 'I'

forever do {'I' ++ ?}

# We execute the following code indefinitely
# But we exit when 'I' has the value 456

0 -> 'I'

forever do 
{
    'I' ++
    I ?

    if (I 456 ==) then {break}
}
```

## `while` loop

To execute a code block as long as a condition is true, you must use `while`.

With this notation (while at the beginning of the loop), the test is performed first:

```
# As long as I is less than 100 we display it

0 -> 'I'

while (I 100 <) do
{
    'I' ++
    I ?
}
```

## `do… while` loop

To execute a code block as long as a condition is true, you must use `do … while`.

With this notation, the loop code is executed and the test is performed at the end:

```
# As long as I is less than 100 we display it

0 -> 'I'

do
{
    'I' ++
    I ?
} while (I 100 <)
```
 
# MATHEMATICAL FUNCTIONS

| Function | Usage                                                                                                                                                         | Example        |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|
| `->deg`  | Converts a radian angle to degrees.                                                                                                                            | `0.05 ->deg`   |
| `->rad`  | Converts a degree angle to radians.                                                                                                                            | `3.14 ->rad`   |
| `+`      | Adds 2 numbers.                                                                                                                                             | `5 7 +`        |
| `-`      | Subtracts 2 numbers.                                                                                                                                          | `5 7 -`        |
| `*`      | Multiplies 2 numbers.                                                                                                                                          | `5 7 *`        |
| `/`      | Divides 2 numbers.                                                                                                                                             | `5 7 /`        |
| `abs`    | Returns the absolute value of a number.                                                                                                                       | `-56 abs`      |
| `acos`   | Returns the arc cosine of an angle in radians.                                                                                                                  | `0.5 acos`     |
| `asin`   | Returns the arc sine of an angle in radians.                                                                                                                    | `0.5 asin`     |
| `atan`   | Returns the arc tangent of an angle in radians.                                                                                                                 | `0.5 atan`     |
| `ceil`   | Returns the value of the smallest integer greater than or equal to the specified number.                                                                                 | `56.89 ceil`   |
| `cos`    | Returns the cosine of an angle in radians.                                                                                                                     | `0.5 cos`      |
| `max`    | Returns the maximum value of a list.<br> Only numbers are taken into account.<br> Returns `null` if the list contains no numbers.                  | `(1 2 3) max`  |
| `mean`   | Returns the average of a list.<br> Only numbers are taken into account.<br> Returns 0 if the list contains no numbers.                              | `(1 2 3) mean` |
| `min`    | Returns the minimum value of a list.<br> Only numbers are taken into account.                                                                            | `(1 2 3) min`  |
| `pow`    | Returns a specified number raised to the specified power.                                                                                                   | `100 2 pow`    |
| `rand`   | Generates a random number between 0 and 1.                                                                                                              | `rand ->'A'`   |
| `shift`  | Performs a bit shift on a specified number.<br> If the shift value is positive the shift is performed to the right, otherwise to the left. | `100 4 shift`  |
| `sin`    | Returns the sine of an angle in radians.                                                                                                                       | `0.5 sin`      |
| `sqrt`   | Returns the square root of a number.                                                                                                                        | `16 sqrt`      |
| `sum`    | Returns the sum of a list.<br> Only numbers are taken into account.<br> Returns null if the list contains no numbers.                             | `(1 2 3) sum`  |
| `tan`    | Returns the tangent of an angle in radians.                                                                                                                    | `0.5 tan`      |
| `PI`     | Returns PI in degrees.                                                                                                                                         | `PI`           |
| `floor`  | Returns the largest integral value less than or equal to the specified number.                                                                              | `45.8 floor`   |
| `mod`    | Returns the remainder of the integer division of one number by another.                                                                                            | `100 3 mod`    |

 
# STRINGS

**MOGWAI** has many character string processing functions.

## Concatenation

The `+` function allows you to concatenate 2 character strings.

This function has a certain "intelligence" because depending on the context it knows how to adapt.


| Operation               | Result         |
|-------------------------|------------------|
| `"HELLO " "LE MONDE" +` | "HELLO LE MONDE" |
| `"HELLO" 3 +`           | "HELLO3"         |
| `3 "HELLO" +`           | "3HELLO"         |

## Extraction

There are several functions to extract part of a character string.

| Operation                   | Result        |
|-----------------------------|-----------------|
| `"HELLO LE MONDE" 0 5 sub`  | "HELLO"         |
| `"HELLO LE MONDE" butfirst` | "ELLO LE MONDE" |
| `"HELLO LE MONDE" butlast`  | "HELLO LE MOND" |
| `"HELLO LE MONDE" first`    | "H"             |
| `"HELLO LE MONDE" last`     | "E"             |
| `"HELLO LE MONDE" 3 left`   | "HEL"           |
| `"HELLO LE MONDE" 3 right`  | "NDE"           |

## Size

To retrieve the size of a character string, you must use the `size` function.

```
# We retrieve the size of a character string and display it

"HELLO LE MONDE" size ?
```

## Finding elements

To search for a substring in a character string, you must use the `where` function which returns a list composed of all corresponding positions.

```
# We search for the location of all the letters "E"

"HELLO WORLD" "O" where

# The answer will be the list (4 7)
```

## Transformations

To transform a character string you can use the following functions:

| Operation                  | Result           |
|----------------------------|------------------|
| `"HELLO WORLD" ->lower`    | "hello world"    |
| `"hello world" ->upper`    | "HELLO WORLD"    |
| `("X" "Y" "Z") ";" join`   | "X;Y;Z"          |
| `"X;Y;Z" ";" split`        | ("X" "Y" "Z")    |
| `"HELLO WORLD" reverse`    | "DLROW OLLEH" |

## Formatting a number

It is possible to format a number using the `->format` function which takes as parameters the number to format and the format to apply.

The format to apply is a character string describing what form the number should take:

| Operation                | Result |
|--------------------------|----------|
| `50.678 "0.00" ->format` | "50.68"  |
| `34 "000" ->format`      | "034"    |

```
# We display the current date in dd/mm/yyyy format
# See the DATE MANAGEMENT chapter to fully understand the following code.

now ->date -> 'dt'

dt day: get "00" ->format ??
"/" ??
dt month: get "00" ->format ??
"/" ??
dt year: get "0000" ->format ?
```

## Including values in a string

It is possible to include directly in a string elements from variables or functions. 
It is thus possible to compose a string very easily without having to perform tedious element-by-element construction operations.
To indicate the location of an element to incorporate into a character string, you must use the self-evaluated code block notation.

For example, to incorporate the contents of the `name` variable, simply write:

`"The name is {! name}" eval`

It is the `eval` function that will take the string and replace all incorporated elements with their true value.
If the evaluation of an incorporated element causes an error (non-existent variable, erroneous code), the replacement of this element is not performed.

In our example, if the `name` variable contains `"DOE John"` the evaluation will give:

`"The name is DOE John"`

You can also place code. For example, you can display the name in uppercase:

`"The name is {! name ->upper}" eval`

Which will give: `"Le nom est DOE JOHN"`

It is very important to note the replacement field with the `!` symbol stuck to the opening brace otherwise it will not be recognized as such.

`"The name is { ! name}" eval` will not work and the result will be `"The name is { ! name}"` (no replacement).

If you need to use quotes in your string, you must replace them with `\'`

```
"DOE John" -> 'name'
50 -> 'age'

"{! name} is {! age} years old" eval ?

# This will display "DOE John is 50 years old"
```

# CONVERSION FUNCTIONS

To convert an object to another (for example a character string to a number or vice versa) **MOGWAI** has conversion functions that start with the `->` symbol.

| Operation                    | Result                                      |
|------------------------------|-----------------------------------------------|
| `DATA:4142434445 ->ascii`    | "ABCDE"                                       |
| `DATA:4142434445 ->ascii7`   | "ABCDE"                                       |
| `45 ->str`                   | "45"                                          |
| `"45" ->num`                 | 45                                            |
| `DATA:FF5612AE5678 ->base64` | "/1YSrlZ4"                                    |
| `"/1YSrlZ4" ->base64`        | DATA:FF5612AE5678                             |
| `1968 ->bin`                 | BIN:11110110000                               |
| `(64 65 66) ->data`          | DATA:414243                                   |
| `64 65 66 3 ->data`          | DATA:414243                                   |
| `0.56 ->deg`                 | 32.08563652732611                             |
| `123.67432 "0.00" ->format`  | "123.67"                                      |
| `234 ->hex`                  | "EA"                                          |
| `20 ->i8`                    | DATA:14                                       |
| `20 ->i16`                   | DATA:0014                                     |
| `20 ->i32`                   | DATA:00000014                                 |
| `20 ->i64`                   | DATA:0000000000000014                         |
| `-30 ->u8`                   | DATA:E2                                       |
| `-30 ->u16`                  | DATA:FFE2                                     |
| `-30 ->u32`                  | DATA:FFFFFFE2                                 |
| `-30 ->u64`                  | DATA:FFFFFFFFFFFFFFE2                         |
| `56.9865 ->int`              | 56                                            |
| `"latitude" ->key`           | latitude:                                     |
| `"rand" ->keyword`           | rand                                          |
| `45 56 78 3 ->list`          | (45 56 78 3)                                  |
| `DATA:414243 ->list`         | (65 66 67)                                    |
| `"HELLO LE MONDE" ->lower`   | "hello le monde"                              |
| `"hello le monde" ->upper`   | "HELLO LE MONDE"                              |
| `DATA:2345E323 ->md5`        | DATA:0E9751A0F9AF52C737038B4F2108A907         |
| `"latitude" ->name`          | 'latitude'                                    |
| `(2 3 +) ->program`          | « 2 3 + »                                     |
| `35.3 ->rad`                 | 0.6161012259539983                            |
| `DATA:12ED45FE89 ->sha1`     | DATA:8B1FB372469A9B52DED84498FF26CEE06C07910B |
| `123 ->type`                 | .number                                       |
| `(1 2 3) ->type`             | .list                                         |
| `'latitude' ->type`          | .name                                         |
| `latitude: ->type`           | .key                                          |
| `"latitude" ->type`          | .string                                       |
| `"Hello !" ->utf8`           | DATA:48656C6C6F2021                           |
| `DATA:48656C6C6F2021 ->utf8` | "Hello !"                                     |

# LISTS

**MOGWAI** lists are not typed, they can contain a collection of any objects.

Lists are noted with parentheses. The objects they contain are simply separated by spaces.

For example `(1 2 7)` is a list of numbers, `("X1" "X2" "X3")` is a list of character strings and `("X1" "X2" "X3" 45 67 (1 2 3) true)` is a list of lots of different objects (you will notice that a list can contain lists).

## Creating a list

The simplest method to create a list is to enter it directly (as above).

You can also place on the stack the elements that must compose it, indicate how many to take and use the `->list` function.

```
# We create a list from the objects that are on the stack.

10 20 30 40 50 5 ->list

# This instruction will place the list (10 20 30 40 50) on the stack
```

You can also type the list directly in your code:

```
# We create a list directly in the code

(10 20 30 40 50)

# This instruction will place the list (10 20 30 40 50) on the stack
```

## Adding elements to a list

The + function allows you to add an element to a list.

```
# We add 1 element to a list.

(10 20 30) 40 +

# This instruction will place the list (10 20 30 40) on the stack

# We add a list to a list.

(10 20 30) (100 200) +

# This instruction will place the list (10 20 30 (100 200)) on the stack
```
 
## Retrieving the size (number of elements) of a list

The `size` function returns the size of a list.

```
# We retrieve the size of a list to display it

(10 20 30 40) size ?

# Will display 4
```` 

## Modifying an element of a list

The `set` function allows you to modify a particular element. You must provide its index (from 0 to size-1) and the new value:

```
# We modify the 3rd element of the list (we replace 55 with "Z")

(10 "E" 55 20 30) 2 "Z" set

# This instruction will place the list (10 "E" "Z" 20 30) on the stack
```

## Retrieving an element from a list

The get function allows you to retrieve an element from a list. As with `set`, you must provide its index (from 0 to size-1):

```
# We retrieve the 5th element of the list

(10 20 30 40 50 60 70) 5 get

# This instruction will place the value 60 on the stack
```

If the specified index is not in the possible range (from 0 to size-1), the function returns `null` and does not raise an error.
 
## Retrieving an element "buried" in a list

If a list is composed of sub-lists and/or sub-records (see later the presentation of records which are key/value associations), it may be interesting to give in a single operation the "path" to follow to retrieve the information:

```
# Method 1, basic, we retrieve information in multiple operations
# We will first retrieve the 2nd record, then the value of its key name:

([id: 0 name: "DOE"] [id: 1 name: "SMITH"] [id: 2 name: "BLACK"]) 1 get

# This operation places the record [id: 1 name: "SMITH"] on the stack
# Then we retrieve the value of the key name:

name: get

# Which places "SMITH" on the stack

# Method 2, we retrieve information in a single operation

([id: 0 name: "DOE"] [id: 1 name: "SMITH"] [id: 2 name: "BLACK"]) (1 name:) get

# This operation directly places "SMITH" on the stack
```

If the path leads to nothing (bad path), the returned value will be the null value.

```
# If the path is bad

([id: 0 name: "DOE"] [id: 1 name: "SMITH"] [id: 2 name: "BLACK"]) (5 name:) get

# This operation directly places null on the stack because element 5 of the list
# Does not exist.
```

## Extracting part of a list

The `extract` function allows you to extract only certain elements from a list in a single operation. It takes as parameters the source list and a list of indexes to extract:

```
# We extract elements 1 2 4 from the list

(10 "E" 55 20 30) (1 2 4) extract

# This instruction will place the list ("E" 55 30) on the stack
```

If you request indexes that don't exist (outside the indexes of the source list), values of type `null` will be added in their place.
 

## Retrieving the 1st element of a list

There are 2 ways, the 1st is the one we just saw, using the `get` function with an index of zero.

The second way is to use the `first` function, which does exactly the same thing. If the list is empty, it returns `null`.

```
# We retrieve the 1st element of the list in 2 ways

# With the get function

(10 20 30 40 50 60 70) 0 get

# This instruction will place the value 10 on the stack

# With the first function

(10 20 30 40 50 60 70) first

# This instruction will place the value 10 on the stack
```

## Retrieving the last element of a list

You've probably guessed it, the `last` function returns the last element of a list, and the `null` value if the list is empty.

```
# We retrieve the last element of the list

(10 20 30 40 50 60 70) last

# This instruction will place the value 70 on the stack
```

## Deleting an element from a list

To delete an element from a list, you must use the purge function with the list and the index to delete as parameters. If the index is < 0 an error is raised. If the index is >= size the operation is simply ignored.

```
# We delete the 3rd element of the list, which is the value 40

(10 20 30 40 50 60 70) 3 purge

# This instruction will place (10 20 30 50 60 70) on the stack
```
 
## Extracting elements from a list from a given index

To extract a sub-list, you must use the `sub` function with the starting index and the number of elements to retrieve as parameters. If the starting index is outside the list, an error is raised.

This function returns a list composed of the selected elements.

If you request more elements than possible, the response will be composed of the maximum possible elements.

```
# We retrieve part of a list

(10 20 30 40 50 60 70) 2 3 sub

# This instruction will place (30 40 50) on the stack

# We retrieve part of a list by requesting too many elements

(10 20 30 40 50 60 70) 2 30 sub

# This instruction will place (30 40 50 60 70) on the stack

# We retrieve part of a list starting from an index that is too large

(10 20 30 40 50 60 70) 20 3 sub

# This instruction will raise the error "bad argument value"
```

## Retrieving an entire list except the 1st element or the last element

It is the `butfirst` function that allows you to retrieve an entire list except the 1st element.

The `butlast` function allows you to retrieve an entire list except the last element.

If the list is empty or if it consists of a single element, these functions return an empty list.

```
# We retrieve a list without its 1st element

(10 20 30 40 50 60 70) butfirst

# This instruction will place (20 30 40 50 60 70) on the stack

# We retrieve a list without its last element

(10 20 30 40 50 60 70) butlast

# This instruction will place (10 20 30 40 50 60) on the stack
```

## Converting a list to a byte array (data)

You can create a data object (byte array) from a list.

Only numbers between 0 and 255 will be taken into account and integrated into the final result.

```
# Example 1: We create a data object from a list of bytes expressed in hexadecimal

(0x10 0x20 0x30 0x40) ->data

# This instruction places the data object DATA:10203040 on the stack

# Example 2: We are not obliged to use hexadecimal notation

(100 200 3000 "EEE" 120 10 true) ->data

# This instruction will place the data object DATA:64C8B8780A on the stack
# Elements 3000 "EEE" true have been ignored because they are not bytes.
```

## Finding the location of values

To search for the location of values in a list, you must use the `where` function.

This function returns all locations of a value that is passed to it as a parameter.

```
# We search for the indexes of the value "XX"

(10 20 "XX" "EA" 670 true "XX") "XX" where

# This instruction will place (2 6) on the stack
```

## Checking that a value is present at least once in a list

The `contains` function returns a boolean value indicating whether a value is present (at least once) in a list.

```
# We verify that the value "JEU" is present in the list

("L1" "L2" "L3" "L4" "L5" "L6" "DIML7 "L4" contains

# This instruction will place true on the stack
```

## Mathematical functions

Some mathematical functions use lists as input parameters. This is the case for example with the `sum`, `mean`, `min`, `max` functions.

The "Mathematical functions" paragraph explains their use.

# RECORDS

**MOGWAI** records are objects that allow you to associate a value with a key (similar to a dictionary).

## The KEY object

The key of an association is assigned to a `.key` type object which is a name that must end with the `:` (colon) symbol.

## The RECORD object

A `.record` type object is delimited by brackets `[ ]` and contains a series of key/value pairs.
A record can be empty, in which case it is simply noted as `[]`.

For example, a record containing an x and y value will have an `x:` key and a `y:` key and their value, which would give: `[x: 100 y: 50]`.
The value can be any **MOGWAI** object, and why not a key (which is a **MOGWAI** object so authorized), or another record.

A key can only be present once in a record. If this is not the case, only the value of the last occurrence of the key is taken into account.

`[x: 10 y: 20 x: 100]` is equivalent to writing `[x: 100 y: 20]`

## Adding or modifying keys

To add a new key or modify an existing key, you must use the `set` function by specifying the record to process, the key to use and the associated value.

```
# Example 1: We add the z: key with the value 300

[x: 100 y: 200] z: 300 set

# This instruction places [x: 100 y: 200 z: 300] on the stack

# Example 2: We modify the y: key by giving it the value 2000 instead of 200

[x: 100 y: 200] y: 2000 set

# This instruction places [x: 100 y: 2000] on the stack
```

## Retrieving the value of a key

To retrieve the value of a key, you must use the `get` function by indicating the record and the key.

```
# We retrieve the value of the y: key

[x: 100 y: 200] y: get

# This instruction places 200 on the stack
```
 
## Retrieving a key "buried" in a record

If a record is composed of sub-records and/or sub-lists, it may be interesting to give in a single operation the "path" to follow to retrieve the information.

```
# Method 1, basic, we retrieve information in multiple operations
# We will first retrieve the value of the gps: key then the value of the latitude: key

[id: 1 name: "DOE" gps: [latitude: 45 longitude: 5]] gps:

# This operation places the record [latitude: 45 longitude: 5] on the stack
# Then we retrieve the value of the latitude: key

latitude: get

# Which places 45 on the stack

# Method 2, we retrieve the information in a single operation

[id: 1 name: "DOE" gps: [latitude: 45 longitude: 5]] (gps: latitude:) get

# This operation directly places 45 on the stack
```

## Retrieving the size of a record (number of keys)

The `size` function returns the number of keys present in a record.

```
# We retrieve the number of keys in the record

[x: 100 y: 200] size

# This instruction places 2 on the stack
```

## Retrieving the list of keys from a record

The keys function returns the list of keys from a record.

```
# We retrieve the list of keys from a record

[x: 100 y: 200] keys

# This instruction places (x: y:) on the stack
```

## Extracting part of a record

The `extract` function allows you to extract only certain keys from a record in a single operation. It takes as parameters the source record and a list of keys to extract.

```
# We extract the x: y: keys from the record

[x: 100 y: 200 z: 70 u: 10] (x: y:) extract

# This instruction will place the record [x: 100 y: 200] on the stack
```

If you request a key that doesn't exist, it is added to the result record with the `null` value.

## Checking that a key is present in a record

The `contains` function returns a boolean value indicating whether a key is in a list.

```
# We check that the x: key is present in a record

[x: 10 y: 20] y: contains

# This instruction will place true on the stack
```

## Deleting a key in a record

The `purge` function allows you to delete a key. It takes as parameters the record and the key to delete.

```
# We delete the x: key from the record

[x: 10 y: 20] x: purge

# This instruction will place [y: 20] on the stack
```

## "Shorter" notation for keys

**MOGWAI** allows a more compact notation for keys passed as parameters when using the `get` and `set` functions using the `->` and `<-` symbols.

This notation is only accepted with a variable name, not directly with a record.

```
# We place a record in a variable and retrieve the value of the y key

[x: 10 y: 20] -> 'A'

A->y ?

# This instruction places the value 20 on the stack
# This instruction is the compact version of A y: get ?

# We place a record in a variable and add a key to it

[x: 10 y: 20] -> 'A'

500 A<-z

# This instruction places [x: 10 y: 20 z: 500] on the stack
# This instruction is the compact version of A z: 500 set
# Warning: the variable A is not modified.
```

# BYTE ARRAYS

In the industrial field, it is very often necessary to manipulate byte arrays.

Commands are sent in the form of byte arrays, information is received in the same form. It is often a matter of manipulating this data in all sorts of ways.

**MOGWAI** having initially been created to simulate a device using Bluetooth Low Energy, naturally has a whole battery of functions to manipulate byte arrays and bytes themselves as simply as possible.

A byte array is named DATA in **MOGWAI** and the type is `.data`.

It is possible to create a DATA directly with the `DATA:` notation followed by the bytes that compose it in hexadecimal format.

```
# We create a byte array composed of 4 bytes
# Which are AB 56 32 FF

DATA:AB5632FF

# Places the array of 4 bytes on the stack
```

You can also create an empty DATA with the `new` function:

```
# We create an empty DATA and store it
# In the global variable $D

.data new -> '$D'
```

You can add a byte to the DATA with the `+` function:

```
# We create a byte array composed of 4 bytes
# Which are 0xAB 0x56 0x32 0xFF

DATA:AB5632FF

# It is placed on the stack
# We now add a byte with value 0x56

0x56 +

# On the stack there is now DATA:AB5632FF56
```

The `size` function returns the size (the number of bytes) of the DATA.

You can concatenate 2 DATA with the `+` function:

```
# On place 2 DATA dans 2 variables globales

DATA:FF56EB23 -> '$A'
DATA:89CD34 -> '$B'

# We concatenate the 2 DATA that we store in another global variable

$A $B + -> '$C'

# $C now contains DATA:FF56EB2389CD34
```

To retrieve a particular byte from a DATA, you must use the `get` function (the 1st byte has index zero):

```
# We create a DATA composed of 4 bytes and we
# Extract the byte placed in 3rd position

DATA:FF56EB23 2 get

# The value 0xEB (235 in decimal) is placed on the stack
```

To modify the value of a particular byte, you must use the `set` function:

```
# We create a DATA composed of 4 bytes
# Then we modify the byte placed at position 1
# The value 0x56 will be replaced by 0x34

DATA:FF56EB23 1 0x34 set 

# DATA:FF34EB23 is placed on the stack
```

To modify part of a DATA with another DATA, you must also use the `set` function:

```
# We will replace the first 2 bytes of a DATA

DATA:FFC0AB0146 0 DATA:AABB set ?

# There is now DATA:AABBAB0146 on the stack
````

To delete a particular byte, you must use the `purge` function:

```
# We create a DATA composed of 4 bytes
# Then we delete the byte placed at position 1

DATA:FF56EB23 1 purge 

# DATA:FFEB23 is placed on the stack
```
 
To extract part of a DATA, you must use the `sub` function:

```
# We create a data composed of 6 bytes
# We extract 3 bytes starting from the 3rd byte

DATA:010203EB5634 2 3 sub

# DATA:03EB56 is placed on the stack
```

The `extract` function allows you to extract only certain elements from a data in a single operation. It takes as parameters the source data and a list of indexes to extract:

```
# We extract elements 1 2 4 from the data

DATA:FF45AB23EA (1 2 4) extract

# This instruction will place the data DATA:45ABEA on the stack
```

If you request indexes that don't exist (outside the indexes of the source data), an error is raised.

It is possible to transform a DATA into a list of numbers with the `->list` function:

```
# We transform a DATA into a list

DATA:FF45EB12AD89 ->list

# The list (255 69 235 18 173 137) is placed on the stack
```

From a list of numbers you can create a DATA with the `->data` function.
Warning, only numbers between 0 and 255 will be taken into account, other elements of the list will be ignored:

```
# We transform a list into DATA

(50 25 45 36 0xFF) ->data

# DATA:32192D24FF is placed on the stack
```

Also with the `->data` function, it is possible to create a DATA directly from the elements placed on the stack.

You just need to indicate how many elements to use. Warning, elements that are not numbers or whose value is not between 0 and 255 are ignored:

```
# We transform the stack elements into DATA
# We must indicate how many elements to use
# Here 6

50 25 45 36 12 0xFF 6 ->data ?

# DATA:32192D240CFF is placed on the stack
```

To find all occurrences of a byte in a DATA, you must use the `where` function:

```
# We will search for all occurrences of the value 0xC0 in a DATA

DATA:FFC005FA12C056EC 0xC0 where

# where will place the list (1 5) on the stack
# Because in this DATA, the value 0xC0 is present at position 1 and 5
```
 
You can also find the locations of a DATA in another:

```
# We will search for all occurrences of 0xFFC0 in a DATA
# This is equivalent to searching for a DATA in another (here DATA:FFC0)

DATA:FFC005FA12C056EC DATA:FFC0 where

# where will place the list (0) on the stack
# Because in this DATA, the value 0xFFC0 is present at position 0 only
```

## Conversion functions to a DATA

To efficiently manipulate byte arrays, you must be able to convert numbers into different formats.
For example, take a number and convert it to an unsigned integer on 16 bits (2 bytes), or to a signed integer on 32 bits (4 bytes) as needed.

**MOGWAI** offers for this purpose a series of conversion functions that take a number as a parameter and return the corresponding DATA after conversion.

For example, after converting a number to a signed integer on 32 bits, you will get a DATA composed of the 4 bytes corresponding to the result of the requested conversion.

Once the conversion is complete, it is quite simple to insert the result (which is a DATA) into a DATA with the `set` function.

Number conversion functions returning a DATA:

| Operation   | Usage                                                | Result                 |
|-------------|------------------------------------------------------|------------------------|
| `50 ->u8`   | Conversion to unsigned integer on 8 bits (1 byte)    | DATA:32                |
| `50 ->u16`  | Conversion to unsigned integer on 16 bits (2 bytes)  | DATA:0032              |
| `50 ->u32`  | Conversion to unsigned integer on 32 bits (4 bytes)  | DATA:00000032          |
| `50 ->u64`  | Conversion to unsigned integer on 64 bits (8 bytes)  | DATA:0000000000000032  |
| `-50 ->i8`  | Conversion to signed integer on 8 bits (1 byte)      | DATA:CE                |
| `-50 ->i16` | Conversion to signed integer on 16 bits (2 bytes)    | DATA:FFCE              |
| `-50 ->i32` | Conversion to signed integer on 32 bits (4 bytes)    | DATA:FFFFFFCE          |
| `-50 ->i64` | Conversion to signed integer on 64 bits (8 bytes)    | DATA:FFFFFFFFFFFFFFCE  |

If a number that is too large or too small is provided as a parameter, it will be truncated during conversion without raising an error.
 
## Advanced DATA display

To visualize the content of a DATA more simply, you can use the `?dump` or `?d` function which will display the dump of a DATA.

```
# We will download the main page of google.fr
# We store the response in the local variable R

[uri: "https://www.google.fr"] http.get -> 'R'

# The function returns a record composed of 2 keys
# state: of type .boolean which indicates if everything went well
# response: of type .data which contains the downloaded resource

if (R state: get) then
{
	# Everything is ok we can retrieve the response
	# Which we store in the local variable B
	
	R response: get -> 'B' 
	
	# We extract the first 1000 bytes and display them
	# In the form of a dump
	
	B 0 1000 sub ?dump
}
```

Here is an example of a DATA dump display:

```
00000000  3C 21 64 6F 63 74 79 70 65 20 68 74 6D 6C 3E 3C  | <!doctype html><  |
00000010  68 74 6D 6C 20 69 74 65 6D 73 63 6F 70 65 3D 22  | html itemscope="  |
00000020  22 20 69 74 65 6D 74 79 70 65 3D 22 68 74 74 70  | " itemtype="http  |
00000030  3A 2F 2F 73 63 68 65 6D 61 2E 6F 72 67 2F 57 65  | ://schema.org/We  |
00000040  62 50 61 67 65 22 20 6C 61 6E 67 3D 22 66 72 22  | bPage" lang="fr"  |
00000050  3E 3C 68 65 61 64 3E 3C 6D 65 74 61 20 63 6F 6E  | ><head><meta con  |
00000060  74 65 6E 74 3D 22 74 65 78 74 2F 68 74 6D 6C 3B  | tent="text/html;  |
00000070  20 63 68 61 72 73 65 74 3D 55 54 46 2D 38 22 20  |  charset=UTF-8"   |
00000080  68 74 74 70 2D 65 71 75 69 76 3D 22 43 6F 6E 74  | http-equiv="Cont  |
00000090  65 6E 74 2D 54 79 70 65 22 3E 3C 6D 65 74 61 20  | ent-Type"><meta   |
000000A0  63 6F 6E 74 65 6E 74 3D 22 2F 69 6D 61 67 65 73  | content="/images  |
000000B0  2F 62 72 61 6E 64 69 6E 67 2F 67 6F 6F 67 6C 65  | /branding/google  |
000000C0  67 2F 31 78 2F 67 6F 6F 67 6C 65 67 5F 73 74 61  | g/1x/googleg_sta  |
000000D0  6E 64 61 72 64 5F 63 6F 6C 6F 72 5F 31 32 38 64  | ndard_color_128d  |
000000E0  70 2E 70 6E 67 22 20 69 74 65 6D 70 72 6F 70 3D  | p.png" itemprop=  |
000000F0  22 69 6D 61 67 65 22 3E 3C 74 69 74 6C 65 3E 47  | "image"><title>G  |
00000100  6F 6F 67 6C 65 3C 2F 74 69 74 6C 65 3E 3C 73 63  | oogle</title><sc  |
00000110  72 69 70 74 20 6E 6F 6E 63 65 3D 22 66 4C 6F 78  | ript nonce="fLox  |
00000120  59 71 79 59 4B 73 59 35 69 6E 59 78 79 4E 4F 4C  | YqyYKsY5inYxyNOL  |
00000130  6E 41 22 3E 28 66 75 6E 63 74 69 6F 6E 28 29 7B  | nA">(function(){  |
00000140  76 61 72 20 5F 67 3D 7B 6B 45 49 3A 27 47 72 39  | var _g={kEI:'Gr9  |
00000150  62 61 50 58 77 45 2D 79 59 6B 64 55 50 5F 39 79  | baPXwE-yYkdUP_9y  |
00000160  43 32 51 59 27 2C 6B 45 58 50 49 3A 27 30 2C 32  | C2QY',kEXPI:'0,2  |
00000170  30 32 37 39 32 2C 36 32 2C 32 2C 36 30 39 36 32  | 02792,62,2,60962  |
00000180  35 2C 33 38 38 2C 32 38 38 37 34 31 34 2C 31 31  | 5,388,2887414,11  |
00000190  30 31 2C 35 35 32 37 37 32 2C 34 32 35 36 30 33  | 01,552772,425603  |
000001A0  2C 32 34 37 33 31 39 2C 34 32 37 32 35 2C 35 32  | ,247319,42725,52  |
000001B0  33 30 32 38 30 2C 31 31 34 30 32 2C 33 32 37 36  | 30280,11402,3276  |
000001C0  38 39 33 33 2C 34 30 34 33 37 30 39 2C 32 35 32  | 8933,4043709,252  |
000001D0  32 38 36 38 31 2C 31 33 38 32 36 38 2C 31 34 31  | 28681,138268,141  |
000001E0  31 38 2C 31 31 39 34 30 2C 35 33 32 32 32 2C 36  | 18,11940,53222,6  |
```

## DATA and character strings

Some conversion functions related to character strings take DATA as parameters or return DATA:

| Operation              | Usage                                                                                                     | Result    |
|------------------------|-----------------------------------------------------------------------------------------------------------|-------------|
| `DATA:414243 ->ascii`  | Returns the ASCII character string (8 bits)<br> composed with the bytes of the DATA passed as parameter.  | "ABC"       |
| `DATA:414243 ->ascii7` | Returns the ASCII character string (7 bits)<br> composed with the bytes of the DATA passed as parameter.  | "ABC"       |
| `DATA:414243 ->utf8`   | Returns the UTF8 character string<br> composed with the bytes of the DATA passed as parameter.            | "ABC"       |
| `DATA:414243 ->base64` | Returns the byte array in the form<br> of a character string encoded in base 64.                | "QUJD"      |
| `"ABC" ->ascii`        | Returns the byte array corresponding to the ASCII conversion (8 bits)<br> of a character string. | DATA:414243 |
| `"ABC" ->ascii7`       | Returns the byte array corresponding to the ASCII conversion (7 bits)<br> of a character string. | DATA:414243 |
| `"QUJD" ->base64`      | Returns the byte array corresponding to the decoding<br> of a string encoded in base 64.                 | DATA:414243 |

## Other available functions

Hash key calculation functions:

| Operation            | Usage                                     | Result                                      |
|----------------------|-------------------------------------------|-----------------------------------------------|
| `DATA:414243 ->md5`  | Returns the MD5 hash key of a DATA  | DATA:902FBDD2B1DF0C4F70B4A5D23525E932         |
| `DATA:414243 ->sha1` | Returns the SHA1 hash key of a DATA | DATA:3C01BDBB26F358BAB27F267924AA2C9A03FCFDB8 |

It is possible, with the compress function to compress a DATA, and decompress it with the `decompress` function:

```
# We will download the main page of google.fr
# We store the response in the local variable R

[uri: "https://www.google.fr"] http.get -> 'R'

# The function returns a record composed of 2 keys
# state: of type .boolean which indicates if everything went well
# response: of type .data which contains the downloaded resource

if (R state: get) then
{
	# Everything is ok we can retrieve the response
	# Which we store in the local variable B
	# And we compress the response which we store in the local variable C
	
	R response: get -> 'B' 
	
	B compress -> 'C'
	
	# We can display the size difference
	
	B size ?
	C size ?
	
	# We decompress C and display its size
	
	C decompress size ?
}
```

# BINARY NUMBERS

To simplify the manipulation of bits of a number, it is possible to use a **MOGWAI** object of type `.binary`.

In **MOGWAI**, a binary number starts with `BIN:` followed by the bits used. For example the binary number `11001101` in binary is written in **MOGWAI** as `BIN:11001101`.

You cannot manage a binary number of more than 64 bits.

The `size` function returns the size (in bits) of the binary number.

It is possible to create a binary number with the `new` function. By default `new` creates an 8-bit binary number.

```
# We create a binary number with new

.binary new 

# Which places BIN:00000000 on the stack
# We could have directly written BIN:00000000 as well

# We now display the size in bits of this binary number
# The displayed value will be 8

size ?
```

With the `resize` function, you can at any time modify the size of a binary number to adapt it to your needs:

```
# We create an 8-bit binary

BIN:11000111

# We change it to 16 bits

16 resize

# We display its size which is now 16 bits

size ?
```

It is possible to assemble 2 binary numbers with the `+` function:

```
# We assemble 2 binary numbers
# The 1st is 1 bit, and the second 7 bits
# The total will therefore be 8 bits in the end

BIN:1 BIN:1111111 + 

# Places BIN:11111111 on the stack
```

With the `->bin` function, you can create a binary number from a regular number. The number of bits of the created binary number will be limited to those necessary to represent the original number.

For example, the number 112 in binary is written as `1110000`, so the created binary number has a size of 7 bits.

To work on a more standard number of bits (e.g. 16 bits) you must use the `resize` function immediately after:

```
# We want to create a 16-bit binary number
# With value 112

112 ->bin 16 resize

# Places BIN:0000000001110000 on the stack
```

The `up` function allows you to raise a given bit, and the `down` function allows the opposite. You must give these functions the number of the bit to modify (the 1st bit has number 0):

```
# We create a 16-bit binary number having
# The value 112

112 ->bin 16 resize

# Places BIN:0000000001110000 on the stack

# We raise bit 15 and lower bit 5

15 up 
5 down

# Places BIN:1000000001010000 on the stack
```` 

To extract part of a binary number you must use the `sub` function by indicating from which bit to perform the extraction and how many bits to extract. The function returns a binary number composed of the extracted bits:

```
# We create a 16-bit binary number having
# The value 112

112 ->bin 16 resize

# Places BIN:0000000001110000 on the stack

# We extract 8 bits starting from bit 3

3 8 sub

# Places BIN:00001110 on the stack
```
 
It is also possible to perform bit shifts with the `shift` function. You must indicate by how many bits to shift and in which direction (a negative number of bits shifts to the left, otherwise to the right):

```
# We shift the binary number BIN:00000001 by 2 bits to the left

BIN:00000001 -2 shift

# Places BIN:00000100 on the stack

# We shift to the right by a single bit

1 shift

# Places BIN:00000010 on the stack
```

The `reverse` function allows you to "reverse" a binary number:

```
# We reverse the binary number BIN:11000111

BIN:11000111 reverse

# Places BIN:11100011 on the stack
```

The `not` function allows you to apply a binary not:

```
# We apply a binary not to BIN:11000111

BIN:11000111 not

# Places BIN:00111000 on the stack
```

To convert a binary number to a regular number you must use the `->num` function:

```
# We retrieve the numeric value of BIN:10011011

BIN:10011011 ->num

# Places 155 on the stack
```
 
# TIME MANAGEMENT

**MOGWAI** knows how to manipulate information concerning dates and durations.

A date is a number that represents the number of 100-nanosecond intervals that have elapsed since midnight, January 1st 0001. For example, the value representing the date of 05/03/2012 at 4:45 PM is 6.3466562759E+17.

Of course in this form it is not very practical, which is why **MOGWAI** has a whole series of functions to perform operations on dates and durations.

## Retrieving the current date

The `now` function returns (places on the stack) the current date of your machine.

## Retrieving the components of a date

To retrieve all the components (day, month, year, hour, etc.) of a date you must use the `->date` conversion function which takes as parameter a date (in numeric format) and returns a record containing all the components of this date.

The returned components are as follows (the keys of the returned record):

| Key          | Value                                                   |
|--------------|---------------------------------------------------------|
| `day:`       | Day of the month.                                       |
| `month:`     | Month.                                                  |
| `year:`      | Year.                                                   |
| `hour:`      | Hour.                                                   |
| `minute:`    | Minute.                                                 |
| `second:`    | Second.                                                 |
| `dayOfYear:` | Day number in the year (e.g. 244th day).               |
| `dayOfWeek:` | Day number in the week (Sunday=0, Monday=1, etc).      |

The returned components are all numbers.

```
# We retrieve the components of the date provided by now

now ->date

# Will place on the stack for example
# [day: 23 month: 5 year: 2025 hour: 12 minute: 19 second: 51 dayOfYear: 143 dayOfWeek: 5]
```

This function returns all the components of a date.

If you only need a single component (for example the year) there are targeted functions to return only the component you need:

| Function   | Usage                          |
|------------|--------------------------------|
| `->day`    | Returns the day of a date.     |
| `->month`  | Returns the month of a date.   |
| `->year`   | Returns the year of a date.    |
| `->hour`   | Returns the hour of a date.    |
| `->minute` | Returns the minutes of a date. |
| `->second` | Returns the seconds of a date. |

## Creating a date from scratch

To create a date from its components, simply provide a record containing the components of the date and use the `->date` function again which will return this date in numeric format.

It is not necessary to provide all the components, only the day, month and year are required. Those that are omitted are considered to be zero.

`[day: 15 month: 4 year: 2015] ->date` creates the date `15/04/2015 at 00:00:00`

`[day: 15 month: 4 year: 2015 hour : 15] ->date` creates the date `15/04/2015 at 15:00:00`

## Calculating durations

It is also possible to calculate durations. A duration being a difference between 2 dates, you can quite easily perform such calculations with **MOGWAI**.

```
# We calculate the time actually elapsed during a 2450 ms pause

now -> 'begin'
2450 sleep
now -> 'end'
end begin - ->duration ?

# Result = [days: 0 hours: 0 minutes: 0 seconds: 2 milliseconds: 461]
# That is 2 seconds and 461 milliseconds
```
 
To retrieve the time elapsed between 2 moments (2 dates) simply subtract the arrival date from the departure date and use the `->duration` function to extract the components of this duration.

The returned value is a record composed of 5 keys:

| Key              | Value                            |
|------------------|----------------------------------|
| `days:`          | Number of days of the duration.  |
| `hours:`         | Number of hours of the duration. |
| `minutes:`       | Number of minutes of the duration.|
| `secondes:`      | Number of seconds of the duration.|
| `millisecondes:` | Number of milliseconds of the duration.|

It is also possible to retrieve these components directly. In this case you get the total duration in the requested unit.

In our previous example, if we request the total seconds, with the `->seconds` function we will obtain the value of `2.461 seconds`.

| Function          | Usage                                    |
|-------------------|------------------------------------------|
| `->days`          | Duration expressed in days.              |
| `->hours`         | Duration expressed in hours.             |
| `->minutes`       | Duration expressed in minutes.           |
| `->secondes`      | Duration expressed in seconds.           |
| `->millisecondes` | Duration expressed in milliseconds.      |

```
# We calculate the time elapsed during a 2450 ms pause

now -> 'begin'
2450 sleep
now -> 'end'
end begin - ->seconds ?

# Result = 2.4551168 seconds

# We calculate the time elapsed during a 2450 ms pause
# With a more compact writing.

now -> 'begin' 2450 sleep now begin - ->seconds ?
```
 
# FUNCTION DECLARATION

In addition to all the functions provided as standard by **MOGWAI**, you can create your own functions which are actually called programs (type `.program`).

There are different ways to declare functions, we will see them one after another. The differences lie in the level of security of the passed parameters (verification of parameter types more or less advanced).

A function must be declared before it can be used.

A function is delimited by the symbols `«  »` (ALT 174 and 175).

From now on, the words 'function' and 'program' will be synonymous.

## Declaring a basic function

A basic function takes all its parameters from the stack. When it is declared nothing is said about the expected parameters. Of course, a function can have no parameters.

```
# We create a function carre that takes a number as parameter and returns its square
# We take the parameter from the stack, duplicate it then perform their multiplication
# The result remains on the stack, the function is finished.

to 'carre' do « dup * »

# To use it:

5 carre

# Places the value 25 (the square of 5) on the stack
```` 

A function, in addition to using all those provided by **MOGWAI**, can use those you define. For example to create the 'cube' function which will calculate the cube of a number we will use the 'carre' function that we defined above:

```
# We create a function cube that takes a number as parameter and returns its cube
# We take the parameter from the stack, duplicate it then calculate its square
# Then we multiply the 2 values to obtain the cube.
# The result remains on the stack, the function is finished.

to 'cube' do « dup carre * »

# To use it:

5 cube

# Places the value 125 (the cube of 5) on the stack
```
 
## Declaring a function with verified parameter types

It is possible to create a function with parameters verified at the time of the call. This avoids having to perform all the verifications in the body of the function. These are operations that can be tedious and costly in time.

For example in the previous function 'carre' nothing is verified, if you pass a character string as a parameter instead of a number an error will be raised at the time of performing the multiplication. Ideally you should verify that the type of the parameter is indeed `.number` before doing anything.

To avoid this you can indicate the expected parameters and their type as soon as the function is declared:

```
# We create a function carre with verification of the input parameter type.

to 'carre' with [x: .number] do « x x * »

# The expected parameter will be placed in the local variable 'x' and its type will be verified.
# If the number of parameters passed is insufficient or the type of one of the parameters is
# Wrong, an error is raised.

5 carre 

# Will place 25 on the stack

"EEE" carre

# The type is incorrect!
# An error is raised with an explanatory message:
# bad argument type
# ->safeVars
# .number expected but .string found for 'x' parameter

clear carre

# If we empty the stack and call the function without any parameters
# An error is raised:
# too few arguments
# ->safeVars

You can define if needed an unlimited number of verified parameters:
# We create a function that calculates a point on a line with the formula
# y=a*x+b which is in RPN a x * b +

to 'fx' with [a: .number b: .number x: .number] do « a x * b + »

# For the values y=5x+9 with x=156 we call

5 9 156 fx

# Which will place 5*156+9 or 789 on the stack
```

If you need to pass a parameter without checking its type you must use the `.any` type instead of a specific type:

```
# We create a function that displays a particular message if the type is a number.

to 'nPrint' with [x: .any] do
«
    if (x ->type .number ==) then
    {
        "It is a number !" ?
    }
    else
    {
        "It is not a number !" ?
    }
»

# We call with a number as parameter…

234 nPrint

# Will display the message "It is a number !"

# If we call with a boolean…

true nPrint

# Will display the message "It is not a number !"
```

## Declaring a function with named parameters

It is also possible to declare a function whose parameters are explicitly named and types verified (belt and suspenders). You can even define default values.
For code readability it is much clearer and security is maximal with this way of doing things.

The parameters are passed via a record whose keys are the names of the parameters and the values are those of the parameters.

If we declare our previous function 'fx' with this method it would look like this:

```
# We create a function that calculates a point on a line with the formula
# y=a*x+b which is in RPN a x * b +

to 'fx' params [a: .number b: .number x: .number] do « a x * b + »

# For the values y=5x+9 with x=156 we call

[a: 5 b: 9 x: 156] fx

# Which will place 5*156+9 or 789 on the stack
```
 
It is possible to call this type of function in a less RPN way (parameters then function) by including the function name in the 1st position of the parameters record (this notation is only possible with function calls, this type of notation for a record does not exist elsewhere):

```
# We create a function that calculates a point on a line with the formula
# y=a*x+b which is in RPN a x * b +

to 'fx' params [a: .number b: .number x: .number] do « a x * b + »

# For the values y=5x+9 with x=156 we call

[fx a: 5 b: 9 x: 156]

# Which will place 5*156+9 or 789 on the stack
```

To declare default values, simply stipulate the type and the default value in a list. Thus, if the parameter is not provided, the default value will be used:

```
# We create a function 'foo' that has an optional boolean parameter save:
# If we don't specify it, it will have true as default value

to 'foo' params [id: .number name: .string save: (.boolean true)] do 
« 
    " " ?
    "id   = {! id}" eval ?
    "name = {! name}" eval ?
    "save = {! save}" eval ?
»

[foo id: 10 name: "DOE John"]
[foo id: 30 name: "SMITH Mike" save: false]

# During the 1st call, the save: parameter will have the value true (default value)
# During the 2nd call, it will have the value false (parameter provided explicitly)
```

## Retrieving the list of declared functions

The `funcs` function returns the list of declared functions in the form of a list of names. It is possible for example, during the program, to verify that a function exists before trying to use it.

```
# We create the functions carre and cube

to 'carre' do « dup * »

to 'cube' do « dup carre * »

# We list the existing functions

funcs 

# Places the list ('carre' 'cube') on the stack
```

# ERROR HANDLING

In case of a problem, **MOGWAI**, like most programming languages, raises an error and stops the program.
It is possible to manage the triggering of an error and ensure that the program does not crash stupidly.

## The trap instruction

To avoid stopping the program in case of an error, the `trap` instruction allows you to "protect" a block of code. If an error occurs, the protected code stops and the code continues just after the `trap` instruction.

> Warning, when an error occurs, the state of the stack is often difficult to know, this is one of the difficulties.

```
# We will generate an error by using a variable that does not exist yet.
# The code will be protected by the trap instruction.

trap 
{ 
    "trap begins." ?
	
    10 a *
	
    "This message will never be displayed." ?
}

"Sortie du trap." ?
"Le code continue…" ?
```

## The guard instruction

The `guard` instruction is a bit more advanced than `trap`. It allows you to execute code if an error occurs.

```
# We will generate an error by using a variable that does not exist yet.
# The code will be protected by the guard instruction.

guard 
{ 
    "guard begins." ?
	
    10 a *
	
    "This message will never be displayed." ?
}
else
{
    "An error has occurred in the guard!" ?
}

"exit of the guard" ?
"the code continues…" ?
```
 
## Knowing the last error raised

Knowing that an error occurred without killing the code is good but knowing what error was raised is better to be able to react.

The `error.last` function returns the code of the last generated error.

The `error.reset` function allows you to reset (no error) the code of the last error. So it is good practice to reset this information once you have finished handling the last error because it will not reset itself.

```
# We will generate an error by using a variable that does not exist yet.
# The code will be protected by the guard instruction.

guard 
{ 
    "guard begins." ?
	
    10 a *
	
    "This message will never be displayed." ?
}
else
{
    "The error " ?? error.last ?? " happened!" ?
    error.reset
}

"exit of the guard" ?
```

## Artificially raising an error

It is possible to raise an error using the `error.throw` function which takes as parameter the number of the error to raise.

List of main errors:

| Number | Label                  |
|--------|--------------------------|
| 0      | no error.                |
| 1      | too few arguments.       |
| 2      | bad argument type.       |
| 3      | parse error.             |
| 4      | record definition error. |
| 6      | unknown type.            |
| 7      | bad name.                |
| 8      | bad key.                 |
| 9      | syntax error.            |
| 10     | unknown word error.      |
| 11     | null eval error.         |
| 12     | unknown var.             |
| 13     | empty stack.             |
| 14     | bad argument value.      |
| 15     | internal error.          |
| 16     | busy error.              |
| 17     | halt encounted.          |
| 18     | bad type error.          |
| 19     | engine error.            |
| 20     | file access error.       |
| 21     | not a number error.      |
| 24     | size too small.          |
| 25     | illegal name.            |
| 40     | name already exists.     |
| 41     | unknown name.            |
| 200    | var already exists.      |
| 201    | function already exists. |
| 202    | unknown function.        |
| 203    | const already exists.    |
| 204    | const can't be deleted.  |
| 205    | const can't be modified. |

# MAKING A PAUSE

It is sometimes necessary to pause in a program or function. **MOGWAI** offers 2 functions for this that have slightly different functionality.

## The `sleep` function

With the `sleep` function, the program is completely suspended for the number of milliseconds passed as parameter.
This means that during the pause, absolutely nothing happens. Events are not received and processed, same for timers. The entire execution engine is suspended.

If you don't use events or timers, it doesn't matter, otherwise you should use the `wait` function instead.

## The `wait` function

With the `wait` function, the program is suspended for the number of milliseconds passed as parameter but events and timers continue to work.

```
# We will display the numbers from 1 to 100
# With a pause of 250 milliseconds between each

1 100 for 'i' do
{
    i ?
    250 wait
	
    # We could have also written 250 sleep
}
```
 
# EXITING A FUNCTION, A LOOP OR THE PROGRAM

The flow of a program can be "broken" by the 4 functions `mogwai.exit`, `mogwai.halt`, `break` and `return`.
 
## The `mogwai.exit` function

It is possible at any time to stop the program, to exit it.
The `mogwai.exit` function takes care of that.

When a program terminates without error (normal stop or caused by the `mogwai.exit` instruction), the reserved function `MOGWAI.onStop` is automatically executed by **MOGWAI**. If it is defined in your code it will be called automatically:

```
# We define the function that will be executed at the end
# Normal program

to 'MOGWAI.onStop' do 
« 
    "The program has just ended." ?
»

# We perform an infinite task
# But if a value < 50 comes out we stop the program

forever do
{
    # We draw a random number and store it
    # In the local variable 'r'
    rand 1000 * ->int -> 'r'
	
    # We display it and take a short break
    r ? 250 wait
	
    # If the number is < 50 then we stop the program
    if (r 50 <) then {exit}
}

# So the code that follows will never be executed
# But the MOGWAI.onStop function will be automatically executed

"Death code!" ?
```
 
## The `mogwai.halt` function

The `mogwai.halt` function behaves exactly like the `mogwai.exit` function, but it raises error 17, "halt encounted" instead of saying nothing at all. So it's an error stop.

When a program terminates on an error (`mogwai.halt` raises an error), the reserved function `MOGWAI.onError` is automatically executed by **MOGWAI**. If it is defined in your code it will be called automatically:

```
# We define the function that will be executed
# If an error is raised in the program

to 'MOGWAI.onError' do 
« 
    "An error has occurred!" ?
»

# We perform an infinite task
# But if a value < 50 comes out we stop the program
# With halt which causes an error stop

forever do
{
    # We draw a random number and store it
    # In the local variable 'r'
    rand 1000 * ->int -> 'r'
	
    # We display it and take a short break
    r ? 250 wait
	
    # If the number is < 50 then we stop the program
    if (r 50 <) then {halt}
}

# So the code that follows will never be executed
# But the MOGWAI.onError function will be automatically executed

"Death code!" ?
```

## The `break` function

When you are in a loop (see the LOOPS chapter) it is possible to exit "by force" with the `break` function which can be used in `while`, `for`, `foreach`, `during`, `repeat` and `forever` loops.

```
# We display the numbers from 1 to 100 with a for loop
# If the current number is > 10 we exit the loop

1 100 for 'i' do
{
    i ?
	
    if (i 10 >) then {break}
}

# The code continues here

"Continuing the program…" ?
```
 
## The `return` function

It allows you to exit a function prematurely.

```
to 'displayValue' with [value: .number] do 
« 
    # We display the value as is
    # Unless it's the value 5 which we replace with a message
    # We could have done it with an else but it's just for the example
	
    if (value 5 !=) then
    {
        value ?
        return
    }

    "Valeur 5 interdite !" ?
»

1 10 for 'i' do
{
    i displayValue
}
```
 
# AUTOMATIC VARIABLE CREATION

## The `->vars` function

The `->vars` function avoids many operations when you want to create local variables from a source such as a record, or from the stack.

### `->vars` from a record

If you have a record and you need to retrieve the included values to manipulate them, the basic solution is to retrieve the values to manually assign them to local variables before processing them.

```
# We need to process the values carried by the x: and y: keys
# Of a record that is stored in the local variable 'r'

[x: 50 y: 30] -> 'r'

# Basic method, we manually retrieve the values
# To store them in local variables with the same name as the keys

r x: get -> 'x'
r y: get -> 'y'

# Now, we can process the values
# Through the local variables
```

You can simplify the code by using the `->vars` function

```
# We need to process the values carried by the x: and y: keys
# Of a record that is stored in the local variable 'r'

[x: 50 y: 30] -> 'r'

# Faster and automatic method
# To store them in local variables with the same name as the keys

r ->vars

# Now, we can process the values with the local variables x and y
# Which were automatically created by ->vars and which carry the values
# that the corresponding keys have in the RECORD.
```

### `->vars` from the stack

It is possible to automatically extract elements from the stack and store them in local variables with `->vars`.

Simply specify the list of variables to create as a parameter. The number of elements corresponding to the number of variables in the list will be taken from the stack and stored in the corresponding local variables. If there are not enough elements on the stack to fill all the listed variables, the function raises an error without modifying the stack.

```
# We place 5 elements on the stack for the test

56 
"HELLO" 
12.34 
(1 2 3) 
true

# We store the stack elements in the local variables a b and c

('a' 'b' 'c') ->vars

# The variables a b and c have been created with the values
# a=12.34, b=(1 2 3) and c=true
# The elements "HELLO" and 56 have not been taken
```

## The `->safeVars` function

With the `->safeVars` function it is possible to verify that the values present on the stack are indeed those expected. You can verify their number and type, and automatically assign local variables with the stack values. In case of non-compliance an error is raised.

```
# We place 5 elements on the stack for the test

56 
"HELLO" 
12.34 
(1 2 3) 
true

# We store the stack elements in the local variables a b and c
# We determine which types are expected

[a: .number b: .list c: .boolean] ->safeVars

# The variables a b and c have been created with the values
# a=12.34, b=(1 2 3) and c=true
# The elements "HELLO" and 56 have not been taken
# By the way, ->safeVars verified that the value taken from the stack for the variable
# 'a' is of type .number, for 'b' of type .list and for 'c' of type .boolean.
```
 
This function is used automatically when you declare a function with the `with` keyword:

```
to 'carre' with [x: .number] do « x x * »

5 carre 

# Will place 25 on the stack
```

## The `->params` function

The `->params` function allows you to pass named parameters (key/value pairs in a record) and verify that the expected parameters are indeed present and that their type matches. If everything is correct, the local variables corresponding to the expected parameters are automatically created with the corresponding values.

This function takes 2 records as parameters. The 1st contains the values to retrieve, the second describes the expected parameters and their type.

For example, to retrieve 2 parameters, named nom and age, nom being a character string and age a number, we will have as parameter definition record:

`[nom: .string age: .number]`

So to pass "STEPHANE" for the name and 55 for the age we will have:

`[nom: "STEPHANE" age: 55] [nom: .string age: .number] ->params`

As everything matches, **MOGWAI** will create the local variables `'nom'` with the value "STEPHANE" and `'age'` with the value 55.

```
# We pass as parameter a name of type character string
# And an age which is a number

[nom: "STEPHANE" age: 55] [nom: .string age: .number] ->params

nom ?
age ?
```

If you pass values with the wrong type, an error is raised:

```
[nom: "STEPHANE" age: "TOO OLD"] [nom: .string age: .number] ->params

# age does not have the right type
# An error is raised
```

If you pass more parameters than expected, they will simply be ignored. On the other hand, if you don't pass all the expected parameters, an error is raised.
 
To pass a parameter with any type, you must use the .any type

```
[nom: "STEPHANE" age: 55 libre: true] [nom: .string age: .number unrestricted: .any] ->params

nom ?
age ?
unrestricted ?

# Will display:
# STEPHANE
# 55
# true
```

This function is used automatically when you declare a function with the `params` keyword:

```
to 'fx' params [a: .number b: .number x: .number] do « a x * b + »

[a: 5 b: 9 x: 156] fx

# Which will place 5*156+9 or 789 on the stack
```
 
# OBJECT EVALUATION

**MOGWAI** allows you to place direct references to variables, functions and even executable code in certain objects.

Objects that can support this possibility are records, lists and character strings.

When you use direct references, they will not be automatically replaced by their value at the moment you use them.

## Evaluating a list

If you have a variable `A` with the value 100, and you place the list `(4 5 A 50)` on the stack, you will have `(4 5 A 50)` on the stack and not `(4 5 100 50)`.

For the list to use the true value of `A` you must evaluate it using the `eval` function.

So if you place `(4 5 A 50)` on the stack and use `eval` right after, you will finally have `(4 5 100 50)` on the stack.

## Evaluating a record

The same thing is possible with a record:

`[x: 10 y: 50 z: A] eval` will give `[x: 10 y: 50 z: 100]`

## Evaluating a character string

For character strings, you must use the code block notation in which you just display the name of the variable to replace.

If you need to include the value of `A` in a character string, you can for example write:

`"The value of A is {! A}" eval` which will give `"The value of A is 100"`

The `!` symbol must be stuck to the opening brace of the code block otherwise the sequence will not be recognized.

## Using code directly in objects

It is possible to use code in the objects seen previously:

```
# We will display the multiplication table of 7

0 9 for 'i' do
{
  "7 x {! i} = {! i 7 *}" eval ?
}

# Which will display
# 7 x 0 = 0
# 7 x 1 = 7
# 7 x 2 = 14
# ...
# 7 x 6 = 42
# 7 x 7 = 49
# 7 x 8 = 56
# 7 x 9 = 63
```

You can do the same with a list, with `A` having the value 100:

`(A {! A 2 *} {! A 3 *}) eval` will give `(100 200 300)`

Or with a record:

`[x: A y: {! A 2 *} z: {! A 3 *}] eval` will give `[x: 100 y: 200 z: 300]`

## Faster notation for evaluation

The `eval` function can be replaced in lists and records by the `!` symbol in first position.

If we take our previous examples:

`(! A {! A 2 *} {! A 3 *})` will give `(100 200 300)`

`[! x: A y: {! A 2 *} z: {! A 3 *}]` will give `[x: 100 y: 200 z: 300]`

It is no longer necessary to call the `eval` function, the evaluation is performed directly before placing the value on the stack.
 
# FLAGS

Flags are used to indicate a state. A flag has a name and a state that can be either activated or deactivated.

## Activating a flag

It is the `flag.set` function that activates a flag. It takes as parameter the name of the flag to activate: `'MY_FLAG' flag.set`

## Deactivating a flag

It is the `flag.clear` function that deactivates a flag. It also takes as parameter the name of the flag to deactivate: `'MY_FLAG' flag.clear`

## Checking that a flag is activated

To check if a flag is activated, you must use the `flag.isSet` function which returns `true` if the flag is activated, and `false` otherwise.

It takes as parameter the name of the flag to check: `if ('MY_FLAG' flag.isSet) then { ... }`

## Checking that a flag is deactivated

To check if a flag is deactivated, you must use the `flag.isClear` function which returns `true` if the flag is deactivated, and `false` otherwise

It takes as parameter the name of the flag to check: `if ('MY_FLAG' flag.isClear) then { ... }`

## Listing activated flags

The `flags` function returns the list of all activated flags. Deactivated flags are considered non-existent and therefore do not appear in this list.

# FILE MANAGEMENT

**MOGWAI** has functions to manage folders and files.

## Path representation

**MOGWAI** does not manipulate paths in the form of character strings, it uses lists where each element is a node of the path.

For example, under Windows the path: `"C:\ProgramData\Microsoft\DRM"` will be manipulated by **MOGWAI** via the list `("C:" "ProgramData" "Microsoft" "DRM")`

## The HOME folder

The **MOGWAI** runtime is designed by default to operate in a "closed" environment. That is, it only has access to the folder (and subfolders) that is declared as the `HOME` folder (root).

In this mode (**MOGWAI CLI** operates this way), **MOGWAI** cannot go above its `HOME` folder. However, it can access all the subfolders and files of `HOME`. Thus, a path will always start with `HOME`.

## Folder management

To manage folders **MOGWAI** has the following functions:

`dir.directories` which returns the list of subfolders of the current folder. By default the current folder is `HOME`.

`dir.enter` which allows you to enter a folder whose absolute path is provided as a parameter. Warning, the path is provided in the form of a list as seen previously and the 1st element must be `HOME`.

`("HOME" "folder1") dir.enter`

It is also possible to give just the folder name and not the entire path from `HOME` to reach it: `"folder1" dir.enter`

`dir.files` which returns the list of files present in the current folder. The current folder by default is `HOME` (of course).

`dir.make` which allows you to create a folder from the current folder: `"folder2" dir.make`

`dir.path` which returns the absolute path to the current folder in the form of a list (since it's a path). The 1st element will therefore always be `HOME`.

Assuming that the `folder1` folder exists under `HOME`:

```
("HOME" "folder1") dir.enter
"folder2" dir.make
"folder2" dir.enter
dir.path
# The list ("HOME" "folder1" "folder2") will be placed on the stack.
```

`dir.programs` which returns the list of program files (*.mog files) present in the current folder. Each element is a character string corresponding to the name of the found file.

`dir.purge` which allows you to destroy a folder that is in the current folder. Warning if the folder is not empty, the deletion fails: `"folder2" dir.purge`

`dir.rename` which renames the current folder: `"folder9" dir.rename`

`dir.home` which returns to `HOME` and makes it the current folder.

## File management

The following functions allow file management:

`file.purge` which deletes the file whose name is passed as a parameter: `"myfile" file.purge`

`file.pack` which serializes a **MOGWAI** object into a file. The **MOGWAI** object can be of any type (number, list, character string, record, boolean value, etc): `(1 2 3 4 5) "list.mog" file.pack`

`file.unpack` which deserializes an object that has been serialized in a file by the `file.pack` function: `"list.mog" file.pack` will place the list `(1 2 3 4 5)` on the stack

`file.read` which reads the binary content of the file passed as a parameter and returns the data in the form of a DATA: `"myfile" file.read`

`file.write` which writes the binary data of the DATA in the file whose name is passed as a parameter. In this example the file will contain the hexadecimal binary data 56 66 34 EA 12 56: `DATA:56FF34EA1256 "myfile" file.write`

All these functions operate in the current folder.

 
# TIMERS

**MOGWAI** allows you to execute code at regular intervals. Timers manage this.

You can create as many timers as you want. Timers use their own stack and therefore cannot disturb that of your main program. The downside of this is that the code of a timer does not have access to what you may have placed on the stack and therefore cannot use it to pass parameters for example. This partitioning is necessary because as the code of a timer can be triggered at any time, it must not disturb the proper functioning of your program.

The code of a timer has access to the global variables of the program currently running.

To use a timer, you must declare it then activate it. At any time you can stop it and delete it.

## Timer of type `after`

A timer of type `after` will only trigger once, after a defined period. The period is defined in milliseconds.

When it triggers, its code is executed (with its own stack) and it stops. To use it again simply restart it.

## Timer of type `every`

A timer of type `every` will trigger at regular intervals. The period is also defined in milliseconds.

When it triggers, its code is executed (with its own stack) then it is reprogrammed to trigger again after the defined period has elapsed.

## Declaring a timer

Declaring a timer is done with a very simple syntax
(thanks to **MOGWAI**'s syntactic sugar).

For a timer of type after:

```
# We declare a timer of type after
# After 5 seconds it will display a message

timer 'timer1' after 5000 do 
«
    "Hello !" ? 
»

# We activate the timer

'timer1' timer.start

# We wait without doing anything for it to trigger

forever do
{

}
```

After 5 seconds, the message "Bonjour !" will be displayed, then nothing more will happen.

For a timer of type every:

```
# We declare a timer of type every
# After 5 seconds it will display a message

timer 'timer1' every 5000 do 
«
    "Hello !" ? 
»

# We activate the timer

'timer1' timer.start

# We wait without doing anything for it to trigger every 5 seconds

forever do
{

}
```

Every 5 seconds the message "Bonjour !" will be displayed.

Here are the available functions to manage timers:

`timer.start` activates the timer whose name is passed as a parameter: `'timer1' timer.start`

`timer.stop` stops the timer whose name is passed as a parameter: `'timer1' timer.stop`

`timer.purge` deletes the timer whose name is passed as a parameter: `'timer1' timer.purge`

`timer.state` returns true if the timer whose name is passed as a parameter is active: `'timer1' timer.state`

`timer.list` returns the list of declared timers.

## Suspending timer triggering

It may be necessary to ensure that timers do not trigger for a certain time.

The `DI` (disable interrupts) function allows you to block the triggering of timers.

The `DI` function prevents the execution of the timer code but does not suspend the timer itself. If for example a timer of type `after` triggers while `DI` is active, its code will not be executed and the timer will become inactive.

To reactivate timers use the `EI` (enable interrupts) function.

## In case of pause in the code

For timers to trigger, your code must not block the management of **MOGWAI**'s internal messages.

To pause, use the `wait` function and not the `sleep` function. The `sleep` function completely suspends the runtime and internal messages (timers and events) are no longer managed.

## Launching code with an execution delay

There are cases where you need to launch code after a certain delay. **MOGWAI** has a mechanism based on `after` type timers to achieve this functionality:

```
# We launch a function in 2 seconds

after 2000 do
«
    "Hello world !" ?
»

# We wait without doing anything for it to trigger

forever do
{

}
```

You have no control over the execution of the function, impossible to delete it before its execution.
 
# EVENTS

**MOGWAI** can trigger and respond to events. An event is defined by a name (for example 'MON_EVENEMENT') and by code to execute when it is triggered.

An event can be triggered by the **MOGWAI** code currently running or by the application that hosts the engine (**MOGWAI CLI** hosts the **MOGWAI** runtime, and as such can trigger events in your code).

Your **MOGWAI** code can also generate events intended for the application that hosts the runtime. It's a way to communicate with it.

> Interactions between the runtime and the hosting application (the host) will not be covered in this documentation but in the one that explains how to integrate **MOGWAI** into a host application.

## Declaring an event

To respond to an event you must declare it. For example, to declare the event 'MON_EVENEMENT' which will simply display "Bonjour !" when it is triggered, simply enter:

```
event 'MY_EVENT' do
«
    "Hello !" ?
»
```

When the event 'MON_EVENEMENT' is triggered, **MOGWAI** will execute the associated code.

The code of an event systematically has the local variable `eventData` which carries the parameter of the event (for example a name or a number). This value is provided by the one who triggers the event. If no value is associated, `eventData` carries the `null` value.

## Triggering an event

From your **MOGWAI** code you can trigger events at any time.

It is the `event.fire` function that allows you to trigger an event. It takes as parameters the name of the event and the associated parameter (if no parameter use `null`): `'MY_EVENT' null event.fire`

## Listing supported events

It is possible at any time to list all the events to which you can respond. It is the `event.list` function that handles this. It returns the list of names of declared events.

## Removing support for an event

For your **MOGWAI** application to stop responding to an event, you must delete it with the `event.purge` function which takes as parameter the name of the event to delete.

## Putting events on hold

As with timers, events can be blocked by the `DI` (disable interrupts) function. Warning, if you use the `DI` function, events are not forgotten, they are queued and when interrupts are reactivated by the `EI` (enable interrupts) function they will all be executed one after the other.
 
You can test this behavior with the following code:

```
mogwai.reset cls

event 'MY_EVENT' do
«
    "Hello num {! eventData}" eval ?
»

1 100 for 'i' do
{
    'MY_EVENT' i event.fire
    1000 wait
	
    if (i 10 ==) then { DI }
	
    if (i 20 ==) then { EI }
}
```

# SKILLS

When you integrate the **MOGWAI** library into an application to make it "controllable", you are able to define a list of "skills" managed by the application.
In **MOGWAI** these are called `skills`.

For example, by default **MOGWAI CLI** has 2 skills which are SQLite support and the use of serial ports.

You can ask what the skills are by using the skills function which returns a list composed of all skills in the form of character strings.
 
You can see that **MOGWAI CLI** has the skill "SQLDB" (SQLite support) and "SERIAL" (serial port support).

To test if a skill is present, simply use the `isSkill` function with the skill to test as a parameter: `"MY_SKILL" isSkill`

It returns `true` if the tested skill is present.

Under **MOGWAI CLI**, if you request `"SQLDB" isSkill`, `true` will be returned (placed on the stack).

# CLASSES

The **MOGWAI** language allows using a "lightweight" version of object-oriented programming (OOP).

It allows declaring classes and instances of these classes. A simplified form of inheritance is possible.

Perhaps over time these features will expand, but for now they are here to simplify your life by allowing you to structure your programs more strongly.

## Defining a class

A **MOGWAI** class is defined using the `defClass` function. This function takes as parameter a record in which everything that composes the class is defined.

A class has:
- A `className` which corresponds to its name, optionally
- A `base` which is the parent class from which it will inherit
- Internal (private) variables
- `props` properties (public)
- Public methods `funcs`
- `events` events

Overall, a class has this architecture:

```
[defClass

    className: 'Person'

    vars:
    [
        counter: .number    
    ]

    props:
    [
        id: .number
        name: .string
        country: .string
    ]

    funcs:
    [
        create: 
        «
            [id: .number name: .string] ->params
			
            id -> 'self:id'
            name -> 'self:name'
            "France" -> 'self:country'

            0 -> 'counter'
        »

        release: 
        « 
            "RELEASE {! self}" eval ?
        »
		
        display:
        «
            'counter' ++
            
            "CLASS    = {! self:className}" eval ?
            "ID       = {! self:id}" eval ?
            "NAME     = {! self:name}" eval ?
            "SAVE     = {! self:save}" eval ?
            "COUNTRY  = {! self:country}" eval ?
            "COUNTER  = {! counter}" eval ?
        »
    ]

    events:
    [
        event1:
        «
            "EVENT 1 from {! self}" eval ?
        »
		
        event2:
        «
            "EVENT 2 from {! self}" eval ?
        »
    ]
]
```

It is imperative to define the class name, via the `className` key (e.g. 'Person').

You can also define the parent class, which we call the base. In our example, our 'Person' class does not inherit from any parent class. We will see inheritance in a second step.

The class is composed of several sections:

### The `vars` section

This section allows declaring internal private variables. 
They can only be used from functions defined in the class. 
You indicate the variable name and its type (e.g. `counter: .number`)

```
vars:
[
    counter: .number    
]
```

### The `props` section

This section allows declaring public properties. 
They can be used from functions defined in the class but also from outside the object. 
As with private variables, you must indicate the property name and its type (e.g. `id: .number`)

```
props:
[
    id: .number
    name: .string
    country: .string
]
```

The properties `className` and `base` are reserved and return the class name and parent class name (if defined) respectively.
 
### The `funcs` section

This section allows declaring the class methods. 
As with properties, they can be used from methods defined in the class but also from outside the object. 
You must indicate the function name and its body (of type `.program` with `« »`).

In the following example, we define 3 methods: create, release and display:

```
funcs:
[
    create: 
    «
        [id: .number name: .string] ->params
			
        id -> 'self:id'
        name -> 'self:name'
        "France" -> 'self:country'

        0 -> 'counter'
    »

    release: 
    « 
        "RELEASE {! self}" eval ?
    »
		
    display:
    «
        'counter' ++
            
        "CLASS    = {! self:className}" eval ?
        "ID       = {! self:id}" eval ?
        "NAME     = {! self:name}" eval ?
        "SAVE     = {! self:save}" eval ?
        "COUNTRY  = {! self:country}" eval ?
        "COUNTER  = {! counter}" eval ?
    »
]
```

The `create` and `release` methods are reserved for the instance lifecycle. 

When an instance is created, **MOGWAI** automatically calls the `create` method if it exists.

When an object is deleted from memory, **MOGWAI** automatically calls the `release` function if it exists.

Class functions systematically have a local variable that points to the instance itself. 
This variable is named `self` and contains the reference to the instance (e.g. @54).
 
### The `events` section

This section allows declaring the events to which the object will subscribe. 
The event body is private to the object and impossible to reach from the outside. 
The format is the same as the `funcs` section.

```
events:
[
    event1:
    «
        "EVENT 1 from {! self}" eval ?
    »
		
    event2:
    «
        "EVENT 2 from {! self}" eval ?
    »
]
```

## Instantiating an object

Once the class is defined, we can create instances of it. The `alloc` function handles this.

Its syntax is simple:

``` 
alloc 'Person' to '$C1'
```` 

Which can be translated as "Create an instance of the 'Person' class and place the reference of this instance in the variable '$C1'."

During this operation, if the `create` method exists, it will be automatically called once the instance is created.

In our example, the create method expects named parameters id: and name: which it expects to find on the stack during its execution. 
To do this, simply provide the parameters expected by the `create` function before invoking the `alloc` function:

```
[id: 50 name: "DUPONT"] alloc 'Person' to '$C1'
```

The `create` method will find the expected parameters on the stack and use them.

## Reading or writing a property

From this point, the instance created by `alloc` and initialized with its create function is stored in the global variable $C1.

Access to an instance property is written with the `:` symbol (colon). On the left, we place the reference to the instance (here it is stored in $C1) and on the right, we place the property to read (for example name):

```
$C1:name ?

# Will display the content of the name property of the object stored in $C1.
```

To store a value in the name property, we use the `sto` or `->` function with the same syntax:

```
"SMITH John" -> '$C1:name'
```

## Calling a method

Calling a method is done exactly the same way.
For example, to call the display method (which displays the person's information), we will write:

```
$C1:display
```

If the method needs parameters to function, you will need to place them on the stack before calling it (which is not the case in this example).

## Using `self`

In the code of a method, if you need to access elements (properties and methods) of the object itself, you must use the `self` local variable which is automatically created by **MOGWAI**.

For example, the display method uses self to retrieve property values and display them:

```
display:
 «
    'counter' ++
            
     "CLASS    = {! self:className}" eval ?
     "ID       = {! self:id}" eval ?
     "NAME     = {! self:name}" eval ?
     "SAVE     = {! self:save}" eval ?
     "COUNTRY  = {! self:country}" eval ?
     "COUNTER  = {! counter}" eval ?
»
```

## Using an instance variable (private)

Our example 'Person' class defines a private instance variable whose name is counter and whose type is .number (number).

This variable is only accessible from methods and not from the outside (`$C1:counter` will not work).

Since it's a private variable, no need to qualify it with `self`, it is directly accessible by simply using its name.
In the `display` method, we use it to count the number of times we display a particular instance.

From the method code, you have access to global variables (those that start with $) and you can also use local variables as you would in a "classic" function.

## End of life of an instance

When an instance is used nowhere, **MOGWAI** realizes it and frees the memory. Before that, it launches the release method if it exists.
 
## Inheritance

Inheritance allows a class to inherit all elements from another class (the parent class, or base). It is also possible to add elements and replace methods in the new class.

The parent class is defined with the base key which carries the name of the parent class.

To take an example, if we create a 'Customer' class that inherits from 'Person' (our example above), without defining anything more, we end up with 2 absolutely identical classes.

```
[defClass className: 'Customer' base: 'Person']
```

All elements of the Person class are present by default in the Customer class.

In the 'Customer' class, we can define properties, methods and events that will be added to those of the Person class. 
We can for example add the turnover property which will indicate the revenue generated with the customer. 
This parameter will be filled during the creation of the instance, so we need to modify the create method so that it takes it into account and assigns it correctly.

To achieve this, we must override the create method and call the base's create (the one from 'Person' therefore) from it. To access an element of the base, simply qualify it with the word base. For example, the base name property will be noted base:name

```
create:
«
    # We expect the 3 parameters id, name and turnover
			
    [id: .number name: .string turnover: .number] ->params
			
    # We call the base's create with the id and name parameters
			
    [! id: id name: name] base:create
			
    # Then we assign the turnover property 
			
    turnover -> 'self:turnover'
»
```

To display the new 'turnover' property in addition to the old ones, we also need to override the display method. We call the base method, then add the display of turnover:

```
display:
«
    base:display
    "TURNOVER = {! self:turnover} $" eval ?
»
```
 
If we create an instance in $C1 of the Customer class, assign USA as country and display it, it will give as code:

```
[
    id: 60 
    name: "FLYN KEVIN"
    turnover: 25000] alloc 'Customer' to '$C1'

"USA" -> '$C1:country'

$C1:display

# It will display on screen:
# CLASS     = 'Customer'
# ID        = 60
# NAME      = FLYN KEVIN
# COUNTRY   = USA
# COUNTER   = 1
# TURNOVER  = 25000 $
```

Whereas if we create an instance in $C2 of the Person class, assign USA as country and display it, it will give as code:

```
[
    id: 60 
    name: "FLYN KEVIN"] alloc 'Person' to '$C2'

"USA" -> '$C2:country'

$C2:display

# It will display on screen:
# CLASS     = 'Customer'
# ID        = 60
# NAME      = FLYN KEVIN
# COUNTRY   = USA
# COUNTER   = 1
```

If we now try to place the value 35000 in the `turnover` property of $C2, it will not work because $C2 is of type `Person` and the `Person` class does not have a `turnover` property.
 
## Events

Each class can determine which events it should respond to.

As with "classic" **MOGWAI** events, the `eventData` local variable carries the event parameter (if no parameter, `eventData` will have the value null).

As with methods, the `self` variable will also be available, pointing to the object itself.

If we have the following events in the Person class:

```
events:
[
    event1:
    «
        "EVENT 1 from {! self}" eval ?
    »
		
    event2:
    «
        "EVENT 2 from {! self}" eval ?
    »
]
```

And in $C2 an instance of the Person class, if we activate the event1 event:

```
# We trigger the event1 event without parameter

'event1' null event.fire

# We will have on screen (for example)
# EVENT 1 from @1
```

## Reserved properties

As already mentioned above, some properties are reserved:

| Property      | Usage                                      |
|---------------|--------------------------------------------|
| `className`   | Name of the instance's class.              |
| `base`	    | Name of the instance's parent class        |
| `props`	    | List of properties as names                |

## The props function

```
$C1 props
```

Will return for example:

```
[id: 60 name: "FLYN KEVIN" country: "USA"]
```

Be careful not to confuse the `props` function with the reserved `props` property!

## The ?instances function

To track the state of instances in memory a bit, it is possible to display on the console a summary showing for each created instance the number of uses (when the use drops to zero, the instance is automatically destroyed).

```
?instances
``` 

Could display:

```
@1          - 0006
@2          - 0002
```

Indicating that instance @1 is used 6 times and @2, only 2 times.

This function is clearly made for debugging.

To know the type of @1 you can write:

```
@1:classeName ?
```

## Alternative notations

The notation `instance:property` or `instance:method` is in fact the shortcut of a more verbose notation (but 100% RPN).

When you want to read the name property of $C1 you can simply write:

```
$C1:name
```

Or also:

```
$C1 name: get
```

Same thing to write a value in a property, you can simply write:

```
"MARTIN Jacques" -> $C1:name
```

Or also:

```
$C1 name: "MARTIN Jacques" set
```

Method calls work the same way. To invoke the display method you can simply write:

```
$C1:display
```

Or also:

```
$C1 display: get
```

Note that on a method, the `set` function will raise an error.
