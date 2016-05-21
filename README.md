# EagleScript

**EagleScript** is a **general-purpose**, **high-level**, **imperative**, **non-object oriented** scripting language with a **C-like** syntax.

It was at first the scripting language for the **[Eagle game engine][EagleEngine]**, but then its game development-specific features were removed from its core and moved into the engine systems, thus making EagleScript a standalone, general-purpose scripting language.

## Features

- A high-level syntax, visually similar to C but with some inherent differences, such as having typeless variables
- An optimizing compiler, which turns high-level EagleScript code (\*.es) into low-level EagleAssembly (\*.easm)
- An assembler turning the low-level EagleAssembly code into its equivalent bytecode (\*.eve)
- A virtual machine (VM) for executing the bytecodes created from the scripts
- Fully embeddable and extendable through C++

### Sample code

The following code is written in the high-level **EagleScript** language:

```
#include "empty.es"

#define SOME_MACRO(a, b) a + b

variable number = 10.35,    // An variable currently holding a floating-point number
	string = "Hello world!", // A variable currently holding a string
	variable0;              // Uninitalized variable

function add(number0, number1); // A function taking two parameters and returning their sum

function initialize() // Script entry point (must be present if a script is going to be executable)
{
	variable0 = 2 ^ 3;
	variable localVariable = 7;
	localVariable = add(localVariable, number);
	localVariable += string; // The final value of "localVariable" is "17.35Hello World!"
}

function add(number0, number1)
{
	return number0 + number1;
}
```

When the code above is compiled, the following low-level **EagleAssembly** code is generated:

```
; sample.es.easm

; Source File : sample.es
; EagleScript Compiler Version 0.2
; Time : Sat May 21 16:53:52 2016

; ------- Directives --------------------

; ------- Global Variables --------------

	Variable	FunctionParameterList[16]
	Variable	number
	Variable	string
	Variable	variable0

; ------- Main Functions --------------------------

	Function Preload()
	{
		; variable number = 10.35,    

		mov			number, 10.350000

		; 	string = "Hello world!", 

		mov			string, "Hello world!"

		return
	}

	Function initialize()
	{
		Variable	localVariable

		; 	variable0 = 2 ^ 3;

		mov			Register1, 3
		mov			Register0, 2
		exp			Register0, Register1
		mov			variable0, Register0

		; 	variable localVariable = 7;

		mov			localVariable, 7

		; 	localVariable = add(localVariable, number);

		call			add, localVariable, number
		mov			localVariable, ReturnValue

		; 	localVariable += string; 

		add			localVariable, string

		return
	}

; ------- Functions ---------------------

	Function add(number0, number1)
	{
		; 	return number0 + number1;

		mov			Register1, number1
		mov			Register0, number0
		add			Register0, Register1
		mov			ReturnValue, Register0

		return
	}
```

## Development Status

**This project is no longer under development.**

The EagleScript toolchain was created mainly for learning purposes. It acted as a testing ground for the developer to learn about various aspects of compiler theory in action. The code isn't structured well but, nevertheless, provides a complete scripting system that is fit for many purposes (game development being one of them).

**Here are the reasons why one should _not_ consider using EagleScript in their project:**

- It suffers from complete lack of documentation;
- The coding conventions are old and ineffective at best;
- The code does not make a good use of C++;
- The different parts of the system are heavily coupled which makes refactoring and maintaining code cumbersome;
- The code comes with no license or warranty;
- Its use of git is simply horrible.

[EagleEngine]: https://github.com/SahandMalaei/EagleEngine "Eagle Engine"