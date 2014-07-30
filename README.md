Fasm
====

The fasm.o is an object file in ELF format. To get the final executable for
your system, you need to use the appropriate tool to link it with the C
library available on your platform. With the GNU tools it is enough to use
this command:

`gcc fasm.o -o fasm`

On OS X, it's a little more complex, but not terribly so.

First, we need to convert fasm.o from an ELF32 file to a Mach-O file.
The fasm.mo file is an object converted using Agner Fog's
[objconv program](http://www.agner.org/optimize/#objconv).
The exact command to create fasm.mo from fasm.o is:
`objconv -nu+ -fmacho fasm.o fasm.mo`
The `-nu+` adds underscores to symbol names, which is necessary to link with
the standard library that Clang/LLVM uses on OS X.
After that, it's quite simple to compile this Mach-O file:

`clang -m32 -o fasm fasm.mo`

This same command should work with GCC, as well. A warning is generated about
making a non-PIE executable, but the resulting binary seems to work.
