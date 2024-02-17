## Search for files

* `find` is recursive ***without*** parameters

* Base syntax: find PATH PARAMETERS

* `find /etc -name "\*host*"` - Search in /etc all file/directories with host in their name. \* is a wildcard

* `find . -perm 777 -exec rm -f '{}' \;` - Search from current position all files/directories with permissions 777 and after remove them

  `-exec` uses the result of find to do something

  `{}` will be substitute with result of find

  The exec's command must be contained between `-exec` and `\;`. 

  `;` is treated as end of command character in bash shell. For this I must escape it with \\. If escaped it will be interpreted by find and not by bash shell.

* Some parameter accepts value n with + or - in front. The meaning is:

  * +n - for greater than n
  * -n - for less than n
  * n - for exactly n

* `find /etc -size -100k` -  Search in /etc all files/directories with size less of 100 kilobytes

* `find . -maxdepth 3 -type f -size +2M` - Search starting from current position, descending maximum three directories levels, files with size major of 2 megabyte

* `find . \( -name name1 -o -name name2 \)`

  * `-o` or, it is used to combine two conditions. \ is escape to avoid that ( or ) will be interpreted by bash shell

* `find . -samefile file` - Find all files that have same i-node of file

* `find . \! -user owner` - It will show all files that aren't owned by user owner. `!` means negation, but must be escaped by \ to  not be interpreted by bash shell
    * ***-not*** is doing the same

* `find . -iname name` - Search name ignoring case

* `find . -perm 222` - Find all files with permissions equal to 222. E.g. only file with permissions 222 will be showed

* `find . -perm -222` - Find all files with at least permissions 222. E.g. 777 match as valid.

* `find . -perm /222` -  Find all files with write for owner or write for group or write for others (at least one)

* `find . -perm -g=w` - Find all files with at least  permission write for group

* `find . -atime +1` -  Show all files accessed at least two days ago (more than 24 hours)

## Evaluate and compare the basic file system features and options

References:

* [https://www.pks.mpg.de/~mueller/docs/suse10.2/html/opensuse-manual_en/manual/sec.new.fs.html](https://www.pks.mpg.de/~mueller/docs/suse10.2/html/opensuse-manual_en/manual/sec.new.fs.html)


## Compare and manipulate file content

* `diff file1 file2` - Compare file1 and file 2

* `diff -y file1 file2` - Compare file1 and file 2 with output in two columns

* `vi file` - It is used to manipulate a file

  Inside vi:

  * i - switch between *command mode* to *insert mode*
  * Esc - switch between *insert* to *command mode*

  In command mode:

  * o - open a new line and enter in insert mode

  * O - open a new line above current position and enter in insert mode

  * :wq - write and quit

  * :q! - quit without save

  * :w! - force write

  * u - undo

  * ctrl + r - redo

  * gg - go to file begin

  * G - go to last line

  * Search

    * :/texttosearch
    * n - next occurence
    * N - previous occurence
    * :300 - go to line 300

  * dd - delete current line

  * x - delete current character

  * d$ - delete from current point to end of line

  * Replace:

    * :%s/one/ONE/g - replace all occurrences of one with ONE

      :%s/one/ONE - replace first occurrences of one with INE

  * Cut and paste:

    * v - select text
    * y - copy text selected text
    * p - paste copied text
    * d - delete selected text

  In insert mode:

  * It's possible to insert text

* `uniq file` - Remove equal consecutive rows

  * `uniq -w 2 fle` - Remove equal consecutive rows comparing only first two characters

  * `uniq -c file` - Remove equal consecutive rows and show number of occurrences

* `sort file` order file content

  * `sort -k 2 file` - Order file content using as reference second word 

* cut -d delimiter -f column 

  * `cut -d '  ' -f 1 file` - Print first word of each line. Delimiter will be space

  * `cut -d '  ' -f 1,3 file` -  Print first and third word of each line. Delimiter will be space

* `cat file` - Print file content
* `tail file` - Print last 10 file lines
  * `tail -n 5 file` - Print last 5 file lines
  * `tail -f file` - Print last 10 file lines and append. Useful to monitor log files
* `head file` - Print first 10 file lines
  * `head -n 2 file` - Print first 2 file lines

* `tr SET1 SET2` - translate set of characters one to set of characters two

  * `cat file | tr test sub` - It will replace all occurrences of test with sub

  * `cat file | tr -s ' '` - It will replace all consecutive occurrences of space with one space

* `file namefile` - print the type of namefile


## Use input-output redirection (e.g. >, >>, |, 2>)

All Unix-based operating systems provide at least three different input and output channels - called *stdin*, *stdout* and *stderr* respectively - that allow communication between a program and the environment in which it is run.

In Bash each of these channels is numbered from 0 to 2, and takes the name of *file descriptor*, because it refers to a particular file: as it happens with any other file stored in the system, you can manipulate it, copy it, read it or write it on its.

When a Bash environment is started, all three default descriptor files point to the terminal where the session was initialized: the input (stdin - 0) corresponds to what is typed in the terminal, and both outputs - stdout ( 1) for traditional messages and stderr (2) for error messages - they are sent to the terminal. In fact, an open terminal in a Unix-based operating system is usually itself a file, commonly stored in /dev/tty0; when a new session is opened in parallel with an existing one, the new terminal will be /dev/tty1 and so on. Therefore, initially the three file descriptor all point to the file representing the terminal in which they are executed.

There are operator to redirect input, ouput and error.

* < - redirect stdin

  * `wc < file`

    Execute wc using the content of file as input

* \> and >> - redirect stdout 

  * `echo test > file1` - Write test in a file1. The content of file1 will be replaced

  * `echo test >> file1` - Append test in file1

* 2> - redirect stderr 

  * `find /proc -name "cpu*" 2> /dev/null` - Find in /proc file/directory that begin with cpu and redirect all errors, like 'Permission Denied' to special file /dev/null (virtual file that discard all data)

* | - the stdout is transformed in stdin

  * `cat file | wc` - Use the output of 'cat file' as input of wc

* 2>&1 - redirect stderr  to same place of stdout 

* All redirections can be combined

  *  `find /etc -name '\*a\*' 2> /dev/null | less`


## Analyze text using basic regular expressions

* File Globbing in Linux

  File globbing is a feature provided by the UNIX/Linux shell to represent multiple
  filenames by using special characters called wildcards with a single file name.
  A wildcard is essentially a symbol which may be used to substitute for one or
  more characters. Therefore, we can use wildcards for generating the appropriate
  combination of file names as per our requirement.

  * \* - Every character 

    `ls -l a*` - List all file/directories that begin with a

  * ? - Every single character

    `ls -l a?` - List all file/directories formed by two character that begin with a

  * [ab] - list of characters

    `ls -l a[ab]` - List file/directories called aa or ab

  * [a-c]

    `ls -l a[a-c]` - List file/directories called aa, ab and ac

  * Wildcards can be combined

    `ls -l a[a-c]*` - List all file/directories that begins aa, ab and ac

* grep pattern path/*

  Search pattern inside the strings of the files in path/*. Show file name and row matching pattern

  It is no recursive and key sensitive. To have recursion -r must be added.

  Pattern can be a regular expression. The regular expression must be surrounded by '  ' otherwise content could match bash globing. 

  * `grep -l patter path/*` - Search pattern inside file in path/*. Show only file name.

  * `grep -lr patter path/*` - Search pattern inside file in path/* and path subdirectories. Show only file name.

  * `grep -ilr patter path/*` - Search pattern ignoring case inside file in path/* and path subdirectories. Show only file name.



Regular Expressions

| Character |                Definition                |  Example   |        Result         |
| :-------: | :--------------------------------------: | :--------: | :-------------------: |
|     ^     |            Start of a string             |    ^abc    |    abc, abcd, abc1    |
|     $     |             End of a string              |    abc$    |  abc, rasabc, 2aabc   |
|     .     |       Any character except newline       |    a.c     |     abc, acc, a1c     |
|           |                                          | Alteration |           a           |
|   {...}   | Explicit quantity of preceding character |   ab{2}c   |         abbc          |
|   [...]   |   Explicit set of characters to match    |   a[bB]c   |        abc,aBc        |
| [a-z0-9]  |   One lower case characters or number    | a[a-z0-9]c |        aac,a1c        |
|   (...)   |           Group of characters            |  (abc){2}  |        abcabc         |
|     *     | Null or more of the preceding characters |    a*bc    | bc, abc, aabc, aaaabc |
|     +     |  One or more of the preceding character  |    a+bc    |       abc, aabc       |
|     ?     |  Null or one of the preceding character  |    a?bc    |        bc, abc        |
|    ^$     |               Empty string               |            |                       |

* Not all regular expressions are supported by `grep`. As alternative can be used `egrep`

* sed - Without `-i` the results of file alteration won't be permanent

  * `sed 's/source/target/' file` - In any row of file, it will change first occurrence of source to target. Print all rows.

  * `sed 's/source/target/g' file`- In any row of file, it will change all occurrences of source to target. Print all rows.

  * `sed 's/source/target/gI'` - In any row of file, it will change all occurrences of source to target. Ignore case = case insensitive. Print all rows.

  * `sed '10s/source/target/' file` - For row 10, it will change first occurrence of source to target. Print all rows.

  * `sed -n 's/source/target/p'` - In any row of file, it will change first occurrence of source to target. Print only changed rows.

  * `sed -n '/source/p' file` - It will print only rows that contain source. It is equal to grep source file

  * `sed -n 2,4p file` - Show lines from 2 to 4.

  * `sed '/source/d' file` - Delete rows with source.

  * `sed -n 12d file` - Delete row 12.

  * `sed '11inewline' file` - It will insert newline as line 11.

  * `sed -i 's/source/target/g' file` - In any row of file, it will change all occurrences of source to target. Save result to file.

  * `sed -i.orign 's/source/target/g' file` - In any row of file, it will change all occurrences of source to target. Save result to file but keep an copy of original file with name file.orign

References:

* [https://www.linuxnix.com/10-file-globbing-examples-linux-unix/](https://www.linuxnix.com/10-file-globbing-examples-linux-unix/)
