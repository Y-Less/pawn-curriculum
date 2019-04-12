# Pawn 1

This module covers pawn, the language.  It does not discuss anything related to San Andreas Multiplayer.  Thus, the information here can be used to code any system that uses pawn (AMXModX, Fallout 3 MP, and more).

## Definitions

First, some very important definitions that beginners often mix up, which then leads to issues when asking for help because those trying to help don't know what they're asking about:

### PAWN

This is the name of the programming language.  It is the main focus of this entire curriculum.  The language is the text written by programmers, including human-readable names and structures.  This is usually stored in a `.pwn` file (amongst others).

### P-code

P-code is the compiled version of pawn.  It is the version of the code that a computer can understand.  It uses numbers instead of commands and names, because they are simpler for a computer to understand.  It operates directly on computer memory, and is not designed for a person to understand.  This is usually stored in a `.amx` file, and so is usually referred to as "AMX".

### pawncc

This is the proper program name of the compiler; however, this is rarely referred to directly.  The compiler is what converts PAWN to p-code, i.e. converts a `.pwn` file to a `.amx` file.  Nice human-readable names are converted to numbers (instructions and addresses).  This is why going back from AMX to PAWN is almost impossible - the nice names are not stored in the AMX because a computer doesn't need them.

### Pawno

Pawno is the editor included with SA:MP.  It is NOT the compiler, and it is NOT the language.  People frequently ask for "help with pawno", or "pawno crashing", this is likely not what they mean.  It is a text editor, like notepad or Word.

Many experienced coders complain about Pawno.  As a coding editor it is extremely basic, but for a beginner, basic is all that is required.  Pawno allows people to get started writing code (especially for SA:MP) in less than a second.  This ease of access doubtless contributed to SA:MP's success by getting so many new people involved and expressing their ideas.

Thus, against common wisdom, we will actually use Pawno in parts of this guide.

### Programming

Programming is the act of telling a computer what to do.  Computers are very stupid - if you tell it something, it will do EXACTLY what you said.  People complain that their computer isn't doing the right thing - this usually means the computer isn't doing what they *intended*, but it is doing what they *said*.  See this link for an excellent demonstration of this idea:

* https://www.youtube.com/watch?v=cDA3_5982h8

Everyone knows what was meant, except the computer (dad), which just followed the instructions precisely as given with no thought at all.  It also perfectly demonstrates debugging - having the "computer" run (read) the instructions, seeing what happens, then tweaking them to be more explicit.  Finally, it nicely demonstrates the frustration this process can cause when the computer doesn't do what you meant over and over and over.

For this reason, programmers must be extremely precise, and this is assumed to extend to writing about programming.  So pay attention to terms and names, and use them correctly.

## Getting Started

To start coding, lets open Pawno (if you don't have it, download and extract the SA:MP server packge - this is the only part of this module that will mention  SA:MP).

## Basic Syntax

By far the best resource for learning basic pawn syntax is, and has always been, pawn-lang.pdf; the original documentation for the language itself:

* https://github.com/pawn-lang/compiler/raw/master/doc/pawn-lang.pdf

The tutorial introduction is a good start, with a few notes:

1. If you want to try these examples out in pawno, first save this code as `pawno/include/default.inc`:

```pawn
native printf(const str[], {Float, _}:...);
#define printf(%0...%1); OldPrintfCompat(%0...%1) = printf;
```

The original version of Pawn had this function defined elsewhere, SA:MP defines it in `a_samp.inc`.

2. The very beginning of the tutorial mentions the difference between two optional syntaxes, shown by `hello.p` and `hello.p - C style`.  Pay close attention to the C style, while most of the examples in this document use the other style, most modern pawn uses the C style instead.  Attempting to compile the examples in Pawno will actually help understanding a lot better in this way - the code as written won't compile without the extra brackets and semi-colons being added, so straight copy and paste won't work.

3. The sections on "Rational numbers" mentions two types - fixed point and floating point.  We will only use floating point, whose differences are explained at the end of that section.

4. The sections on "Event-driven programming" and "Multiple events" use functions of the form `@EventName`, while the remainder of this guide will use `public EventName` instead.  These are equivalent, simply subtly different syntax for the same thing.  These "events" are also called "callbacks".

5. Skip the sections on "State programming", "Entry functions and automata theory", "State variables", and "State programming wrap-up".

## Modern syntax

As mentioned above, the coding style used in modern pawn is subtly different to that in the original documentation.  The following links will give a much better idea of this syntax, when armed with the existing knowledge from the documentation.

* https://wiki.sa-mp.com/wiki/Scripting_Basics
* https://wiki.sa-mp.com/wiki/Control_Structures
* https://wiki.sa-mp.com/wiki/Keywords:Statements
* https://wiki.sa-mp.com/wiki/Keywords:Initialisers

Ignore `assert`, `exit`, `goto`, `sleep`, and `state` from the third link above.

The keen-eyed among you may have noticed two more keyword pages, but these can be left for now:

* https://wiki.sa-mp.com/wiki/Keywords:Operators
* https://wiki.sa-mp.com/wiki/Keywords:Directives

For even more reference, at this point you can also read the next few sections of pawn-lang.pdf - "Data and declarations", "Functions", "The preprocessor", "General syntax", "Operators and expressions", and "Statements".  Don't forget `default.inc`.

## Core Library

The core library consists of:

* `core.inc`: This is in pawn-lang.pdf under the section "Proposed Function Library".
* `float.inc`: https://raw.githubusercontent.com/compuphase/pawn/master/doc/Floating_Point_Support.pdf
* `file.inc`: https://raw.githubusercontent.com/compuphase/pawn/master/doc/File_IO_Support.pdf
* `string.inc`: https://raw.githubusercontent.com/compuphase/pawn/master/doc/String_Manipulation.pdf
* `time.inc`: https://raw.githubusercontent.com/compuphase/pawn/master/doc/Time_Functions.pdf

There is also `datagram.inc` included with SA:MP but it does nothing.  Other documentation relates to `console.inc`, `fixed.inc`, and `process.inc`, but these aren't in SA:MP at all.

These are includes independent from SA:MP that provide very basic functions.

