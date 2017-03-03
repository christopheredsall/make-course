[Prev](includes.md) [Next](other.md)

# Recursive Makefiles

In the previous example we looked at breaking up our makefiles into smaller,
more manageable parts, as our projects grow. The include statement is useful
for makefiles in the same directory, but sometimes it's convenient to split our
projects over several directories, what then? In this case you can invoke a
sub-make process from inside the first makefile, i.e. call make--perhaps after
moving to a different directory--as part of the actions of a rule. In this way,
we can cascade down to tree of directories, calling make appropriately as we
go. I have found this approach to be useful, but it would be amiss of me not to
point out a paper which was written to highlight some of the dangers of this
approach: ["Recursive Make Considered Harmful"](http://aegis.sourceforge.net/auug97.pdf)

OK, Let's look our next example:

```bash
cd ../example6
```

In this makefile, we see a healthy swathe of variables at the top of the file,
including a set pointing to a subdirectory and the name of a library of
routines that we wish to create inside:

```make
LIBDIR=mylibdir
LIBNAME=libfuncs.a
LIBFULLNAME=$(LIBDIR)/$(LIBNAME)
```

It is very convenient to package up the files for a third party library--or one
of our own--in such a subdirectory. We will build the library in that directory
(mylibdir in this case) as part of the top-level build and link to it
accordingly.

Inside main.c we include the library header file and so we augment the include
path accordingly in the compilation rule:

```make
$(OBJS): %.o : %.c defs.h $(LIBDIR)/func.h
	gcc -I$(LIBDIR) -c $< -o $@
```

The link rule has a dependency upon the compiled library:

```make
$(EXE): $(OBJS) $(LIBFULLNAME)
	gcc $^ -o $@
```

and we have a matching target for the rule which will call our sub-make:

```make
$(LIBFULLNAME):
	\cd $(LIBDIR); $(MAKE) $(LIBNAME)
```

We have an automatic variable called $(MAKE) available, which takes the value
of the make command we used to invoke the top-level process. Typically this is
simply make, but the variable is useful if, for instance, GNU make is installed
on your system as gmake. Note that we are explicitly calling a target in the
sub-makefile with the value of $(LIBNAME). This is not strictly necessary, but
can aid clarity, and we must know the name of the library at link-time in any
case.

The clean-up rules also use a recursive call, to request clean-up in the
library sub-directory. For example:

```make
clean:
	\rm -f $(OBJS)
	\cd $(LIBDIR); $(MAKE) clean
```
The makefile in mylibdir is familiar and largely autonomous:

```make
OBJS=func.o

all: $(LIBNAME)

$(LIBNAME): $(OBJS)
	ar rcvs $@ $^

$(OBJS) : %.o : %.c func.h
	gcc $(CPPFLAGS) -c $< -o $@

.PHONY: clean spotless

clean:
	\rm -f $(OBJS)

spotless:
	\rm -f $(OBJS) $(LIBNAME)
```

We compile the object files and then create an archive of them in the
$(LIBNAME) rule.

I say largely autonomous. Indeed, we could arrange for our sub-makefile to be
independent of anything else. In this case, however, I have made use of the
variable $(CPPFLAGS). This is not defined in the sub-makefile and so would be
empty were it not for the export statement in the top-level makefile. This has
the effect of exporting the value of all variables to any sub-make files. Try
changing the value of $(CPPFLAGS) and see the effect upon the overall program.


[Prev](includes.md) [Next](other.md)
