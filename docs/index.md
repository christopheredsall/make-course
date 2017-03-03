[Next](hello.md)

# Building Software With Make

## Introduction

The purpose of `make` is to automate the process of turning source code (the
human readable instructions) in to executables (the binary files that the
operating system can run on a particular processor architecture).

You can invoke the compiler to compile and link a single source file by hand
and this is simple and feasible, however more complex projects with many source
files, librarys and other build artifacts require a more automated solution. In
particular if one file is changed it might not be necessary to rebuild all the
file, saving time.  This is where build tools like `make` come in. Even large
integrated development environments like Visual Studio and Xcode and tend to
call their platform's versions of make in the background (`nmake.exe` and GNU
`make` respectively).

In a research context, together with source control, using a build system
ensures reproducability of results.

## History

`make` is a venerable program, it has been distribued with Unix as part of the
programmer's work bench since 1976. It generalised and improved a numbe of
ad-hoc build and install scripts.

The GNU project's version of `make` was first released in 1988. 

[Next](hello.md)

