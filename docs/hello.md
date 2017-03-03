[Prev](index.md) [Next](basics.md)

# Building Hello World

As is traditional in software textbooks, we will begin by compiling what is
almost the simplest possible program. This is to make sure all the rest of the
tools are set up correctly.

Change in to the directory for the first exercise and list the files, you
should see the source code and a file called `Makefile`

```bash
[trainXX@newblue1 examples]$ cd example1
[trainXX@newblue1 example1]$ ls
Makefile	hello_world.c
```

The `Makefile` contains the "recipe" for building the executable. (The name
"Makefile" is conventional and the defualt we could use a different name if
told make about with th `-f` flag). To build the executable, run the command
`make`. It will print out the commands it runs as it goes along:

```bash
[trainXX@newblue1 example1]$ make
gcc -c hello_world.c -o hello_world.o
gcc hello_world.o -o hello_world.exe
```

Now if you list the directory you will see there are two new files, an object
file `hello_world.o` and and executable called `hello_world.exe`.

```bash
[trainXX@newblue1 example1]$ ls
Makefile	hello_world.c	hello_world.exe	hello_world.o
```

Run `hello_world.exe` to see the traditional greeting:

```bash
[trainXX@newblue1 example1]$ ./hello_world.exe 
Hello, World
``` 

The `Makefile` you used here can also do some other things. You can clean up
the intermediate object file with the clean "target". To see what `make` will
do give it the `-n` (or equivalent `--dry-run` flag):

```
[trainXX@newblue1 example1]$ make -n clean
\rm -f hello_world.o
```

Here, the leading backslash in front of the `rm` temporarily undoes any ailis
that might be applied to `rm`. Go ahead and run that without the `-n` to do it
for real.

This makefile has another target to remove the executable as well, use the `-n`
flag to check what `make spotless` will do. We are free to choose any target
name we like. In opensource rojects you will often see a `distclean` target
that does the same thing. Go ahead and use `make` to remove the execuatble as
well.

## Exercise

Edit the source code `hello_world.c` using your preferred editor (e.g. `nano`)
so that it prints a different message. Rebuild the execuatable and rerun it to
verify the message has changed.

[Prev](index.md) [Next](basics.md)
