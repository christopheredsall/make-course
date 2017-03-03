[Prev](recursive.md) [Next](resources.md)

# Doing Other things with Make

So far, we have looked at makefiles solely concerned with building libraries
and executables. Make is very flexible, however and compiling source code is
not the only task we can perform with it. If you are familiar with the
typesetting language LaTeX, for example, you can write a makefile to create
your presentation quality document from your set of typesetting source files.
Then, as you work on your document, it is a simple matter of typing make to
obtain the latest version of your PDF or Postscript file.

In the next example, we will look at 3 tasks which go hand-in-hand with, but
are distinct from, code development:

```bash
cd ../example7
```

If you use the Emacs editor, you can use a program called etags to index your code. Given this index, Emacs can jump you to the definition of a function, perhaps in a different source code file, with the click of a mouse. Very handy! To create the index we have a rule called tags:

```make
tags:
	@ if [ -f TAGS ] ; then \rm TAGS ; fi
	@ $(SHELL) -ec 'which etags > /dev/null; \
		etags -a -o TAGS *.c *.h'
```

To try it out. Type make tags and then open up main.c, say, in Emacs. Move the
cursor to a function name and use M-. ("meta-dot") to jump to the definition of
that function.

A crucial part of any software development process is testing. In a similar
vein, we can write a rule which automates code testing. This can be a huge boon
to a project as it makes testing very easy indeed and so encourages developers
to test often-a proven recipe for success and efficiency in software
development. Since the actions attached to a rule are arbitrary commands, we
can bundle them up into a shell script, if we like and simply call the shell
script in the rule:

```make
test: $(EXE)
	test.sh
```

Inside the script, we could setup our model, run it and compare the outputs to
a set of outputs we know to be correct. Put simply, test that the program is
doing what it is supposed to be doing. The sky is really the limit here, as we
can make our shell script as complex as we like. For example, I created a
testing suite for a climate model in this way.

Try make test now and note the dependency on the executable. If it is not built
(throw in a make spotless), the test rule will force compilation first, and
then test the results. Things get quite exciting when you keep your source code
inside a version control repository, such as that provided by [Subversion]. It
is a simple matter to write a cron job to first get a copy of the latest
version of the code and then to call make test. In this way, you can
automatically build and test your project every night. Computers love to do
these routine and sometimes mundane tasks! Nightly build and test suites are
particularly useful for collaborative projects and significantly reduce
debugging time, since recently created bugs are easier to find and fix.

Automatic documentation tools take appropriately formatted comments from your
source code and turn them into professional quality project manuals. It is well
established that the best way to keep documentation up-to-date (or let's be
honest, written at all!) is to include it in the same file as the source code
and to create it when you build the project. Two examples of automatic
documentation tools are [doxygen] and [naturaldocs]. You could make use of
these benefits using a make doc rule.

Indeed, if you use a Subversion, you can arrange for test and documentation
rules to be invoked each time a change is made to the source code repository.
We can see then, that the combination of a few relatively simple code
development tools can yield very significant benefits


# Exercise

Thinking about your research, is there anything you could automate with a makefile? Particularly where you might need to trasform one file type in to another. m
Try writing a proof of concept example (you don't need yor full software
environment, try using `echo` to stand in for other programs).

[Prev](recursive.md) [Next](resources.md)
