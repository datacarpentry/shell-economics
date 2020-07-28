---
title: "Working with Files and Directories"
teaching: 30
exercises: 15
questions:
- "How can I view and search file contents?"
- "How can I create, copy and delete files and directories?"
- "How can I control who has permission to modify a file?"
- "How can I repeat recently used commands?"
objectives:
- View, search within, copy, move, and rename files. Create new directories.
- Use wildcards (`*`) to perform operations on multiple files.
- Make a file read only.
- Use the `history` command to view and repeat recently used commands.
keypoints:
- "You can view file contents using `less`, `cat`, `head` or `tail`."
- "The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories."
- "You can view file permissions using `ls -l` and change permissions using `chmod`."
- "The `history` command and the up arrow on your keyboard can be used to repeat recently used commands."
---

## Working with Files

### Our data set: World Development Indicators

Now that we know how to navigate around our directory structure, let's
start working with our data files. The World Development Indicators dataset is stored in the `worldbank` directory. 

### Wildcards

Navigate to your `data/raw/worldbank` directory:

~~~
$ cd ~/Downloads/shell-economics/data/raw/worldbank/
~~~
{: .bash}

We are interested in looking at the CSV files in this directory. We can list
all files with the .csv extension using the command:

~~~
$ ls *.csv
~~~
{: .bash}

~~~
WDICountry-Series.csv	WDICountry.csv		WDIData.csv		WDIFootNote.csv		WDISeries-Time.csv	WDISeries.csv
~~~
{: .output}

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character. 
Thus, `*.csv` matches every file that ends with `.csv`. 

This command: 

~~~
$ ls *Data.csv
~~~
{: .bash}

~~~
WDIData.csv
~~~
{: .output}

lists only the file that ends with `Data.csv`.

This command:

