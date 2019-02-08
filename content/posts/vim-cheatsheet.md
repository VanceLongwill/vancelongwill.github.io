---
title: "Vim Cheatsheet"
date: 2019-02-08T12:25:49+03:00
draft: false
---

## VIM cheatsheet

### Navigation

- relative line numbers `:set relativenumber` to allow quick navgation with number+`j`/`k`
- `^` go to start of line
- `$` go to end of line
- `gg` go to start of file
- `G` go to end of file
- `ctrl-u` / `ctrl-d` scroll up / down
- `zz` adjust scroll so that the cursor is in the middle of the window
- `/foo` search for 'foo'
- `*` highlight all occurences of current word (use `n` / `N` to go to the next/previous)

#- ```  ``  ``` go to the previous cursor position

### Editing

Entering insert mode
- `i` insert at current character
- `I` insert at start of current line
- `A` append to end of current line
- `a` append after current character

Some of my most frequently used vim actions follow this pattern.
- `yiw` copy inside current word
- `cab` cut inside and including current parenthesis ()
- `diB` delete inside current curly brackets {}
- `vt.` visual selection until character '.'

> TIP: if in doubt, just use a punctation character you know instead of the vim shortcut

- e.g. `cab` is equivalent to `ca)`, `ciB` is equivalent to `ci}`

- `dd` delete whole line


Visual block mode - great for comments and more! - `ctrl-v`

Visual line mode - `V`

- `>>` and `<<` to shift indentation back and forth
- `.` repeat last action

### Commands

- `:noh` clear all highlighting
- `:s/foo/bar/g` replace all instances of foo on the current line (if you want to replace within a visual selection, select first then just append the 's' to get `:'<,'>s/foo/bar/g` 
- `:s//bar/g` replace whatever we last searched with bar
- `:e.` explore the current directory
> NB: check out NERDTree for file exploration

### Other

- word count info & more
  `g ctrl-g`
- mapping caps lock to escape (I do this at OS level, not inside my vim config)
- hit `ctrl-z` to send your vim process to the background (or any other shell process), and type `fg` (foreground) in the shell to bring it back (this is especially useful for looking at `man` documentation

> NB: if you suspend more than one job, you'll have to specify the which process to bring to the foreground
- e.g. `fg %2`
- To get this number just type `jobs` in the terminal to see your suspended processes

- Automate repetitive editing tasks with macros 
    1. press `q` then a key to save the macro under
    2. do stuff
    3. press `q` again to stop recording
    4. press `@` then the key you saved it under to do stuff again

