---
title: "Loops"
teaching: 40
exercises: 10
questions:
- "How can I perform the same actions on many different files?"
objectives:
- "Write a loop that applies one or more commands separately to each file in a set of files."
- "Trace the values taken on by a loop variable during execution of the loop."
- "Explain the difference between a variable's name and its value."
- "Explain why spaces and some punctuation characters shouldn't be used in file names."
keypoints:
- "A `for` loop repeats commands once for every thing in a list."
- "Every `for` loop needs a variable to refer to the thing it is currently operating on."
- "Use `$name` to expand a variable (i.e., get its value). `${name}` can also be used."
- "Do not use spaces, quotes, or wildcard characters such as '*' or '?' in filenames, as it complicates variable expansion."
- "Give files consistent names that are easy to match with wildcard patterns to make it easy to select them for looping."
- "Use the up-arrow key to scroll up through previous commands to edit and repeat them."
---

**Loops** are a programming construct which allow us to repeat a command or set of commands
for each item in a list.
As such they are key to productivity improvements through automation.
Similar to wildcards and tab completion, using loops also reduces the
amount of typing required (and hence reduces the number of typing mistakes).

### loop syntax

This is what a _for_ loop looks like in bash.

![loop-anatomy](../fig/loop-anatomy.png)

It starts with the keyword `for`. Next you choose a name for the loop variable.  Here we use `colors`. then another keyword `in` and lastly a list of space delimited items to loop over.

Now the loop is setup and we're ready to begin looping. The keyword `do` starts the loop.  The line(s) between `do` and `done` will be run as many times as you have items.  In our example code we have four colors so the loop will be run four times.  the keyword `done` closes the loop.

So let's try writing and running this loop:

~~~
for colors in brown blue yellow green
do
echo $colors
done
~~~
{: .language-bash}
 
The `do` keyword marks the beginning of the loop.

With each iteration of the loop the value of the `$colors` variable is set to the next item from the list.

In the third line of our loop we're using `echo` to output the current value of our `$colors` variable to the screen.  

The `done` keyword signals the end of the loop and tells the shell to repeat the loop with the next item assigned to the `$colors` variable.

> ## Follow the Prompt
>
> The shell prompt changes from `$` to `>` and back again as we were
> typing in our loop. The second prompt, `>`, is different to remind
> us that we haven't finished typing a complete command yet. A semicolon, `;`,
> can be used to separate two commands written on a single line.
{: .callout}

When the shell sees the keyword `for`,
it knows to repeat a command (or group of commands) once for each item in a list.
Each time the loop runs (called an iteration), an item in the list is assigned in sequence to
the **variable**, and the commands inside the loop are executed, before moving on to the next item in the list.
Inside the loop, we call for the variable's value by putting `$` in front of it.
The `$` tells the shell interpreter to treat
the variable as a variable name and substitute its value in its place,
rather than treat it as text or an external command.

In this example, the list is our four colors: `brown`, `blue`, `yellow`, and `green`.
Each time the loop iterates, it will assign sucessive colors to the variable `$colors`.  
The first time through the loop,
`$colors` is `brown`.
The interpreter runs the command `echo $colors` generating our first line of output.
For the second iteration, `$colors` becomes
`blue`. This time, the shell again runs `echo $colors` but the value of our `$colors` variable has now changed to "blue".

The process is repeated for the third and fourth iteration of the loop.
Since the list was only four items, the shell then exits the `for` loop.

> ## Same Symbols, Different Meanings
>
> Here we see `>` being used a shell prompt, whereas `>` is also
> used to redirect output.
> Similarly, `$` is used as a shell prompt, but, as we saw earlier,
> it is also used to ask the shell to get the value of a variable.
>
> If the *shell* prints `>` or `$` then it expects you to type something,
> and the symbol is a prompt.
>
> If *you* type `>` or `$` yourself, it is an instruction from you that
> the shell should redirect output or get the value of a variable.
{: .callout}

We have called the variable in this loop `colors`
in order to make its purpose clearer to human readers.
The shell itself doesn't care what the variable is called;
if we wrote this loop as:

~~~
for x in brown blue yellow green
do
echo $x
done
~~~
{: .language-bash}

it would work exactly the same way.
*Don't do this.*
Programs are only useful if people can understand them,
so meaningless names (like `x`) increase the odds that the program won't do what its readers think it does.