FIXME: is this working on git bash?
~~~
$ ls /usr/bin/*.sh
~~~
{: .bash}

~~~
/usr/bin/power_report.sh
~~~
{: .output}

Lists every file in `/usr/bin` that ends in the characters `.sh`.

> ## Home vs. Root
> 
> The `/` character is another navigational shortcut and refers to your root directory.
> The root directory is the highest level directory in your file system and contains
> files that are important for your computer to perform its daily work, but which you usually won't
> have to interact with directly. In our case,
> the root directory is two levels above our home directory, so `cd` or `cd ~` will take you to `/Users/koren`
> and `cd /` will take you to `/`, which is equivalent to `~/../../`. Try not to worry if this is confusing,
> it will all become clearer with practice.
> 
> While you will be using the root at the beginning of your absolute paths, it is important that you avoid 
> working with data in these higher-level directories, as your commands can permanently alter files that the 
> operating system needs to function. In many cases, trying to run commands in root directories will require 
> special permissions which are not discussed here, so it's best to avoid it and work within your home directory.
{: .callout}

> ## Exercise (Linux and Mac)
> Do each of the following tasks from your current directory using a single
> `ls` command for each:
> 
> 1.  List all of the files in `/usr/bin` that start with the letter 'c'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a'. 
> 3.  List all of the files in `/usr/bin` that end with the letter 'o'.
>
> Bonus: List all of the files in `/usr/bin` that contain the letter 'a' or the
> letter 'c'.
> 
> Hint: The bonus question requires a Unix wildcard that we haven't talked about
> yet. Try searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
> 
> > ## Solution
> > 1. `ls /usr/bin/c*`
> > 2. `ls /usr/bin/*a*`
> > 3. `ls /usr/bin/*o`  
> > Bonus: `ls /usr/bin/*[ac]*`
> > 
> {: .solution}
{: .challenge}

> ## Exercise (Windows)
> Do each of the following tasks from your current directory using a single
> `ls` command for each:
> 
> 1.  List all of the files in `/usr/bin` that start with the letter 'c' and end with '.exe'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a' and end with '.exe'. 
> 3.  List all of the files in `/usr/bin` that end with the letters 'o.exe'.
>
> Bonus: List all of the files in `/usr/bin` that contain the letter 'a' or the
> letter 'c' and end with '.exe'.
> 
> Hint: The bonus question requires a Unix wildcard that we haven't talked about
> yet. Try searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
> 
> > ## Solution
> > 1. `ls /usr/bin/c*.exe`
> > 2. `ls /usr/bin/*a*.exe`
> > 3. `ls /usr/bin/*o.exe`  
> > Bonus: `ls /usr/bin/*[ac]*.exe`
> > 
> {: .solution}
{: .challenge}

> ## Exercise
> We can use the command `echo` to see how the wildcard character is interpreted by the shell.
> 
> ~~~
> $ echo *.csv
> ~~~
> {: .bash}
> 
> ~~~
> WDICountry-Series.csv WDICountry.csv WDIData.csv WDIFootNote.csv WDISeries-Time.csv WDISeries.csv
> ~~~
> {: .output}
> 
> The `*` is expanded to include any file that ends with `.csv`. We can see that the output of
> `echo *.csv` is the same as that of `ls *.csv`.
> 
> What would the output look like if the wildcard could *not* be matched? Compare the outputs of
> `echo *.missing` and `ls *.missing`.
> 
> > ## Solution
> > ~~~
> > $ echo *.missing
> > ~~~
> > {: .bash}
> > 
> > ~~~
> > *.missing
> > ~~~
> > {: .output}
> > 
> > ~~~
> > $ ls *.missing
> > ~~~
> > {: .bash}
> > 
> > ~~~
> > ls: cannot access '*.missing': No such file or directory
> > ~~~
> > {: .output}
> > 
> {: .solution}
{: .challenge}


## Command History

If you want to repeat a command that you've run recently, you can access previous
commands using the up arrow on your keyboard to go back to the most recent
command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts: 

- <kbd>Ctrl</kbd>+<kbd>C</kbd> will cancel the command you are writing, and give you a 
fresh prompt.
- <kbd>Ctrl</kbd>+<kbd>R</kbd> will do a reverse-search through your command history.  This
is very useful.
- <kbd>Ctrl</kbd>+<kbd>L</kbd> or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

~~~
$ history
~~~
{: .bash}

to see a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

~~~
259  ls *
260  ls /usr/bin/*.sh
261  ls *R1*csv
~~~
{: .output}

then you could repeat command #260 by entering:

~~~
$ !260
~~~
{: .bash}

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.

> ## Exercise
> Find the line number in your history for the command that listed all the .sh
> files in `/usr/bin`. Rerun that command.
>
> > ## Solution
> > First type `history`. Then use `!` followed by the line number to rerun that command.
> {: .solution}
{: .challenge}


## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all of the
contents using the program `cat`. 

Enter the following command from within the `worldbank` directory: 

~~~
$ cat WDIData.csv
~~~
{: .bash}

This will print out all of the contents of the `WDIData.csv` to the screen. (It will take a few seconds, as the file is large.)


> ## Exercise
> 
> 1. Print out the contents of the `~/Downloads/shell-economics/data/raw/worldbank/WDIFootNote.csv ` file. What is the last line of the file? 
> 2.  From your home directory, and without changing directories,
> use one short command to print the contents of all of the files in
> the `~/Downloads/shell-economics/data/raw` directory.
> 
> > ## Solution
> > 1. The last line of the file is `"ZWE","ST.INT.ARVL","YR2010","Refers to arrivals of non-resident visitors at national borders.",`.
> > 2. `cat ~/Downloads/shell-economics/data/raw/*`
> {: .solution}
{: .challenge}

`cat` is a terrific program, but when the file is really big, it can
be annoying to use. The program, `less`, is useful for this
case. `less` opens the file as read only, and lets you navigate through it. The navigation commands
are identical to the `man` program.

Enter the following command:

~~~
$ less WDIData.csv
~~~
{: .bash}

Some navigation commands in `less`:

| key     | action |
| ------- | ---------- |
| <kbd>Space</kbd> | to go forward |
|  <kbd>b</kbd>    | to go backward |
|  <kbd>g</kbd>    | to go to the beginning |
|  <kbd>G</kbd>    | to go to the end |
|  <kbd>q</kbd>    | to quit |

`less` also gives you a way of searching through files. Use the
"/" key to begin a search. Enter the word you would like
to search for and press `enter`. The screen will jump to the next location where
that word is found. 

**Shortcut:** If you hit "/" then "enter", `less` will  repeat
the previous search. `less` searches from the current location and
works its way forward. Note, if you are at the end of the file and search
for the word "Belgium", `less` will not find it. You either need to go to the
beginning of the file (by typing `g`) and search again using `/` or you
can use `?` to search backwards in the same way you used `/` previously.

For instance, let's search forward for the word `Tanzania` in our file. 
You can see that we go right to that row, what it looks like,
and where it is in the file. If you continue to type `/` and hit return, you will move 
forward to the next instance of this string. If you instead type `?` and hit 
return, you will search backwards and move up the file to previous examples of this string.

> ## Exercise
>
> What is the three-letter word after the first occurence of the word "Tanzania"?
> 
> > ## Solution
> > `TZA`
> {: .solution}
{: .challenge}

Remember, the `man` program actually uses `less` internally and
therefore uses the same commands, so you can search documentation
using "/" as well!

There's another way that we can look at files, and in this case, just
look at part of them. This can be particularly useful if we just want
to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at
the beginning and end of a file, respectively.

~~~
$ head WDISeries-Time.csv 
~~~
{: .bash}

~~~
"SeriesCode","Year","DESCRIPTION",
"SP.ADO.TFRT","YR1960","Interpolated using data for 1957 and 1962.",
"SP.DYN.AMRT.FE","YR1960","Interpolated using data for 1957 and 1962, if the data source is United Nations World Population Prospects.",
"SP.DYN.AMRT.MA","YR1960","Interpolated using data for 1957 and 1962, if the data source is United Nations World Population Prospects.",
"SP.DYN.TO65.FE.ZS","YR1960","Interpolated using data for 1957 and 1962.",
"SP.DYN.TO65.MA.ZS","YR1960","Interpolated using data for 1957 and 1962.",
"SP.DYN.TO65.MA.ZS","YR1961","Interpolated using data for 1957 and 1962.",
"SP.DYN.TO65.FE.ZS","YR1961","Interpolated using data for 1957 and 1962.",
"SP.DYN.AMRT.MA","YR1961","Interpolated using data for 1957 and 1962, if the data source is United Nations World Population Prospects.",
"SP.DYN.AMRT.FE","YR1961","Interpolated using data for 1957 and 1962, if the data source is United Nations World Population Prospects.",
~~~
{: .output}

~~~
$ tail WDISeries-Time.csv 
~~~
{: .bash}

~~~
"SP.DYN.TO65.FE.ZS","YR2016","Interpolated using data for 2012 and 2017.",
"SP.DYN.TO65.FE.ZS","YR2017","The data refer to 2015-2020.",
"SP.DYN.AMRT.MA","YR2017","The data refer to 2015-2020, if the data source is United Nations World Population Prospects.",
"SM.POP.NETM","YR2017","The data refer to five-year periods running from 1 July, 2015 to 30 June, 2020.",
"ER.MRN.PTMR.ZS","YR2017","Reflects data that was available in the Protected Planet API in August 2018.",
"ER.LND.PTLD.ZS","YR2017","Reflects data that was available in the Protected Planet API in August 2018.",
"ER.PTD.TOTL.ZS","YR2017","Reflects data that was available in the Protected Planet API in August 2018.",
"SP.ADO.TFRT","YR2017","Interpolated using data for 2012 and 2017.",
"SP.POP.BRTH.MF","YR2017","The data refer to 2015-2020.",
"DT.DOD.PVLX.CD","YR2017","Present value calculations for these countries are for public and publicly guaranteed debt only.",
~~~
{: .output}

The `-n` option to either of these commands can be used to print the
first or last `n` lines of a file. 

~~~
$ head -n 1 WDISeries-Time.csv 
~~~
{: .bash}

~~~
"SeriesCode","Year","DESCRIPTION",
~~~
{: .output}

This is particularly useful for CSV files, as they often have a column header in their first row.

~~~
$ tail -n 1 WDISeries-Time.csv 
~~~
{: .bash}

~~~
"DT.DOD.PVLX.CD","YR2017","Present value calculations for these countries are for public and publicly guaranteed debt only.",
~~~
{: .output}

## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move
them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all 
of those files. In cases like this, it's much faster to do these operations at the command line.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted. 
For this lesson, our raw data is our CSV files.  We don't want to accidentally change the original files, so we'll make a copy of them
and change the file permissions so that we can read from, but not write to, the files.

First, let's make a copy of one of our CSV files using the `cp` command. 

Navigate to the `data/raw/worldbank` directory and enter:

~~~
$ cp WDIData.csv WDIData-copy.csv 
$ ls -F
~~~
{: .bash}

~~~
WDICountry-Series.csv*	WDIData-copy.csv*	WDIFootNote.csv*	WDISeries.csv*
WDICountry.csv*		WDIData.csv*		WDISeries-Time.csv*
~~~
{: .output}

We now have two copies of the `WDIData.csv` file, one of them named `WDIData-copy.csv`. We'll move this file to a new directory
called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create:

~~~
$ mkdir backup
~~~
{: .bash}

### Moving / Renaming 

We can now move our backup file to this directory. We can
move files around using the command `mv`: 

~~~
$ mv WDIData-copy.csv backup
$ ls backup
~~~
{: .bash}
 
~~~
WDIData-copy.csv
~~~
{: .output}

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup:

~~~
$ cd backup
$ mv WDIData-copy.csv WDIData-backup.csv
$ ls
~~~
{: .bash}

~~~
WDIData-backup.csv
~~~
{: .output}

### File Permissions

We've now made a backup copy of our file, but just because we have two copies, it doesn't make us safe. We can still accidentally delete or 
overwrite both copies. To make sure we can't accidentally mess up this backup file, we're going to change the permissions on the file so
that we're only allowed to read (i.e. view) the file, not write to it (i.e. make new changes).

View the current permissions on a file using the `-l` (long) flag for the `ls` command: 

~~~
$ ls -l
~~~
{: .bash}

~~~
-rwxr-xr-x@ 1 koren  staff  213164145 Oct 10 17:25 WDIData-backup.csv
~~~
{: .output}

The first part of the output for the `-l` flag gives you information about the file's current permissions. There are ten slots in the
permissions list. The first character in this list is related to file type, not permissions, so we'll ignore it for now. The next three
characters relate to the permissions that the file owner has, the next three relate to the permissions for group members, and the final
three characters specify what other users outside of your group can do with the file. We're going to concentrate on the three positions
that deal with your permissions (as the file owner). 

![Permissions breakdown](../fig/rwx_figure.svg)

Here the three positions that relate to the file owner are `rwx`. The `r` means that you have permission to read the file, the `w` 
indicates that you have permission to write to (i.e. make changes to) the file, and the third position is a `x`. We'll 
talk more about this in [a later lesson](http://www.datacarpentry.org/shell-economics/05-writing-scripts/).

Our goal for now is to change permissions on this file so that you no longer have `w` or write permissions. We can do this using the `chmod` (change mode) command and subtracting (`-`) the write permission `-w`. 

~~~
$ chmod -w WDIData-backup.csv 
$ ls -l 
~~~
{: .bash}

~~~
-r-xr-xr-x@ 1 koren  staff  213164145 Oct 10 17:25 WDIData-backup.csv
~~~
{: .output}

### Removing

To prove to ourselves that you no longer have the ability to modify this file, try deleting it with the `rm` command:

~~~
$ rm WDIData-backup.csv
~~~
{: .bash}

You'll be asked if you want to override your file permissions:

~~~
override r-xr-xr-x  koren/staff for WDIData-backup.csv? 
~~~
{: .output}

If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra 
measure of security, as there is one more step between us and deleting our data files.

**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm` will not delete directories. You can tell `rm` to
delete a directory using the `-r` (recursive) option. Let's delete the backup directory
we just made. 

Enter the following command:

~~~
$ cd ..
$ rm -r backup
~~~
{: .bash}

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory, 
you will be asked whether you want to override your permission settings. 

> ## Exercise
>
> Starting in the `data/raw/worldbank/` directory, do the following:
> 1. Make sure that you have deleted your backup directory and all files it contains.  
> 2. Create a backup of each of your CSV files using `cp`. (Note: You'll need to do this individually for each of the six CSV files. We haven't 
> learned yet how to do this
> with a wildcard.)  
> 3. Use a wildcard to move all of your backup files to a new backup directory.   
> 4. Change the permissions on all of your backup files to be write-protected.  
>
> > ## Solution
> >
> > 1. `rm -r backup`  
> > 2. 
> > ```
> > cp WDICountry-Series.csv WDICountry-Series-backup.csv
> > cp WDICountry.csv WDICountry-backup.csv 
> > cp WDIData.csv WDIData-backup.csv 
> > cp WDIFootNote.csv WDIFootNote-backup.csv 
> > cp WDISeries-Time.csv WDISeries-Time-backup.csv 
> > cp WDISeries.csv WDISeries-backup.csv
> > ```
> > {: .bash}
> > 3. `mkdir backup` and `mv *-backup.csv backup`
> > 4. `chmod -w backup/*-backup.csv`   
> > It's always a good idea to check your work with `ls -l backup`. You should see something like: 
> > 
> > ~~~
> > total 522592
> > -r-xr-xr-x@ 1 koren  staff     785984 Oct 10 17:35 WDICountry-Series-backup.csv
> > -r-xr-xr-x@ 1 koren  staff     169534 Oct 10 17:35 WDICountry-backup.csv
> > -r-xr-xr-x@ 1 koren  staff  213164145 Oct 10 17:35 WDIData-backup.csv
> > -r-xr-xr-x@ 1 koren  staff   49492815 Oct 10 17:35 WDIFootNote-backup.csv
> > -r-xr-xr-x@ 1 koren  staff      43570 Oct 10 17:35 WDISeries-Time-backup.csv
> > -r-xr-xr-x@ 1 koren  staff    3898578 Oct 10 17:35 WDISeries-backup.csv
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}
