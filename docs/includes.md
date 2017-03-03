[Prev](conditionals.md) [Next](recursive.md)

# Including other Makefiles

We noted earlier that as a project grows we inevitably end up splitting the
source code over a number of files. This can also be true for your makefiles.
Make provides a simple mechanism for doing this. Using the include statement,
you can embed the contents of one makefile inside another. In this way we can
keep the size of any individual makefile to manageable proportions. One word of
caution, however. You'll remember that make reads down from the top of a
makefile and will try to execute the first rule it encounters by default. Since
make takes the contents of one makefile and drops it directly into another, we
must be careful about the order in which we include things. We could
accidentally include a rule from another makefile before our intended default
rule-conventionally the one named all. Let's take a look at our next example:

```bash
cd ../example5
```

In this case, we have an include statement right at the top of the makefile:

```make
include macros.mak
```

Inside macros.mak, we have only variable assignments and so we are safe to
include it at the top of the main makefile:

```make
OBJS=main.o math.o file.o
EXE=myprog.exe
```

We have another include statement at the end of the file:

```make
include clean.mak
```

We have grouped all the rules relating to the tidy-up into clean.mak:

```make
.PHONY: clean spotless

clean:
	\rm -f $(OBJS)

spotless:
	\rm -f $(OBJS) $(EXE)
```

but since we included it after the first (all) rule, we are safe to do this.
Notice that clean.mak contains references to variables only defined in
macros.mak. Since both macros.mak and clean.mak are effectively cut and pasted
into the file Makefile, make reports no errors and everything proceeds
smoothly.

In previous examples, we have called make with all the defaults, i.e. it will
look for a file called Makefile and try to execute the first rule (all) by
default. We've also seen how we can invoke a specific target, such as calling
make clean. We can also pass a non-default makefile to make. For example we
could type:

```bash
make -f clean.mak clean
```

or indeed,

```bash
make -f clean.mak
```

In this case, we are invoking the clean rule directly from the fragment of
makefile in clean.mak (by virtue of it being the first rule in the latter
case). From the output, you can see that the variables OBJS and EXE are empty
in this case and that the rules only make sense when embedded in the composite.


[Prev](conditionals.md) [Next](recursive.md)
