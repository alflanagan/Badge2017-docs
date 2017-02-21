# HackRVA Badge 2017 Style Guide

The badge this year is being developed using Github. Developers on the badge, 
need to keep a certain consistency to ease collaboration and generally keep the
badge code easier to read. There are some small points beyond just code 
formatting included here to maintain some other operating principles.

More to the point:

IF YOU DO NOT ADHERE TO THE STYLE GUIDE WE WILL REJECT YOUR PULL REQUEST.

## A Little Help

A tool such as [GNU Indent](https://www.gnu.org/software/indent/) may help 
rectify mistakes and can automatically fix some possible departures from this
style. Command line invocation details for `indent` will be added once the
style is finalized.

## Function Names

Functions should be named with a prefix and a description such that words
in the function name are separated by underscores and the names are all
lowercase.

The format will generally be:

```C
void pfx_descriptor();
```

Functions that are meant to be private should be prefixed by an underscore:

```C
void _pfx_descriptor();
```

Functions that are tightly connected to a struct should have the struct name 
included just after the prefix.

```C
void pfx_struct_name_descriptor();
```

## Struct Names

Structs must be `typedef`ed, be lowercase, must have a name prefix, and be 
postfixed with `_t`.

```C
typedef struct pfx_descriptor_t {
} pfx_descriptor_t;
```

## Enum Names

Enums must not be `typedef`ed, be lowercase, must have a prefix, and be 
postfixed with `_e`.

```C
enum pfx_descriptor_e {
  PFX_MEMBER1,
  PFX_MEMBER2,
  PFX_MEMBER3
};
```

## Header Files

Every C file that defines a function to be called from another file
should have a corresponding header file (with same name and .h
extension). Every non-private function in the C file should have a
prototype in the header file. The C file itself, and any other C file
that calls a function from the C file, should `#include` the matching
header file.

For private functions, it's a good idea (but not required) to put a
prototype at the top of the file, just to help the compiler check
arguments.

This allows the compiler to ensure that every call to each function
has arguments of the correct number and type. Many such calls will not
be caught by the linker.

The header file with prototype is also a good location for the
external function documentation, i.e. anything that function callers
need to know. The function definition is the place for documentation
that developers who are modifying the function should know.

## Global Variables (`extern`)

Several global variables are defined in the source. The rules for a
variable which is shared between multiple source files are as follows:

* A header file only contains extern declarations of variables — never
  static or unqualified variable definitions.

* For any given variable, only one header file declares it (SPOT —
  Single Point of Truth).

* A source file never contains extern declarations of variables —
  source files always include the (sole) header that declares them.

* For any given variable, exactly one source file defines the
  variable, preferably initializing it too. (Although there is no need
  to initialize explicitly to zero, it does no harm and can do some
  good, because there can be only one initialized definition of a
  particular global variable in a program).

* The source file that defines the variable also includes the header
  to ensure that the definition and the declaration are consistent.

* A function should never need to declare a variable using extern.

These guidelines are "stolen" from a
stackoverflow.com [answer](http://stackoverflow.com/a/1433387/132510)
which has an extensive discussion best practices for global variables
in C. Note I left out one rule: "Avoid global variables whenever
possible — use functions instead". That's usually excellent advice,
but may cause a space or time penalty that should be avoided in
embedded programming. Use your best judgement (and if in doubt, try it
both ways and see what happens).

## Indentation

Whenever a new level of curly braces is reached the lines within those curly
braces must be indented four spaces.

If the arguments to a function need to be separated into multiple lines, 
each argument must havetheir own line and each start at the same column.

```C
int pfx_descriptor() {
    /* Code goes here */
    int argument1 = 0, argument2 = 1, argument3 = 2;
    the_func_call(argument1,
                  argument2,
                  argument3);
    return 1 + 1;
}
```

## Blame

Any time that a file is modified, the modifier must add their name to comments 
at the top of the file listing everyone who has modified that file. This is
in addition to pull request records that will include similar and additional
records.

## Comments

Doxygen documentation generation will be used for this project so in-code
documentation must be readable by Doxygen.

