---
tags:
  - "#tmux"
---
## Overview

`tmux` is a program that runs in a terminal and allows multiple other terminal programs to run inside it. It manages multiple terminals for the user (i.e. terminal multiplexer) which allows a much more flexible workflow by allowing the user to have 

- **session**: (top-level organization) comprises multiple windows and panes. The user can dettach or attach to a session without stopping the execution of the programs inside that session. 
- **windows**: (mid-level organization) they are the equivalent to tabs in other programs and can hold multiple panes.
- **panes**: (low level organization) they allow to divide the terminal screen into multiple smaller independent screens.

> [!Note]
> **Sessions** are specially useful when remotely `ssh` into other computers. The ssh connection can break but the tmux session in the remote computer would continue to run and once the ssh connection is established back again the user can re-attach to the tmux session.

## Get help

```bash
man 1 tmux
```

## Cheatsheet

The prefix key `C-b` (ctrl+b) is the key binding used by tmux to differentiate commands.

| key bind | Description                                        |
| -------- | -------------------------------------------------- |
| ?        | list all key bindings                              |
| %        | split the current pane in two (left and right)<br> |
| "        | split the current pane in two (up and down)        |
| x        | kill the current pane                              |
| c        | create a new window                                |
| &        | kill the current window                            |
| ,        | rename current window                              |
| 0...9    | select window 0...9                                |
| w        | choose the current window interactively            |
https://tmuxcheatsheet.com/
## References

- [Getting started with Tmux](https://github.com/tmux/tmux/wiki/Getting-Started)