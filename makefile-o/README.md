Demonstration of how Makefiles being used for sequencing (rather than
compiling C) can result in some strange behavior.

Here's what you might expect to see in your terminal if you run `make`
in this directory:

```
Before-command
Main command, with parameter foo
After-command
```

Here's what actually happens, with GNU Make 4.1:

```
Main command, with parameter foo
After-command
Main command, with parameter o
cc   main.o before main.foo after   -o main
cc: error: main.o: No such file or directory
cc: error: before: No such file or directory
cc: error: main.foo: No such file or directory
cc: error: after: No such file or directory
cc: fatal error: no input files
compilation terminated.
<builtin>: recipe for target 'main' failed
make: *** [main] Error 1
```

On the fourth line we get `Main command, with parameter o`, which in a
real system may error out before the C compiler calls can be made,
resulting in a *very* opaque error message.

One solution is to put `.PHONY: main` in the Makefile.
