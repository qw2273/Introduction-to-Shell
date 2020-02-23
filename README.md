# Introduction to Shell


## Find Location
__`pwd` __ show current location 
 
 __`ls`__  list all files in the current level  
__`ls -R`__  shows every file and directory in the current level, then everything in each sub-directory
__`ls -F -R`__  prints a `/` after the name of every directory and a `*` after the name of every runnable program.

 __`cd + new path`__  move to the new path 
 
 __`..`__  the directory above the one I'm currently in
- If you are in `/home/repl/seasonal`, then `cd ..` moves you up to `/home/repl` 
 
 __`.`__ the current directory
-  `cd .`  has no effect to the current location

__`~`__ path of user's home directory
-  `ls ~` will list the contents of your home directory
- `cd ~`  give you home location
- if you are in `/home/repl/seasonal`, then  `cd ~/../.` will take you to `/home/` 

## Copy 
__`cp`__  make a copy of original file 
- `cp original.txt duplicate.txt` creates a copy of  `original.txt`  called  `duplicate.txt`. If there already was a file called  `duplicate.txt`, it is overwritten.
- `cp seasonal/autumn.csv seasonal/winter.csv backup` copies  _all_  of the files into  `backup` directory.

## Move/rename

__`mv`__  
 1. move the files to another path 
 - `mv autumn.csv winter.csv` moves the files  `autumn.csv`  and  `winter.csv`  from the current working directory up _one level to its parent directory_
 2. rename files ( __overwriting__ if file name are the same) 
 - `mv course.txt old-course.txt` then `course.txt` in the current working directory is "moved" to the file `old-course.txt`

##  Delete 
__`rm`__  romove as many as _files_ you want by specify their paths
unlike `mv` which can be applied to directories , `rm` can not perfrom this, but instead you need to use __`rmdir`__
and if you want to create a new directory, use `mkdir`

## View content 
 1. __`cat`__  print out the file content  on to the screen 

 2. If file is large, then use __`less`__ :  
			* When you  `less`  a file, one page is displayed at a time; you can press spacebar to page down or type  `q`  to quit.
		* If you give  `less`  the names of __several files__, you can type  `:n`  (colon and a lower-case 'n') to move to the next file,  `:p`  to go back to the previous one, or  `:q`  to quit.

 3.  __`head`__   if not specify, print out the first 10 lines of a file ;  __`head -n 100`__ display the first 100 lines

4. __`tail -n`__ if not specify n , print out the last 10 lines of a file;  __`tail -n +[Num]`__ output the  starting with [Num]

 5. __`cut -f + columns no.  -d , file name`__: select columns 
     - `cut -f 2-5,8 -d , values.csv` which means  select `-f` _columns 2 through 5 and columns 8_ , using (`-d`)  comma `,`  as the separator 
     - `cut` doesn't understand quoted strings 

## Find matches 
__`grep`__: selects lines according to what they contain
	         - `grep bicuspid seasonal/winter.csv` prints lines from `winter.csv` that contain "bicuspid" 
|flag|usage  |
|--|--|
| `-c` |print a _count of matching lines_ rather than the lines themselves|
|`-h`|do _not_  print the names of files when searching multiple files |
| `-i` |ignore case (e.g., treat "Regression" and "regression" as matches) | 
| `-l` |  print the names of files that contain matches, not the matches | 
| `-n` |  print line numbers for matching lines | 
| `-v` |  invert the match, i.e., only show lines that  _don't_  match | 
 
## Count records 
__`wc`__ : word count
| **if print the number of**| **flag**  |
|--|--|
| **c**haracters | `-c`|
| **w**ords | `-w` | 
| **l**ines i |`-l`| 

## Sort 
__`sort`__: puts data in order
| flag | meaning |
|--|--|
|`-n` | sort numerically| 
|`-r` | reverse the order of its output| 
|`-b` | ignore leading blanks | 
| `-f` | tells it to **f**old case (i.e., be case-insensitive) | 

## Remove duplicates 
__`uniq`__: only removes _adjacent_ duplicated lines
`uniq -c`: display unique lines with a count of how often each occurs

