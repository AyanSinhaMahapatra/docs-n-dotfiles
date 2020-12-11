## zsh Shell CheatSheet

Docs - https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet

# Changing Settings/Themes/Plugins

# Key Bindings

1. Press ^G (the Control and G keys simultaneously) to toggle between local and global histories.
2. If you would prefer a different shortcut to toggle set the `PER_DIRECTORY_HISTORY_TOGGLE` environment variable.

# Aliases

1. `h`	       `history`
2. `hs`	       `history | grep`
3. `hsi`       `history | grep -i`

# tmux

1. `ta`		`tmux attach -t`		Attach new tmux session to already running named session
2. `tad`	`tmux attach -d -t`		Detach named tmux session
3. `ts`		`tmux new-session -s`	Create a new named tmux session
4. `tl`		`tmux list-sessions`	Displays a list of running tmux sessions
5. `tksv`	`tmux kill-server`		Terminate all running tmux sessions
6. `tkss`	`tmux kill-session -t`	Terminate named running tmux session
7. `tmux`	`_zsh_tmux_plugin_run`	Start a new tmux session

# Git Aliases

