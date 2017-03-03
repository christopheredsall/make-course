[Prev](ordering.md) [Next](includes.md)

# If, Then and Else

Next up are conditionals. Sometimes its useful to say if this, do that,
otherwise, do something else and makefiles are no exception. For example, you
may want to compile your program under both Windows and Linux. It's quite
likely that your compile commands will be a little different in this case.
Using conditionals, you can write a makefile which you can use for both
operating systems. Let's take a look our next makefile:

```bash
cd ../example4
```

In the makefile we have a conditional based upon the value of the variable ARCH:

```make
# A conditional based on the value of the variable 'ARCH'
ifeq ($(ARCH),Win32)
  CC=ccWin32
  OBJ_EXT=obj
else
  CC=gcc
  OBJ_EXT=o
endif
```

Inside the conditional, we set the values of the variables CC (our C compiler
name) and OBJ_EXT according to whether we are building on Windows or Linux. We
can make use of the values of these variables in other assignments, e.g.:

```make
OBJS=main.$(OBJ_EXT) math.$(OBJ_EXT) file.$(OBJ_EXT)
```

where we will get object files named main.obj etc. on Windows and main.o etc.
on Linux. We can also embed conditionals in rules. For example, our compilation
rule this time looks like:

```make
$(OBJS): %.$(OBJ_EXT) : %.c defs.h
ifeq ($(ARCH),Linux)
	$(CC) -c $< -o $@
else
	@echo "This is what would happen on Windows"
	@echo $(CC) -c $< -o $@
	echo "Note what happens to an echo without the @"
endif
```

where we can choose the compiler, perhaps compiler flags etc. etc. based on the architecture we're using. Since we're not using Windows, I've added just echoed the action for Win32. Try setting ARCH to both Linux and Win32 and watch the results. Note the silencing effect of a leading @ on an action line.

[Prev](ordering.md) [Next](includes.md)