> ## Variables in Loops
>
> This exercise refers to the `data-shell/molecules` directory.
> `ls` gives the following output:
>
> ~~~
> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> ~~~
> {: .output}
>
> What is the output of the following code?
>
> ~~~
> $ for datafile in *.pdb
> > do
> >    ls *.pdb
> > done
> ~~~
> {: .language-bash}
>
> Now, what is the output of the following code?
>
> ~~~
> $ for datafile in *.pdb
> > do
> >	ls $datafile
> > done
> ~~~
> {: .language-bash}
>
> Why do these two loops give different outputs?
>
> > ## Solution
> > The first code block gives the same output on each iteration through
> > the loop.
> > Bash expands the wildcard `*.pdb` within the loop body (as well as
> > before the loop starts) to match all files ending in `.pdb`
> > and then lists them using `ls`.
> > The expanded loop would look like this:
> > ```
> > $ for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > > do
> > >	ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > > done
> > ```
> > {: .language-bash}
> >
> > ```
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > ```
> > {: .output}
> >
> > The second code block lists a different file on each loop iteration.
> > The value of the `datafile` variable is evaluated using `$datafile`,
> > and then listed using `ls`.
> >
> > ```
> > cubane.pdb
> > ethane.pdb
> > methane.pdb
> > octane.pdb
> > pentane.pdb
> > propane.pdb
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## Limiting Sets of Files
>
> What would be the output of running the following loop in the `data-shell/molecules` directory?
>
> ~~~
> $ for filename in c*
> > do
> >    ls $filename
> > done
> ~~~
> {: .language-bash}
>
> 1.  No files are listed.
> 2.  All files are listed.
> 3.  Only `cubane.pdb`, `octane.pdb` and `pentane.pdb` are listed.
> 4.  Only `cubane.pdb` is listed.
>
> > ## Solution
> > 4 is the correct answer. `*` matches zero or more characters, so any file name starting with
> > the letter c, followed by zero or more other characters will be matched.
> {: .solution}
>
> How would the output differ from using this command instead?
>
> ~~~
> $ for filename in *c*
> > do
> >    ls $filename
> > done
> ~~~
> {: .language-bash}
>
> 1.  The same files would be listed.
> 2.  All the files are listed this time.
> 3.  No files are listed this time.
> 4.  The files `cubane.pdb` and `octane.pdb` will be listed.
> 5.  Only the file `octane.pdb` will be listed.
>
> > ## Solution
> > 4 is the correct answer. `*` matches zero or more characters, so a file name with zero or more
> > characters before a letter c and zero or more characters after the letter c will be matched.
> {: .solution}
{: .challenge}



> ## Spaces in Names
>
> Spaces are used to separate the elements of the list
> that we are going to loop over. If one of those elements
> contains a space character, we need to surround it with
> quotes, and do the same thing to our loop variable.
> Suppose our data files are named:
>
> ~~~
> red dragon.dat
> purple unicorn.dat
> ~~~
> {: .source}
>
> To loop over these files, we would need to add double quotes like so:
>
> ~~~
> $ for filename in "red dragon.dat" "purple unicorn.dat"
> > do
> >     head -n 100 "$filename" | tail -n 20
> > done
> ~~~
> {: .language-bash}
>
> It is simpler to avoid using spaces (or other special characters) in filenames.
>
> The files above don't exist, so if we run the above code, the `head` command will be unable
> to find them, however the error message returned will show the name of the files it is
> expecting:
> ```
> head: cannot open ‘red dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple unicorn.dat’ for reading: No such file or directory
> ```
> {: .output}
> Try removing the quotes around `$filename` in the loop above to see the effect of the quote
> marks on spaces. Note that we get a result from the loop command for unicorn.dat when we run this code in the `creatures` directory:
> ```
> head: cannot open ‘red’ for reading: No such file or directory
> head: cannot open ‘dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple’ for reading: No such file or directory
> CGGTACCGAA
> AAGGGTCGCG
> CAAGTGTTCC
> ```
> {: . output}
{: .callout}


## Nelle's Pipeline: Processing Files

Nelle is now ready to process her data files using `goostats` --- a shell script written by her supervisor.
This calculates some statistics from a protein sample file, and takes two arguments:

1. an input file (containing the raw data)
2. an output file (to store the calculated statistics)

Since she's still learning how to use the shell,
she decides to build up the required commands in stages.
Her first step is to make sure that she can select the right input files --- remember,
these are ones whose names end in 'A' or 'B', rather than 'Z'. Starting from her home directory, Nelle types:

~~~
$ cd north-pacific-gyre/2012-07-03
$ for datafile in NENE*[AB].txt
> do
>     echo $datafile
> done
~~~
{: .language-bash}

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
~~~
{: .output}

Her next step is to decide
what to call the files that the `goostats` analysis program will create.
Prefixing each input file's name with 'stats' seems simple,
so she modifies her loop to do that:

~~~
$ for datafile in NENE*[AB].txt
> do
>     echo $datafile stats-$datafile
> done
~~~
{: .language-bash}

~~~
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
~~~
{: .output}

She hasn't actually run `goostats` yet,
but now she's sure she can select the right files and generate the right output filenames.

Typing in commands over and over again is becoming tedious,
though,
and Nelle is worried about making mistakes,
so instead of re-entering her loop,
she presses <kbd>↑</kbd>.
In response,
the shell redisplays the whole loop on one line
(using semi-colons to separate the pieces):

~~~
$ for datafile in NENE*[AB].txt; do echo $datafile stats-$datafile; done
~~~
{: .language-bash}

Using the left arrow key,
Nelle backs up and changes the command `echo` to `bash goostats`:

~~~
$ for datafile in NENE*[AB].txt; do bash goostats $datafile stats-$datafile; done
~~~
{: .language-bash}

When she presses <kbd>Enter</kbd>,
the shell runs the modified command.
However, nothing appears to happen --- there is no output.
After a moment, Nelle realizes that since her script doesn't print anything to the screen any longer,
she has no idea whether it is running, much less how quickly.
She kills the running command by typing <kbd>Ctrl</kbd>+<kbd>C</kbd>,
uses <kbd>↑</kbd> to repeat the command,
and edits it to read:

~~~
$ for datafile in NENE*[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
~~~
{: .language-bash}

> ## Beginning and End
>
> We can move to the beginning of a line in the shell by typing <kbd>Ctrl</kbd>+<kbd>A</kbd>
> and to the end using <kbd>Ctrl</kbd>+<kbd>E</kbd>.
{: .callout}

When she runs her program now,
it produces one line of output every five seconds or so:

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~
{: .output}

1518 times 5 seconds,
divided by 60,
tells her that her script will take about two hours to run.
As a final check,
she opens another terminal window,
goes into `north-pacific-gyre/2012-07-03`,
and uses `cat stats-NENE01729B.txt`
to examine one of the output files.
It looks good,
so she decides to get some coffee and catch up on her reading.


> When using variables it is also possible to put the names into curly braces to clearly delimit the variable name: `$colors` is equivalent to `${colors}`. You may find this notation in other people's programs.
> {: .note}


> ## Doing a Dry Run
>
> A loop is a way to do many things at once --- or to make many mistakes at
> once if it does the wrong thing. One way to check what a loop *would* do
> is to `echo` the commands it would run instead of actually running them.
>
> Suppose we want to preview the commands the following loop will execute
> without actually running those commands:
>
> ~~~
> $ for datafile in *.pdb
> > do
> >   cat $datafile >> all.pdb
> > done
> ~~~
> {: .language-bash}
>
> What is the difference between the two loops below, and which one would we
> want to run?
>
> ~~~
> # Version 1
> $ for datafile in *.pdb
> > do
> >   echo cat $datafile >> all.pdb
> > done
> ~~~
> {: .language-bash}
>
> ~~~
> # Version 2
> $ for datafile in *.pdb
> > do
> >   echo "cat $datafile >> all.pdb"
> > done
> ~~~
> {: .language-bash}
>
> > ## Solution
> > The second version is the one we want to run.
> > This prints to screen everything enclosed in the quote marks, expanding the
> > loop variable name because we have prefixed it with a dollar sign.
> >
> > The first version appends the output from the command `echo cat $datafile` 
> > to the file, `all.pdb`. This file will just contain the list; 
> > `cat cubane.pdb`, `cat ethane.pdb`, `cat methane.pdb` etc.
> >
> > Try both versions for yourself to see the output! Be sure to open the
> > `all.pdb` file to view its contents.
> {: .solution}
{: .challenge}

> ## Nested Loops
>
> Suppose we want to set up a directory structure to organize
> some experiments measuring reaction rate constants with different compounds
> *and* different temperatures.  What would be the
> result of the following code:
>
> ~~~
> $ for species in cubane ethane methane
> > do
> >     for temperature in 25 30 37 40
> >     do
> >         mkdir $species-$temperature
> >     done
> > done
> ~~~
> {: .language-bash}
>
> > ## Solution
> > We have a nested loop, i.e. contained within another loop, so for each species
> > in the outer loop, the inner loop (the nested loop) iterates over the list of
> > temperatures, and creates a new directory for each combination.
> >
> > Try running the code for yourself to see which directories are created!
> {: .solution}
{: .challenge}
