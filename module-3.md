# Pawn 3

## Modules

Modules are individual isolated parts of a mode.  Each one providing a single piece of functionality - for example a taxi system or checkpoint streamer.  Similar to includes in that they also do one thing, with the only difference really being that includes tend to be third-party, while modules are more mode-specific.  However, the basic principle is the same.  They are also called "systems", and the act of using them is called "modular programming":

* https://forum.sa-mp.com/showthread.php?t=597338

## Style Guide

The style guide presented here is based on officially released SA:MP code, i.e. the official includes and bundled gamemodes, plus conventions adopted by a majority of the community over the years.  Everyone has their own preferred style they sometimes use in the face of existing code, and there are arguments about that because the SA:MP includes and core includes are inconsistent in style (a problem being worked on), there's no need to be consistent at all.  This is simply false if you ever want to share code with other people (yours or theirs).

```pawn
// Constants and macros in "UPPER_CASE", with underscores separating words.
#define MAX_TAXIS (32)

const INVALID_TAXI_ID = -1;

// enums are constants, with `e_` or `E_` prefix depending on tag strength (see pawn-lang.pdf).
enum e_TAXI_FLAGS
{
	e_TAXI_FLAG_OCCUPIED,
	e_TAXI_FLAG_EMPTY,
	e_TAXI_FLAG_BILKED,
}

// Strongly tagged enum.
enum E_TAXI_DATA
// "Alman" braces (always on a new line).
{
	e_TAXI_FLAGS:E_TAXI_DATA_FLAGS,
	// Tags are "Camel Case", with the first letter determined by tag strength rules.  However,
	// since tags are always followed by a colon, this is always unambiguous.
	Float:E_TAXI_DATA_X,
	Float:E_TAXI_DATA_X,
	Float:E_TAXI_DATA_X
}

// Globals `static` as much as possible to prevent complex inter-module dependencies.
static
	// Globals `g` prefixed and camel case.
	gTaxiData[MAX_TAXIS][E_TAXI_DATA];

// Functions in "PascalCase" (aka "UpperCamelCase") - all words start with an upper-case letter.
Taxi_HasPassenger(taxi, playerid)
{
	// Tab indentation, set to 4 spaces (pawno intermixes tabs and 4 spaces).

	// Single declaration for multiple variables, with each on a new indented line.
	new
		// Variables/parameters in "lowerCamelCase" - as above, except the first word.
		playerCount = GetMaxPlayers(),
		
		// Module names come first before an underscore.
		passenger = Taxi_GetCurrentPassenger(taxi);

	if (0 <= playerid < playerCount && passenger == playerid)
	{
		// Braces even for single statements.
		goto found_the_player;
	}

// Pre-processor directives use separate indentation level tracking.
#if MAX_PLAYERS < 100
	#if MAX_TAXIS > 100
		return Taxi_HasPassenger(playerid, taxi);
	#else
		return false;
	#endif
#else
	return false;
#endif

// Labels in `lower_snake_case:`, and at column 0.
found_the_player:
	return true;
}
```

## Destructors

Pawn has destructors.  This was only discovered after about a decade of common use, and is still not widely known due to a lack of information.  In fact the majority of information on this is only found in issues and commits on the community compiler:

* https://github.com/pawn-lang/compiler/issues?utf8=%E2%9C%93&q=destructor
* https://github.com/pawn-lang/compiler/pull/260

In short:

```pawn
operator~(Tag:data[], size)
{
	// Called with a single element array containing `[5]` (thus `size` is `1`) when `Func` ends.
}

Func()
{
	new
		Tag:a = Tag:5;
	printf("%d", _:a);
}

main()
{
	Func();
}
```

## Extended Syntax

Thanks to a lot of research and development over the years, pawn has been extended within itself to include new keywords and control structures.  These add many more modern language features and make things much easier to use.

### `inline`

An inline function is a function inside another function: https://github.com/pawn-lang/YSI-Includes/blob/5.x/YSI_Coding/y_inline.md

### `hook`

Hooks have alredy been covered under the section on ALS.  But ALS hooks are slow and complex to write.  The much simpler method is to declare `hook` functions, allowing multiple files/libraries/modules to be notified when something happens: https://github.com/pawn-lang/YSI-Includes/blob/5.x/YSI_Coding/y_hooks.md

### `foreach`

`for` is fine for certain types of loop, when you want every number between two end points.  However, often you only want a subset of valid values - for example connected players.  This is where `foreach` comes in: https://github.com/pawn-lang/YSI-Includes/blob/5.x/YSI_Data/y_iterate.md

A basic introduction is here: https://misiur.github.io/2015/03/07/y_iterate-aka-foreach-what-is-it-and-how-to-use-it/

The use and implementation of this language feature has been very extensively documented on the forums; including an in-depth explanation of how it works:

* https://forum.sa-mp.com/showpost.php?p=3430951
* https://forum.sa-mp.com/showpost.php?p=3430953
* https://forum.sa-mp.com/showpost.php?p=3430954
* https://forum.sa-mp.com/showpost.php?p=3430959
* https://forum.sa-mp.com/showpost.php?p=3430963

Again, this was a topic originally posted by Y_Less then later reposted after the original was deleted.

### `yield`

An extension to `foreach`, repeatedly passing data to a calling function from inside the called function: https://forum.sa-mp.com/showpost.php?p=4011246

### `foreign`/`global`

Develop modes across multiple scripts: https://github.com/pawn-lang/YSI-Includes/blob/5.x/YSI_Core/y_master.md

### `remotefunc`

`CallRemoteFunction` wrapper, to hide ugly and error-prone specifier strings (e.g. `"iis"`): https://github.com/pawn-lang/YSI-Includes/blob/5.x/YSI_Coding/y_remote.md

