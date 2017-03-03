[Prev](variables.md) [Next](ordering.md)

# Automatic Variables

Now, you will no doubt have spotted some patterns in the previous makefile. For
example, the actions to compile the source code files were essentially the same
for each of the files. Through the use of some wonderfully esoteric character
sequences, we can streamline our makefile a good deal. We'll learn to love
these ugly ducklings. Let's see how:

```bash
cd ../example3
```

Looking at the makefile, we see can see some funky character sequences. Working
down from the top, we see that the variable assignments are unchanged from last
time. We do see a change in the all rule, however:

```make
$(EXE): $(OBJS)
	gcc $^ -o $@
```

The target and dependencies are fine, but what's happened to the action part?
We've made use of some automatic variables in the shape of $^ and $@. These are
rather useful and we'll start to see automatic variables cropping up quite
often. $^ takes the value of all the dependencies of a rule and $@ takes the
value of the target. They are used in the next rule, and allow us to make it
rather general purpose; hugely reducing the need for makefile maintenance,
should our build change. Let's see how. The second rule:

```make
$(OBJS): %.o : %.c defs.h
	gcc -c $< -o $@
```

What on earth is that?!. I know. I said they were queer fish, but it's OK,
don't panic. This is the format for a static pattern rule. You'll love these
pups. Honest. Let's calmly walk through the rule.

We have an extra colon on the top line. Before the first colon is a list of targets to which the rule will apply. It will apply to each target in the list in turn. We have used the list of object files--main.o, math.o and file.o. So, the first target will be main.o, the second will be math.o and so on. Next--between the first and second colon--is the target-pattern. This is matched against each target in turn so as to extract part of the target name, called the stem. The stem is used to create dependency-pattern corresponding to each target. We can summarise the general form of a static pattern rule as:

```make
targets.. : target-pattern : dependency-pattern(s) other-deps..
	actions
```

For our particular rule, the first target is main.o. The target-pattern %.o is
matched to this, giving a stem of main. The dependency-pattern, %.c, becomes
main.c, as a consequence. Neat! Given some automatic variables, we can now
write a single rule which will compile all our source code files. The variable
$< has the value of the first dependency only.

## Exercise

Let's see how all this works in practice. Type make, and pay attention to each
instantiation of the rule as it is echoed to the screen.

[Prev](variables.md) [Next](ordering.md)