## Repeat command  ！
`history` print a list of commands you have run recently
`!command` re-run the most recent use of that command 
`!55` re-run the 55th command in your history

## Store command's output > 
__`>`__: store output symbol 
      - `head -n 5 seasonal/summer.csv > top.csv`：`head`'s output is put in a new file called `top.csv`
     - `> result.txt head -n 3 seasonal/winter.csv`

## Combine commands  | 
__`|`__: pip line symbol, send the output from left side as input to left side
- `head -n 5 seasonal/summer.csv | tail -n 3`: 
The pipe symbol tells the shell to use the output of the command on the left as the input to the command on the right.

## Wildcards 
|Wildcards  | match | example |
|--|--|--| 
| `*` | match zero or more characters | `s*` will match `spring.csv`  or ` summer.csv` | 
|`?` | matches a single character| `201?.txt`  will match  `2017.txt`  or  `2018.txt`
|`[...]` |matches any one of the __characters__ inside the square brackets | `201[78].txt`  matches  `2017.txt`  or  `2018.txt`
|`{...}` |matches any of the comma-separated __patterns__ inside the curly brackets| `{*.txt, *.csv}`  matches any file whose name ends with  `.txt`  or  `.csv`| 

##  Variables 
|Type | Def|
|--|--|
| **environment variables** |those that are available all the time, in __UPPER CASE__  |
|**shell variables** | assign by `=` , called by adding `$` to it   | 

#### Common environment varaibles 
|Variable Name |Purpose |  
|--|--| 
|`HOME`|User's home directory | 
|`PWD`|Present working directory| 
| `SHELL` | Which shell program is being used| 
| `USER` | User's ID | 
To get a complete list (which is quite long), you can type  `set`  in the shell.


## Print Variable's value 
__`echo`__ prints its arguments.
    -  `echo hello` =》hello 
    - `echo $USER` => repl ( the value of environment variable value ) 


## For Loop 
`for` ...variable... `in` ...list.../wildcards ; `do` ...body... `; done`

- _Input_ 
 `for filetype in gif jpg png; do echo $filetype; done`
- _it produces_
`
gif
jpg
png
`

-_Input_
`for f in seasonal/*.csv; do echo $f ; head -n 2 $f | tail -n 1; done`
-_it produces_
`
seasonal/autumn.csv
2017-01-05,canine
seasonal/spring.csv
2017-01-25,wisdom
seasonal/summer.csv
2017-01-11,canine
seasonal/winter.csv
2017-01-03,bicuspid
`


## Edit file 
__`nano filename`__ open the file for editing 
| command | purpose |
|--|--|
| `Ctrl`  +  `K` |delete a line | 
|`Ctrl`  +  `U`| un-delete a line|
| `Ctrl`  +  `O`| save the file ('O' stands for 'output').  _You will also need to press Enter to confirm the filename!_|
|`Ctrl`  +  `X`|exit the editor| 

## Record previous steps 
1.  Run  `history`.
2.  Pipe its output to  `tail -n 10`  (or however many recent steps you want to save).
3.  Redirect that to a file called something like  `figure-5.history`.

## Store commands  and Re-run 
1.  Use  `nano dates.sh`  to create a file called  `dates.sh`  that contains this command:
   `cut -d , -f 1 seasonal/*.csv`
2.  Use  __`bash`__  to run the file  `dates.sh`.
OR `bash dates.sh > dates.out` to run file and save output to a new file `dates.out`


## Pass filenames to scripts 
__`$@`__ all of the command-line parameters given to the script
__`$1,$2,...`__ specific command-line parameters

-  `unique-lines.sh`  contains  `sort $@ | uniq`, 
when you run:
`bash unique-lines.sh seasonal/summer.csv`
then: 
the shell replaces  `$@`  with  `seasonal/summer.csv`  and processes one file. 

- `column.sh` contains `cut -d , -f $2 $1` 
and then run it using:
`bash column.sh seasonal/autumn.csv 1`
then:
the shell replaces   `$1`  with seasonal/autumn.csv, 
`$2` with 1 


## Auto-completion
__`tab`__  auto completion of file name, if multiple files exists with same first letter, __press tab twice__


## Get help 
__`man`__ see the manual  of a command 
