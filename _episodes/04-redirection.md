---
title: "Redirection"
teaching: 30
exercises: 15
questions:
- "How can I search within files?"
- "How can I combine existing commands to do new things?"
objectives:
- "Employ the `grep` command to search for information within files."
- "Print the results of a command to a file."
- "Construct command pipelines with two or more stages."
keypoints:
- "`grep` is a powerful search tool with many options for customization."
- "`>`, `>>`, and `|` are different ways of redirecting output."
- "`command > file` redirects a command's output to a file."
- "`command >> file` redirects a command's output to a file without overwriting the existing contents of the file."
- "`command_1 | command_2` redirects the output of the first command as input to the second command."
- "for loops are used for iteration."
- "`basename` gets rid of repetitive parts of names."
---

## Searching files

We discussed in a previous episode how to search within a file using `less`. We can also
search within files without even opening them, using `grep`. `grep` is a command-line
utility for searching plain-text files for lines matching a specific set of 
characters (sometimes called a string) or a particular pattern 
(which can be specified using something called regular expressions). We're not going to work with 
regular expressions in this lesson, and are instead going to specify the strings 
we are searching for.
Let's give it a try!

> ## Indicator codes in WDI
> 
> The World Bank names its economic and social indicators according to a [systematic coding convention](https://datahelpdesk.worldbank.org/knowledgebase/articles/201175-how-does-the-world-bank-code-its-indicators). For example, "GDP per capita, PPP (constant 2011 international $)" is called `NY.GDP.PCAP.PP.KD`. The codes stand for "national accounts, income", "GDP", "per capita", "at purchasing power parity", "in constant dollars." 
> 
{: .callout}

We'll search for indicators inside of our CSV files. Let's first make sure we are in the correct 
directory:

~~~
$ cd ~/Downloads/shell-economics/data/raw/worldbank/
~~~
{: .bash}

Suppose we want to see how many reads in our file have really bad segments containing 10 consecutive unknown nucleotides (Ns).

Let's search for the string `NY.GDP.PCAP.PP.KD` in `WDIData.csv`:
~~~
$ grep NY.GDP.PCAP.PP.KD WDIData.csv
~~~
{: .bash}

This command returns a lot of output to the terminal. Every single line in the WDIData.csv 
file that contains information about GDP per capita (as defined above) is printed to the terminal, regardless of how long or short the file is. 

> ## Exercise
>
> Search for the indicator `SP.POP.TOTL` (corresponding to total population) in the `WDIData.csv` file.
> 
> 
> > ## Solution  
> > `grep SP.POP.TOTL WDIData.csv` 
> {: .solution}
{: .challenge}

## Redirecting output

`grep` allowed us to identify lines in our CSV files that match a particular pattern. 
All of these lines were printed to our terminal screen, but in order to work with and perform other operations on them, we will need to capture that output in some
way. 

We can do this with something called "redirection". The idea is that
we are taking what would ordinarily be printed to the terminal screen and redirecting it to another location. 
In our case, we want to print this information to a file so that we can look at it later and 
use other commands to analyze this data.

The command for redirecting output to a file is `>`.

Let's try out this command and copy all the records 
in WDI that contain 
`NY.GDP.PCAP.PP.KD` to another file called `gdp_per_capita.csv`.

~~~
$ grep NY.GDP.PCAP.PP.KD WDIData.csv > gdp_per_capita.csv
~~~
{: .bash}

The prompt should sit there a little bit, and then it should look like nothing
happened. But type `ls`. You should see a new file called `gdp_per_capita.csv`. 

We can check the number of lines in our new file using a command called `wc`. 
`wc` stands for **word count**. This command counts the number of words, lines, and characters
in a file. 

~~~
$ wc gdp_per_capita.csv
~~~
{: .bash}

~~~
     264    2345  175056 gdp_per_capita.csv
~~~
{: .output}

This will tell us the number of lines, words and characters in the file. If we
want only the number of lines, we can use the `-l` flag for `lines`.

~~~
$ wc -l gdp_per_capita.csv
~~~
{: .bash}

~~~
264 gdp_per_capita.csv
~~~
{: .output}

> ## Exercise
>
> How many rows in `WDIData.csv` contain GDP per capita data in current national currency (`NY.GDP.PCAP.CN`)?
>
>> ## Solution
>>  
>>
>> ~~~
>> $ grep NY.GDP.PCAP.CN WDIData.csv > gdp_per_capita.csv
>> $ wc -l gdp_per_capita.csv
>> ~~~
>> {: .bash}
>> 
>> ~~~
>> 264
>> ~~~
>> {: .output}
>> Note that we did not check whether these rows actually contain data.
> {: .solution}
{: .challenge}

We might want to search multiple indicators in multiple files.
However, we need to be careful, because each time we use the `>` command to redirect output
to a file, the new output will replace the output that was already present in the file. 
You want to avoid overwriting your data files.

~~~
$ grep NY.GDP.PCAP.PP.KD WDISeries.csv  > gdp_per_capita.csv 
$ wc -l gdp_per_capita.csv
~~~
{: .bash}

~~~
1 gdp_per_capita.csv
~~~
{: .output}

~~~
$ grep NY.GDP.PCAP.PP.KD WDISeries-Time.csv  > gdp_per_capita.csv
$ wc -l gdp_per_capita.csv
~~~
{: .bash}

~~~
0 gdp_per_capita.csv
~~~
{: .output}

Here, the output of our second  call to `wc` shows that we no longer have any lines in our `gdp_per_capita.csv` file. This is 
because the second file we searched (`WDISeries-Time.csv`) does not contain any lines that match our
search pattern. So our file was overwritten and is now empty.

We can avoid overwriting our files by using the command `>>`. `>>` is known as the "append redirect" and will 
append new output to the end of a file, rather than overwriting it.

~~~
$ grep NY.GDP.PCAP.PP.KD WDISeries.csv >> gdp_per_capita.csv
$ wc -l gdp_per_capita.csv
~~~
{: .bash}

~~~
1 gdp_per_capita.csv
~~~
{: .output}

~~~
$ grep NY.GDP.PCAP.PP.KD WDISeries-Time.csv >> gdp_per_capita.csv
$ wc -l gdp_per_capita.csv
~~~
{: .bash}

~~~
1 gdp_per_capita.csv
~~~
{: .output}

The output of our second call to `wc` shows that we have not overwritten our original data. 

We can also do this with a single line of code by using a wildcard: 

~~~
$ grep NY.GDP.PCAP.PP.KD WDI*.csv > gdp_per_capita.csv
$ wc -l gdp_per_capita.csv
~~~
{: .bash}

~~~
286 gdp_per_capita.csv
~~~
{: .output}

Since we might have multiple different criteria we want to search for, 
creating a new output file each time has the potential to clutter up our workspace. We also
thus far haven't been interested in the actual contents of those files, only in the number of 
reads that we've found. We created the files to store the reads and then counted the lines in 
the file to see how many reads matched our criteria. There's a way to do this, however, that
doesn't require us to create these intermediate files - the pipe command (`|`).

This is probably not a key on
your keyboard you use very much, so let's all take a minute to find that key. 
What `|` does is take the output that is
scrolling by on the terminal and uses that output as input to another command. 
When our output was scrolling by, we might have wished we could slow it down and
look at it, like we can with `less`. Well it turns out that we can! We can redirect our output
from our `grep` call through the `less` command.

~~~
$ grep NY.GDP.PCAP.PP.KD WDIData.csv | less
~~~
{: .bash}

We can now see the output from our `grep` call within the `less` interface. We can use the up and down arrows 
to scroll through the output and use `q` to exit `less`.

If we don't want to create a file before counting lines of output from our `grep` search, we could directly pipe
the output of the grep search to the command `wc -l`. This can be helpful for investigating your output if you are not sure
you would like to save it to a file. 

~~~
$ grep NY.GDP.PCAP.PP.KD WDIData.csv | wc -l 
~~~
{: .bash}

Redirecting output is often not intuitive, and can take some time to get used to. Once you're 
comfortable with redirection, however, you'll be able to combine any number of commands to
do all sorts of exciting things with your data!

None of the command line programs we've been learning
do anything all that impressive on their own, but when you start chaining
them together, you can do some really powerful things very
efficiently. 



## Writing for loops

Loops are key to productivity improvements through automation as they allow us to execute commands repeatedly. 
Similar to wildcards and tab completion, using loops also reduces the amount of typing (and typing mistakes). 
Loops are helpful when performing operations on groups of sequencing files, such as unzipping or trimming multiple
files. We will use loops for these purposes in subsequent analyses, but will cover the basics of them for now.

When the shell sees the keyword `for`, it knows to repeat a command (or group of commands) once for each item in a list. 
Each time the loop runs (called an iteration), an item in the list is assigned in sequence to the **variable**, and 
the commands inside the loop are executed, before moving on to the next item in the list. Inside the loop, we call for 
the variable's value by putting `$` in front of it. The `$` tells the shell interpreter to treat the **variable**
as a variable name and substitute its value in its place, rather than treat it as text or an external command. In shell programming, this is usually called "expanding" the variable.

Sometimes, we want to expand a variable without any whitespace to its right.
Suppose we have a variable named `foo` that contains the text `abc`, and would
like to expand `foo` to create the text `abcEFG`.

~~~
$ foo=abc
$ echo foo is $foo
foo is abc
$ echo foo is $fooEFG      # doesn't work
foo is
~~~
{: .bash}

The interpreter is trying to expand a variable named `fooEFG`, which (probably)
doesn't exist. We can avoid this problem by enclosing the variable name in 
braces (`{` and `}`, sometimes called "squiggle braces").

~~~
$ foo=abc
$ echo foo is $foo
foo is abc
$ echo foo is ${foo}EFG      # now it works!
foo is abcEFG
~~~
{: .bash}

Let's write a for loop to show us the first two lines of the CSV files in the `worldbank` folder. You will notice the shell prompt changes from `$` to `>` and back again as we were typing in our loop. The second prompt, `>`, is different to remind us that we haven’t finished typing a complete command yet. A semicolon, `;`, can be used to separate two commands written on a single line.

~~~
$ for filename in *.csv
> do
> head -n 2 ${filename}
> done
~~~
{: .bash}

The for loop begins with the formula `for <variable> in <group to iterate over>`. In this case, the word `filename` is designated 
as the variable to be used over each iteration. In our case `WDICountry-Series.csv`, `WDICountry.csv`, `WDIData.csv`, `WDIFootNote.csv`, `WDISeries-Time.csv`, `WDISeries.csv`, and `gdp_per_capita.csv`
will be substituted for `filename` 
because they fit the pattern of ending with .csv in the directory we've specified. The next line of the for loop is `do`. The next line is 
the code that we want to execute. We are telling the loop to print the first two lines of each variable we iterate over. Finally, the
word `done` ends the loop.

After executing the loop, you should see the first two lines of both fastq files printed to the terminal. Let's create a loop that 
will save this information to a file.

~~~
$ for filename in *.csv
> do
> head -n 2 ${filename} >> wdi_headers.txt
> done
~~~
{: .bash}

Note that we are using `>>` to append the text to our `wdi_headers.txt` file. If we used `>`, the `wdi_headers.txt` file would be rewritten
every time the loop iterates, so it would only have text from the last variable used. Instead, `>>` adds to the end of the file.

## Using Basename in for loops
Basename is a function in UNIX that is helpful for removing a uniform part of a name from a list of files. In this case, we will use basename to remove the `.csv` extension from the files that we’ve been working with. 

~~~
$ basename WDICountry.csv .csv
~~~
{: .bash}

We see that this returns

~~~
WDICountry
~~~
{: .output}

If we try the same thing but use `.xls` as the file extension instead, nothing happens. This is because basename only works when it exactly matches a string in the file.

~~~
$ basename WDICountry.csv .xls
~~~
{: .bash}

~~~
WDICountry.csv
~~~
{: .output}

Basename is really powerful when used in a for loop. It allows to access just the file prefix, which you can use to name things. Let's try this.

Inside our for loop, we create a new name variable. We call the basename function inside the parenthesis, then give our variable name from the for loop, in this case `${filename}`, and finally state that `.csv` should be removed from the file name. It’s important to note that we’re not changing the actual files, we’re creating a new variable called name. The line `> echo $name` will print to the terminal the variable name each time the for loop runs. Because we are iterating over two files, we expect to see two lines of output.

~~~
$ for filename in *.csv
> do
> name=$(basename ${filename} .csv)
> echo ${name}
> done
~~~
{: .bash}



> ## Exercise
>
> Print the file prefix of all of the `.md` (markdown) files in `~/Downloads/shell-economics`.
>
>> ## Solution
>>  
>>
>> ~~~
>> $ cd ~/Downloads/shell-economics
>> $ for filename in *.md
>> > do
>> > name=$(basename ${filename} .md)
>> > echo ${name}
>> > done
>> ~~~
>> {: .bash}
>>
> {: .solution}
{: .challenge}

One way this is really useful is to move files. Let's rename all of our .md files using `mv` so that they have the years on them, which will document when we created them. 

~~~
$ for filename in *.md
> do
> name=$(basename ${filename} .md)
> mv ${filename}  ${name}-2019.md
> done
~~~
{: .bash}


> ## Exercise
>
> Remove `-2019` from all of the `.md` files. 
>
>> ## Solution
>>  
>>
>> ~~~
>> $ for filename in *-2019.md
>> > do
>> > name=$(basename ${filename} -2019.md)
>> > mv ${filename} ${name}.md
>> > done
>> ~~~
>> {: .bash}
>>
> {: .solution}
{: .challenge}
