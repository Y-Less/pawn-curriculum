# Pawn 4

## Macros

This has been covered before in module 2 as the pre-processor, but there is a large amount more to go in to.  The most in-depth coverage of the subject is on the SA:MP forums, in a seven part tutorial:

* https://forum.sa-mp.com/showpost.php?p=3430908
* https://forum.sa-mp.com/showpost.php?p=3430993
* https://forum.sa-mp.com/showpost.php?p=3431027
* https://forum.sa-mp.com/showpost.php?p=3431034
* https://forum.sa-mp.com/showpost.php?p=3431037
* https://forum.sa-mp.com/showpost.php?p=3431039
* https://forum.sa-mp.com/showpost.php?p=3431040

These are originally by Y_Less, and were kindly reposted after the originals were deleted.  Unfortunately the "Additional" links are all broken.

## amx_assembly

This is a library for inspecting, modifying, and manipulating the currently running script.  Unfortunately there is not a lot of documetation available currently, mostly just examples in libraries such as YSI:

* https://github.com/Zeex/amx_assembly

There is also y_amx which does a similar thing, but with a smaller scope.  This has been mostly supplanted by amx_assembly, but still handles looping over and searching header items better:

* https://github.com/pawn-lang/YSI-Includes/blob/5.x/YSI_Storage/y_amx.md

## #emit

This is the ultimate in power for coding, but also the ultimate in responsibility.  It removes all checks, all boundaries, all help, and all sanity.  The compiler converts pawn to p-code (see module 1), this is just entering raw p-code.  The general advice is - if you don't know what it is for, just don't use it because you probably don't need it.  It is the basis for several advanced libraries, including parts of YSI and amx_assembly (though they tend to abstract in much the same way as normal code to present a nicer interface).  There are two good sources of information on this:

* https://forum.sa-mp.com/showpost.php?p=3430898
* https://github.com/YashasSamaga/AMX-Assembly-Docs

There is also `@emit` in amx_assembly, which adds run-time code generation; and `emit__`, added in the community compiler, which is an inline expression version of `#emit` (for use in expressions directly).

