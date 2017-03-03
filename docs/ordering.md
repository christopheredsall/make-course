[Prev](autovariables.md) [Next](conditionals.md)

# Ordering

Fortran90 is a popular language for scientific programming and for that reason
alone, we should include an example makefile which is used to compile and link
it. The use of modules in Fortran90 gives us reason to consider the order in
which the source code files are compiled and also the dependencies between
them. Let's take a look:

```bash
cd ../example8
```

You will see some Fortran90 files:
```
constants.f90  main.f90  utils.f90
```

and some makefiles:

```
first.mak  second.mak  third.mak
```
Note that I have included a symbolic link also:

```
Makefile -> third.mak
```

OK, enough of the listings, let's try the first makefile:

```bash
make -f first.mak
```

Using the gfortran compiler, I get the following error message:

```
gfortran -c utils.f90 -o utils.o
utils.f90:12.27:

    use constants, only: pi
                          1
Fatal Error: Can't open module file 'constants.mod' for reading at (1): No such file or directory
make: *** [utils.o] Error 1
```

If we take a look inside first.mak:

```make
OBJS=utils.o constants.o main.o
```

we see that utils.o is the first in the list of object files and so utils.f90
will be the first to be compiled--the ordering in lists in makefiles is
important. However, the routines in utils.f90 make use of the pi, included from
the constants module, e.g.:

```fortran
  real function surface_area_sphere(radius)

    use constants, only: pi
    implicit none
    real,intent(in) :: radius
    surface_area_sphere = 4.0 * pi * radius**2.0

  end function surface_area_sphere
```

and so the fortran compiler will look for a file called constants.mod--produced
as a side-effect of compiling constants.f90--when it is compiling utils.f90. In
this case, the .mod file for the required Fortran90 module was not found and we
got an error as a result. In order to correct this, we need to adjust the order
of compilation in the makefile. Take a look at the OBJS variable in second.mak
and try to compile again:

```make
OBJS=constants.o utils.o main.o
```

```bash
make -f second.mak
```
This time we get a clean compilation, in the right order:

```bash
gfortran -c constants.f90 -o constants.o
gfortran -c utils.f90 -o utils.o
gfortran -c main.f90 -o main.o
gfortran -o myprog.exe constants.o utils.o main.o
```

and an run the executable program, myprog.exe:

```
 Given a radius of (km):   6357.000    
 The surface area of the earth is (km^2):  5.0782483E+08
 and has a volume of (km^3):  1.0760809E+12
```

"Great!" we say, "we've cracked it!" Well yes, we can now compile correctly
from a clean start. However, recall that one of the great benefits of make is
that it can determine what parts of a program need to be recompiled if we have
made a change to the source code. That is a real bonus. Let's see if our
makefile can determine what work needs to be done. Let's refresh the date stamp
on constants.f90, simulating a code change:

```bash
touch constants.f90
```

and re-make:

```bash
make -f second.mak
```
The result is:

```bash
gfortran -c constants.f90 -o constants.o
gfortran -o myprog.exe constants.o utils.o main.o
```

constants.o is re-made and so is the executable. We're missing something,
however. Since utils.f90 contains the use constants statement, a change to
constants.f90 is very likely effect the behaviour of the routines and so
utils.o should also be re-made. The reason it was not re-made is because we
have a missing dependency. We need to include some more information in our
makefile. Take a look at third.mak. We have a new rule:

```make
utils.o : utils.f90 constants.f90
main.o : main.f90 utils.f90
```

When make chains through the rules, starting with:

```make
all: $(EXE)
```

It will examine all rules and their dependencies. It will find two rules with
the target utils.o--both equally valid. The rule we have just added extends the
dependencies to include both utils.f90 and constants.f90. If either of these
dependencies are newer than utils.o, all pertaining actions will be triggered
and the file re-made. Note our new rule is 'empty'. This is because we have all
the actions we need (i.e. to recompile utils.f90) already included in the
static pattern rule.

Some compilers will allow the creation of such dependency rules on-the-fly. You
can use the -M option with both gcc (for C files) and g95 (for Fortran) to
obtain dependcy rules, which you can re-direct to a file and subsequently
include in your makefile--see section Going_Further_and_the_Wider_Context.
gfortran will follow suit in an upcoming release.

This third makefile now contains all that we need. It has a symbolic link to
Makefile, so we can re-touch constants.f90, type make and all will be re-made
correctly.

[Prev](autovariables.md) [Next](conditionals.md)
