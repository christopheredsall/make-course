[Prev](basics.md) [Next](autovariables.md)

# Variables

For the previous example, we had only one source code file to
compile--hello_world.c. In the real world, programs grow rather rapidly and we
end up keeping the source code in a number of files. More convenient for
editing, certainly, but less convenient for compiling. Now we must compile each
piece of source code individually and then link all the object files together.
If we edit some source, we must keep a track of which files to recompile and,
of course, relink. If we forget, we can be left wondering why the changes we
just made didn't have the effect we were expecting. Oh boy! It can give you a
headache. This is where make really starts to find its wings! Write a short
makefile and thereafter you don't have to worry about those things. As you
develop your code, just type make and bingo! Make will compile and link all and
only what is necessary, and you'll have a fully up-to-date executable every
time. Nirvana:)

Let's take a look at the second example:

```bash
cd ../example2
```

In Makefile, you will see that we've begun using variables. For example:

```make
OBJS=main.o math.o file.o
```

The variable OBJS contains a list of all the object files that will result from
the compilation and which we will link together to create the executable. For
flexibility we have put the name of the executable into a variable too.

The compilation rules are much as before, except we have one for each source
code file. We can make good use of our OBJS variable in the linking and
clean-up rules. For example, we have used:

```make
$(EXE): $(OBJS)
	gcc $(OBJS) -o $(EXE)
```

instead of:

```make
myprog.exe: main.o math.o file.o
	gcc main.o math.o file.o -o myprog.exe
```

In doing so, we have far less repetition and the rule will stand no matter how
many object files we have and what we call the executable.

## Exercise

Let's try it out now. Type make, followed by ./myprog.exe. Look at the chaining
from the all rule in the makefile and notice the order of events reported by
make. We can update the time-stamp on one of the source code files by typing:

```bash
touch math.c
```

Type make again now and see which files and recompiled and relinked. As before,
we can use make clean and make spotless to tidy up.

[Prev](basics.md) [Next](autovariables.md)
