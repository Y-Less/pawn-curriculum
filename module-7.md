The default editor for SA:MP is pawno.  As previously mentioned, this editor is widely derrided by experienced coders, for whom it is not intended.  For beginners who need simplicity and ease of access over more advanced abilities, it is fine (not perfect, but good enough).  Of course, beginners eventually become experienced and want these better systems.  The documentation for a few are listed below:

* [Notepad++](https://notepad-plus-plus.org/)
* [Sublime](https://www.sublimetext.com/)
* [VSCodium](https://vscodium.com/)
* [Qawno](https://github.com/Zeex/qawno)
* [PawnPlus](https://github.com/WopsS/PawnPlus/)

Other editors, including vim and emacs, currently have no common pawn support.  However, the compiler can also be invoked directly:

```
pawncc gamemode_name_here.pwn -ipawno/includes -(+ -;+ -ogamemode_name_here
```

`gamemode_name_here.pwn` is the input file (relative to the compiler).
`-ogamemode_name_here` is `-o` followed by the output file, also relative to the compiler, but with no extension.
`-iincludes` is `-i` followed be the default location of include files.  If the compiler is in the `pawno` directory, this would equate to `pawno/includes`, as with every other parameter being relative to the compiler.
`-(+ -;+` are two flags required by SA:MP.  They make brackets and semi-colons required in code, rather than optional.  Certain libraries will also set these flags internally.

There are several other options available, run the compiler with no arguments, or check pawn-lang.pdf for more information.

## sampctl 

This is a dependency management system for Pawn, which handles downloading and including files, and running the SA:MP server.  It is by far the best method for using Pawn after pawno, and is integrated in several of the editors mentioned above already.  It is extensively documented at https://sampctl.com


