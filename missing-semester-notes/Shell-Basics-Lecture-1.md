## Lecture 1

# All commands Covered

	$ pwd
	$ ls     (-l)
	$ cd     (~)  [Move to Home]
	$ echo   ($PATH)
	$ which
	$ date
	$ mv
	$ cp
	$ rm     (-r) [recursive]
	$ rmdir
	$ mkdir
	$ man
    $ cat
    $ tail   (-n1)
    $ curl   (--head --silent WEBSITE) 
    $ grep   (--ignore-case to_find_string)
    $ cut    (--delimiter=' ' -f2)
    $ tee
    $ touch
    $ stat
    $ chmod
    $ sh
    $ cut

# Input Output Streams

	$ <  (cat < hello.txt)          [Rewire the Input of this program to the contents of this file] 
	$ >  (echo hello > hello.txt)   [Rewire the Output of this program to be the File]
	$ >>                            [Same as > but append instead of Overwrite]
	$ |  (ls -l / | tail -n1)       [Take the output of the left_program and make it the output of the right_program ]   

# Important Stuff

1. $ which python3                                   [ Where is the program located (works inside virtualenvs) ]

2. $ cd -                                            [ Goes in the previous Directory]
	
3. $ ls -l                          Output Example - [drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing ]

                                    First - d for directory, else File. Groups of 3 - Read/Write/Execute. 
                                    If permission yes to all, then [rwx] or else [rw-]/[r-x]/[-wx].
                                    3 Groups - Owner of the File(missing), Owning Group(users), Others.

                                    For Directories, Read - List for dir, Write - Rename/Create/Move, Execute - Search

                                    #ToDo - s/l/t etc in place of d at the first

4. $ mv path_to_current_loc path_to_future_loc (This can also rename)
   $ cp path_to_current_loc path_to_future_loc

5. $ rmdir - remove a Directory if it's empty

6. Ctrl + L - Clear Terminal

7. $ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
                                     
                                     Gets HTTP headers, Searches for lines with "content-length", and cuts value of the second term using delimiter " "

8. $ # echo 1 > /sys/net/ipv4_forward   
                                     
                                     # -> Run as Sudo/Superuser

9. $ sudo su
   $ exit 

10. Tee opens and writes to file, given some input

    $ `sudo echo 3 | brightness`                [ will fail as shell opens brightness and it doesn't have permissions ]
    $ `echo 3 | sudo tee brightness`

11. Change a file access and modification times (atime, mtime).

    `touch {{filename}}`                          [ Create a new empty file(s) or change the times for existing file(s) to current time ]
    `touch -t {{YYYYMMDDHHMM.SS}} {{filename}}`   [ Set the times on a file to a specific date and time: ]
    `touch -r {{filename}} {{filename2}}`         [ Use the times from a file to set the times on a second file: ]


12. `chmod` Changes the access permissions of a file or directory.

    `chmod u+x {{file}}`                      [ Give the [u]ser who owns a file the right to e[x]ecute it ]
    `chmod u+rw {{file_or_directory}}`        [ Give the [u]ser rights to [r]ead and [w]rite to a file/directory: ]
    `chmod a+rx {{file}}`                     [ Give [a]ll users rights to [r]ead and e[x]ecute ]

13. `wc` - Count lines, words, or bytes.

    `wc -l {{file}}`   [ Count lines in file ]
    `wc -w {{file}}`   [ Count words in file ]
    `wc -c {{file}}`   [ Count characters (bytes) in file ]

