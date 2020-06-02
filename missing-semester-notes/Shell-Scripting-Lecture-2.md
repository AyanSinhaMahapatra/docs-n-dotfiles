## Lecture 1

# All commands Covered

	$ vim script.sh       [ To open in sublime text use `subl`]

	$ source function.sh  [ To define a function written in a file ]

	$ ffmpeg

	$ convert

	$ tldr                [ Examples for man-pages ]

	$ find

	$ fd

	$ locate

	$ grep

	$ rg

	$ shellcheck

# Tricks

1. `shellcheck mcd.sh`   - Checks for common script errors

2. Directory Listing 

	`ls -R`

	`tree`

	`broot`       [Lists some, fuzzy matching and navigate]

	`nnn`

# Using Special Variables in Bash

 - `$0`          - Name of the script

 - `$1` to `$9`  - Arguments to the script. $1 is the first argument and so on.

 - `$@`          - All the arguments

 - `$#`          - Number of arguments

 - `$?`          - Return code of the previous command

 - `$$`          - Process Identification number (pid) for the current script

 - `!!`          - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions,
                   then you can quickly execute it with sudo by doing sudo !!

 - `$_`          - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing 'Esc' followed by '.'


# Important Stuff

1. Equations in bash, if spaces it reats like first one program name, others are arguments

	$ foo=bar         [No Spaces]
	$ echo $bar


2. String quoting

	$ echo "Haha $foo"      [ This prints `Haha bar` ]

	$ echo 'Haha $foo'      [ This prints `Haha $foo` ]


3. Logical Operators

	$ ||                    [ If first is true, second not checked ]

	$ &&                    [ If first is false, second not executed ]

	$ ;                     [ Concatenate commands, will always execute everything]


4. Command Substitution (Will execute command, place in a temp file, and put that file nam in place of `<()` )

	$ `echo $(date)` 

	$ `for file in $(ls)`

	$ `<( CMD )`             [ will execute CMD and place the output in a temporary file and substitute the <() with that file’s name ]

	$ `diff <(ls foo) <(ls bar)`


5. shebang

	As scripts can be any language it is important to let the shell know what compiler/iterpreter to use

	`#!/usr/bin/bash`

	`#!/usr/bin/env python` in place of `#!/usr/local/bin/python`


5. Trash

	$ `> /dev/null/`          [ Will discard everything]


6. Access comparisions

	$ `man test`              [ gets all file comparisions ]

	$ `$? -ne 0`              [ If last command's error code is not equal to 0	 ]


7. Globbing ? and * to match one or any amount of characters respectively

	$ `ls _????`              [ executes ls in both `_2020` and `_2019` ]

	$ `ls *.sh`               [ shows all `.sh` files ]

	$ `ls` 


8. Expanding common substrings

	$ `ebook-convert Ghosts.{epub,mobi}` 

	$ `ebook-convert Ghosts.epub Ghosts.mobi`   [same as above]

	$ `cp /path/to/project/{foo,bar,baz}.sh /newpath{1,2}`

	$ `touch {foo,bar}/{a..h}`                  [ cartesian product expansion ]


9. Finding files - `find` (Alternate - `fd`, `locate`)

	$ `find . -name src -type d`                           [ # Find all directories named src ] 

	$ `find . -path '**/test/**/*.py' -type f`             [ # Find all python files that have a folder named test in their path ]

	$ `find . -mtime -1`                                  [ # Find all files modified in the last day ]

	$ `find . -size +500k -size -10M -name '*.tar.gz'`     [ # Find all zip files with size in range 500k to 10M ]

	$ `find . -name '*.tmp' -exec rm {} \;`


10. Finding Files - `fd`

	`fd {{pattern}}`                             [ Find files matching the given pattern in the current directory ]

	`fd {{'^foo'}}`                              [ Find files that begin with "foo" ]

	`fd --extension {{txt}}`                     [ Find files with a specific extension ]

	`fd {{pattern}} {{path/to/dir}}`             [ Find files in a specific directory ]

	`fd --hidden --no-ignore {{pattern}}`        [ Include ignored and hidden files in the search: ]


11. Faster name searching using locate

	$ `locate missing-semester`              [ Searches by filename ]

	$ `locate */{{filename}}`                [ Look for a file by its exact filename ]

	$ `sudo updatedb`                        [ Updates the database ]

	Note: a pattern containing no globbing characters is interpreted as *pattern* 

12. Grep         (alternate - `rg`)

	Matches patterns in input text.
	Supports simple patterns and regular expressions.

	`grep {{search_string}} {{path/to/file}}`                [ Search for an exact string ]

	`grep -i`                                                [ Search in case-insensitive mode ]

	`grep -RI`                                               [ Search recursively (ignoring non-text files) in current directory for an exact string ]

	`grep -{{C|B|A}} 3`                                      [ Print 3 lines of [C]ontext around, [B]efore, or [A]fter each match ]

	`grep -Hn `                                              [ Print file name with the corresponding line number for each match ]

	`grep -v {{search_string}}`                              [ Invert match for excluding specific strings ]


13. RIPGrep 

	`rg {{pattern}}`                           [ Recursively search the current directory for a regex pattern ]

	`rg -{{C|B|A}} n`                          [ Print n lines of [C]ontext around, [B]efore, or [A]fter each match ]

	`rg -uu {{pattern}}`                       [ Search for pattern including all .gitignored and hidden files ]

	`rg -t py {{pattern}}`                     [ Search for a pattern only in a certain filetype (e.g., sh, py, html, css, etc.) ]

	`rg -T py {{pattern}}`                     [ Search for a pattern excluding a certain filetype ]

	`rg {{pattern}} {{set_of_subdirs}}`        [ Search for a pattern only in a subset of directories ]

	`rg {{pattern}} -g {{glob}}`               [ Search for a pattern in files matching a glob ]

	`rg --files-with-matches {{pattern}}`      [ Only list matched files (useful when piping to other commands) ]

	`rg --files-without-matches {{pattern}}`   [ Only list matched files (useful when piping to other commands) ]

	`rg -F {{string}}`                         [ Search a literal string pattern ]



# ToDo

1. Use find/fd/grep/ripgrep/fzf with `nnn`

2. Use groot