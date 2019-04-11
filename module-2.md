# Pawn 2

## New Compiler

By far the most important step you can take in improving in Pawn is to replace the compiler with the latest version from here:

https://github.com/pawn-lang/compiler/releases

Just overwrite the old files with the new ones in the "bin" folder in the archive.

For information on what has changed in this version, check the associated wiki:

https://github.com/pawn-lang/compiler/wiki

Take special note of the page on "Const Correctness", as this can produce a lot of new warnings in code that has always been wrong, but silently.  "What's new" covers new features (most of which are for fairly advanced corner-cases), and "Known compiler bugs" goes over what is fixed in this version.  Other pages are more internal details.

## Pre-Processor

This aspect of Pawn has been somewhat skirted around up to this point - only basic includes and definitions have been mentioned so far but there is far more to it than that.  pawn-lang.pdf and the wiki have some information:

https://wiki.sa-mp.com/wiki/Keywords:Directives

## Automata

The other major language feature deftly avoided to the point is states.  The simple reason being that they are not widely used in SA:MP.  This is not because they are no use, or because they are broken, but simply scalability.  States adjust the behaviour of an entire script, whereas with multiple players, actions tend to be per-player.  When one player enters a checkpoint, the code should do one thing, but something else for another player.  Using states, only one of these situations could be represented at any given time.  However, they do have uses when some setting truly is global.  Module 1 mentioned several tutorial sections of pawn-lang.pdf to avoid, go back and read them now.  Then read the forum tutorial:

https://forum.sa-mp.com/showthread.php?t=570939

YSI contains several examples automata, but they are not widely used anywhere else:

Using states to enable and disable debug messages:

https://github.com/pawn-lang/YSI-Includes/blob/9868b37e42dd0827058f7ddcae0012a872fb27aa/YSI_Core/y_core/y_debug_impl.inc#L575-L593

Using states to determine how to show a piece of text:

https://github.com/pawn-lang/YSI-Includes/blob/9868b37e42dd0827058f7ddcae0012a872fb27aa/YSI_Players/y_text/y_text_render.inc#L760-L811

Using states to call another function based on which parameters are required:

https://github.com/pawn-lang/YSI-Includes/blob/9868b37e42dd0827058f7ddcae0012a872fb27aa/YSI_Storage/y_ini/y_ini_impl.inc#L1479-L1517

## ALS

ALS stands for "Advanced Library System", though very few people know that, and it isn't actually important what it stands for any more.  What it is is a way to use the same callback in many different files.  There are two versions of ALS, one is documented here:

https://forum.sa-mp.com/showthread.php?t=574534

The other is documented here:

https://forum.sa-mp.com/showpost.php?p=4091581

You might note that this version uses states, it is a little slower, but has one advantage - greater control over hook order.

## #pragma

This directive gives instructions to the compiler, mainly changing settings.  The main ones are:

### `dynamic`

Changes how much memory is allocated for the stack and heap.  These are the memory locations in which local variables are stored.  Stack "depth" is how many functions have called each other within each other.  A single function that calls 100 other functions is not deep because those are all just sequentially.  But if one function calls another function which in turn calls another which also calls another, then this is a deeper chain.  The ultimate is when a call is recursive - a function calls itself, which means it can call itself again, and if you aren't careful will never stop calling itself.

If the compiler needs more memory for these deep calls it will give the following message:

```
Header size:           5032 bytes
Code size:            60924 bytes
Data size:           763444 bytes
Stack/heap size:      16384 bytes; estimated max. usage=10122 cells (40488 bytes)
Total requirements:  845784 bytes
```

This is the important line:

```
Stack/heap size:      16384 bytes; estimated max. usage=10122 cells (40488 bytes)
```

Meaning:

```
size = 16384 bytes; usage = 40488 bytes
```

Clearly `40488` is bigger than `16384`, so we need more memory.  How much more is the other number (`10122`, the size in cells (4 bytes)).  So to fix this warning (which is only an estimate):

```pawn
#pragma dynamic 10240 // Something bigger than 10122
```

If the compiler can't work out how much memory is needed, you will get a subtly different message:

```
Header size:            148 bytes
Code size:              320 bytes
Data size:              400 bytes
Stack/heap size:      16384 bytes; estimated max. usage: unknown, due to recursion
Total requirements:   17252 bytes
```

This is a little harder to gauge, but the same solution after guessing.

The default stack size is 16384 bytes, 4096 cells.

### `unused`

Hide unused variable warnings:

```pawn
Func(a)
{
	#pragma unused a
}
```

### `deprecated`

Mark a function as no longer used, and inform people of the new version without breaking existing code:


```pawn
#pragma deprecated Use `MyNewFunc` instead.
MyOldFunc()
{
}

MyNewFunc()
{
}

main()
{
	MyOldFunc(); // Warning given here.
}
```

The other pragmas are in pawn-lang.pdf and on the new compiler's wiki.

