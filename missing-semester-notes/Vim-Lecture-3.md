## Vim - Lecture 3

# Tricks

1. `.` - Dot does the previous thing again, can be edit/insert.

2. `^s` freezes and `^q` unfreezes.

3. Open man page for word under the cursor

4. Editing Read Only System Files, after making edits

   `:w !sudo tee %` and select `L` to reload file from disk. And then `:q` to close.



# Modes 

1. Normal (Press `Esc` to come out from every other mode) [Navigate]

2. Insert (Press `i` to go into)  [Put Into Text Buffer]

3. Replace (Press `r` to go into)

4. Selection

	Visual         (Press `v` to go into)
	Visual-line    (Press `shift v` to go into)
	Visual-block   (Press `ctrl v` to go into)

5. Command Line Mode  (Press `:` key to go into)

# Notations

1. Ctrl + V  

	^V
	<C-V>

# Vim Command Line

	`:q`                 quit (close window)
	`:qa`                closes all open windows
	`:w`                 save (“write”)
	`:wq`                save and quit
        `:x`                 save if buffer changed, and exit
	`:e` {name of file}  open file for editing
	`:ls`                show open buffers
	`:help {topic}`      open help
	`:help :w`           opens help for the :w command
	`:help w`            opens help for the w movement
        `:saveas filename`   Save file as filename

## Vim Normal Mode

# Movement

1. Undo-Redo

   `u`  - Undo
   `^r` - Redo

2. Basic Movement
   
   `h` - Left
   `j` - Down
   `k` - Up
   `l` - Right 

3. Words 

   `w` - Next Word
   `b` - Beginning of Word
   `e` - End of Word

3. Lines

   `0` - Beggining of the line
   `^` - First non-blank Character
   `$` - End of the line

4. Move by Screen

   `H` - top of the screen
   `M` - Middle of the screen
   `L` - Bottom of the Screen
   `^b` - Move up 1 full screen
   `^f` - Move down 1 screen
   `^u` - Move up 1/2 Screen
   `^d` - Move down 1/2 Screen

5. Move by block

   `{` - Previous Paragraph/function/block
   `}` - Next Paragraph/function/block

6. Move by File

   `gg` - Beginning of the file
   `G`  - End of the file

7. Search and Replace 

   `/{regex}`       - Search in current file
   `n`              - Upwards Navigating Matches
   `N`              - Downwards Navigating Matches
   `:%s/old/new/g`  - Replace all old with new
   `:%s/old/new/gc` - Same but with confirmations

7. Find

   `f{char}` - Find forward to char
   `t{char}` - Find forward before char
   `F{char}` - Find backward to char
   `T{char}` - Find backward after char
   `:`       - Repeat previous f/t/F/T movement
   `:`       - Repeat previous f/t/F/T movement, backwards

8. Delete/Change (here delete means Cut)

   `d{motion} - Deletes
   `c{motion} - Deletes + [Insert Mode] 
   `dd` - Deletes Line
   `cc` - Deletes line + [Insert Mode]
   `D`  - delete to the end of line
   `C`  - delete to the end of line + [Insert Mode]
   `S`  - Delete line and substitute text
   `cc` - Delete Line + [Insert Mode]


9. Delete/Replace Current Character

   `x`       - Deletes Current Char
   `s`       - Deletes current char + [Insert Mode]
   `r{char}` - Replaces current char with {char}

10. Yank(copy)/Paste

   `y{motion}` - Copy the word/whatever
   `p`         - Paste
   `yy`        - Copy Line


# Edits/Verbs

1. Newline + Insert Mode

   `a` - After this, next character
   `A` - End of the Line
   `o` - Line below
   `O` - Line above

2. Change Case

   `~` - Change case of all selection upper to lower, and vice-versa

# Visual Mode

   Visual Mode       - Selects chunk of text depending on motion
   Visual Line Mode  - Selects chunk of lines
   Visual Block Mode - Selects rectangular block

# Counts

  `n{motion/verb}` - Performs Motion/Verb n times

# Modifiers

   `ci{paren}` - Delete text inside {paren} + [Insert Mode]
   `da{paren}` - Delete text inside {paren} + {paren} itself

# Windows/Tabs

   `:sp file` - Opens a horizontal split, same file if nowt given
   `:vsp file` - Same but vertical split
   `Ctrl + ww` - Move Between Splits
   `:x`        - Closes current windows and saves changes to buffer
   `tabnew file` - Opens File in a new tab, or same file
   `gt`/`:tabn`  - Next Tab
   `gT`/`:tabp`  - Previous Tab
   `#gt`         - Move to tab #
   `:tabo`       - Close all but current tab`

# Plugins

   `Ctrl + p`        - Starts ctrlp, fuzzy file search.
   `:help ctrlp.txt` - Help for ctrlp
# ToDo

1. Rebind Keys Esc
2. Customize Config
3. Learn More about Vimium (Google Chrome)
4. Vim Bindings for Jupyter Notebooks
5. Complete Vimtutor
6. Learn Vim macros


