## Jupyter Notebook/Lab Basics

# Shortcuts

1. [d] + [d]              : Delete Cell
2. [Ctrl] + [z]           : Undo
3. [Ctrl] + [Shift] + [z] : Redo

# Docs/Autocompletion

1. Documentation        `help(len)` OR `len?`
2. Code (if python)     `np.stack??`
3. Exploring Modules    `L.c<TAB>` -> `L.clear  L.copy   L.count`  
4. Wildcard Matching    `str.*find*?`

# Magic Commands

1. `%run myscript.py`              [Runs Script and everything defined is available]

2. `%cpaste and %paste`            [Interactive and Non-interactive pasting]

3. `%timeit`                       [ ]

4. `%history -n 1-4`               [History of inputs]

5. `%automagic 1`                  [Use Commands]

6. `%cd`                           [Change Directory]

7. `%debug`                        [ipdb - Debugger for iPython]

8. `%pdb on`                       [ipdb launched on Error]

# All commands Covered

1. Shell Commands (Any) in iPython - 
   
    `!pwd`
    `files = !ls` - Save Output to a Python List-ish type [IPython.utils.text.SList]
                  - files.fields(0)
                  - files.grep('chm', field=-1)
                  - files.sort(1, nums = True)


2. In/Out Commands

	`Print(In[2])`
	`Out[2] ** 2 + Out[3] ** 2`
	`print(_)`                    : Prints last output
	                                2 and 3 underscores also works `__` `___`
	`math.sin(2) + math.cos(2);`  : Supress output by Semicolon

2. `%xmode` `Plain`/`Context`/`Verbose`

# Timing and Profiling

1. Time Profiling
   
   `%prun sum_of_lists(1000000)`
   `%load_ext line_profiler`
   `%lprun -f sum_of_lists sum_of_lists(5000)`

2. Memory Profiling

   `%load_ext memory_profiler`
   `%memit sum_of_lists(1000000)`
   `from mprun_demo import sum_of_lists`
   `%mprun -f sum_of_lists sum_of_lists(1000000)`

3. Timing Snippets

   `%%timeit -n 10`         [First Line not Timed]
   `%timeit L.sort()`       [Time one statement]

4. `pip install line_profiler memory_profiler`

# ToDo

1. Read all pf %magic and Docs to Find More Magic Functions