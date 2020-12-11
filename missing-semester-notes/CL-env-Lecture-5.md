## Lecture 1

# All commands Covered

	$ sleep 20                  [ Sleep for 20 secs ]

	$ bg                        [ Run task in background ]

	$ fg                        [ Run task in foreground ]

	$ kill                      [ Sends signals to jobs ]

	$ nohup                     [ Jobs will not die if terminal closes ]

	$ tmux

	$ 

# Signals/Key-strokes related to Jobs (Software Interrupts)

1. `Ctrl + c`  - Exit current job.

2. `Ctrl + z`  - Suspend current job.

3. `Ctrl + /`  - Kill current job, can't be bypassed.

4. 

# Tmux - Terminal Multiplexer

All Keyboard Shortcuts - `Ctrl`+`b`,`?`

1. Sessions 

	`tmux new -s NAME`   	- Starts a tmux session with that name
	`tmux ls`            	- Shows all tmux sessions
	`tmux a`             	- Attaches last session
	`tmux a -t SESSION`  	- Attaches to SESSION

	`<C-b> d`            	- Detaches from current session
	`<C-b> (`            	- Move to prev session
	`<C-b> )`            	- Move to next session
	`<C-b> $`            	- Rename session

	`tmux kill-ses -t Sess`	- Kills Session `Sess`  

2. Windows - Equivalent to tabs in editors or browsers, they are visually separate parts of the same session

	`<C-b> c` 		Creates a new window.  
	`<C-d>`			To close it you can just terminate the shells doing this
	`<C-b> N` 		Go to the N th window. Note they are numbered
	`<C-b> p` 		Goes to the previous window
	`<C-b> n` 		Goes to the next window
	`<C-b> ,` 		Rename the current window
	`<C-b> '`       Search by window name and got to that
	`<C-b> w` 		List current windows

3. Panes - Like vim splits, pane lets you have multiple shells in the same visual display.

	`<C-b> "` 			Split the current pane horizontally
	`<C-b> %` 			Split the current pane vertically
	`<C-b> <direction>` Move to the pane in the specified direction. Direction here means arrow keys.
	`<C-b> z` 			Toggle zoom for the current pane
	`<C-b> [` 			Start scrollback. You can then press <space> to start a selection and <enter> to copy that selection.
	`<C-b> <space>` 	Cycle through pane arrangements.

4. Tmux Copy-Paste

	[TMUX Copy Pasting](https://www.rushiagr.com/blog/2016/06/16/everything-you-need-to-know-about-tmux-copy-pasting-ubuntu/)

	[TMUX Copy Pasting](https://www.rockyourcode.com/copy-and-paste-in-tmux/)

	Enable VIM Keybindings for Copy Paste by xclip

		`sudo apt-get install --assume-yes xclip`

    	```
    	## ~/.tmux.conf
		## Use vim keybindings in copy mode
		set-option -g mouse on
		setw -g mode-keys vi
		set-option -s set-clipboard off
		bind P paste-buffer
		bind-key -T copy-mode-vi v send-keys -X begin-selection
		bind-key -T copy-mode-vi y send-keys -X rectangle-toggle
		unbind -T copy-mode-vi Enter
		bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel 'xclip -se c -i'
		bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel 'xclip -se c -i'	
    	```

	1. Now you can enter copy mode normally with `CTRL`+`B`+`[`.
	2. Navigate the copy mode with vi-like-key bindings (u for up, d for down, etc.)
	3. Hit `v` to start copying.
	4. Press `y` or `Enter` to copy the text into the tmux buffer. This automatically
	   cancels copy mode.
	5. Paste into the buffer with `shift`+`p` (make sure that it’s uppercase P).

5. TMUX others

	Mouse and Command Modes

		`Shift`             To copy from terminal
		`set -g mouse on`   To activate Mouse mode [ In command mode ]
		`<C-b> :`           Command Mode


# Tricks

1. Run Last Backgrounded Job - `bg $!`

2. `fg` and `bg` if without job number, runs most recently suspended job.

3. `sleep 20 &`  - Run in the background

# Important Stuff

1. `kill` command

	`kill -SIGHUP`    [ Exits (like when terminal killed) ] 
	`kill -STOP`      [ Pauses Job ]
	`kill -KILL`      [ Kills job no matter what - SIGTERM ]

	`kill PID`        [ Terminates process ] 



# ToDo

1. Browse dotfiles and integrate inside mine
2. Research and s,etup Symlinks to dotfiles, in git repo
3. 