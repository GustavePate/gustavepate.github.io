---
layout: post
title:  "My vim cheat sheet"
published: true
description: "This is yet another vim cheat sheet, don't forget it's available on github if you want to tweak it"
categories: [linux]
tags: [vim, shortcuts, cheat sheet]
image:  false
article: yes
---

* my toc
{:toc}

# Actions (verbs)

* `y` yank or copy
* `d` delete or cut
* `c` cut / cut and go to insert mode
* `p` paste

# Moves (adjectives)

These can be applied to previously definend actions

* `s` sentence
* `w` next word
* `W` next space-separated word
* `b` previous word
* `B` begining of / previous word (space separated)
* `e` end of word
* `E` end of / next word (space separated)
* `tx` to next character `x` excluding `x`
* `Tx` to previous character `x` excluding `x`
* `fx` to move to the next instance of a `x` character on the current line, (; to repeat)
* `Fx` to move to the previous instance of a `x` character on the current line (
; to repeat)
* `)` sentence end
* `}` paragraph end
* `$` line end
* `^` first non-blank character of line
* `0` start of line
* `gg` file beginning
* `G` end of file
* `gD` go to the current method definition
* `%` on a `(` or `\[` go to the ending brace `)` or `\]` otherwise go to the preceding one.

# Normal mode to insert mode
* `i` start insert mode at cursor
* `I`  insert at the beginning of the line
* `a` append after the cursor
* `A` append at the end of the line
* `o` open (append) blank line below current line (no need to press return)
* `O` open blank line above current line
* `ea` append at end of word
* `cc` change (delete and go in insert mode) an entire line
* `cw` change (replace) to the end of word
* `c$` change (replace) to the end of line
* `ciw` replace word
* `ci)` replace parenthesis cent
* `s` delete character at cursor and enter insert mode
* `S` delete line at cursor and substitute text (same as cc)

# Undo / Redo / Repeat

* `u` undo
* `Ctrl+r` redo
* `.` repeat last command


# Scrolling

* `Crtl+(f/F)` next page
* `Crtl+(b/B)` previous page
* `Crtl+u` move up half a page
* `Crtl+d` move down half a screen


# Visual mode

* `v` enter visual mode
* `Shift+v` enter visual mode lines only
* `Ctrl+v` enter rectangular visual mode (bloc mode)

Exemple, to ident 3 lines:
* `Shift-v`
* select 3 lines
* `==`

## Buffers (not tabs !)

* `:bn` buffer next
* `:bp` buffer previous
* `:bd` buffer delete (close)
* `:ls` list buffers
* `:bufdo cmd` execute cmd on all open buffers

Exemples:
* `bufdo w` write all buffers
* `bufdo bd` close all buffers

## Windows

* `:vsplit` or `:vsp` vertical split
* `:split` or `:sp` horizontal split
* `Crtl + w` then  `w` navigate through windows
* `Crtl + w` then  `arrow` navigate to arrow directed window
* `Ctrl + w` then `+`  increase the actual viewport size
* `Ctrl + w` then `-`  decrease the actual viewport size
* `Ctrl + w` then `=`  equalize viewports sizes
* `Ctrl + w` then `r`  exchange  viewport positions
* `Ctrl + w` then `R`  exchange viewport in reverse order
* `Ctrl + w` then `q`  close the actual viewport

# Search / Replace

* `:s/toto//` search the first occurence of `toto` on the line
* `:s/toto//g` search all occurences of `toto` on the line
* `:%s/toto//g` search all occurences of `toto` for each line of the buffer (or opened file)
* `n` go to the next occurence of what was previously searched
* `:s/toto/tata/` replace the first occurence of `toto` on the line by  `tata`
* `:s/toto/tata/g` replace all occurences of `toto` on the line by `tata`
* `:%s/toto/tata/g` replace all occurences of `toto` in the buffer by `tata`
* regex allowed ;)


# Mapping

Test in buffer before putting them in your .vimrc to create your own shortcuts
* `:map`  insert + normal mode
* `:nmap` normal mode
* `:imap` interactive mode
* `:iab`  substitue

Exemples:
* `:nmap <tab> :bnext <CR>` pressing `tab` will go to the next buffer in normal mode only
* `:map <C-n> :vsp<CR>` pressing `Crtl + n`  will vertical split the current viewport in normal and insert mode
* `:iab c Class` typing  `c` then `space`  will be expanded to `Class`


## Macros

1. Start with recording: qq
1. Do stuff
1. Stop recording: q
1. Repeat: @q (the first time), @@ after that.
1. Repeat 20 times: 20@@


# External commands

* `:!cmd` executes a linux command (ex\: `:!echo 'toto'`)
If vim runs at project root:
* `:make` works
* `:make target` works great
* `:.! ls` dumps ls result in current window
* `:r! ls` dumps ls result in current window
* `:%!sort` sorts the entire buffer (works best with awk or sed)

## Register

To use when working in the : mode
* `a - z` the named registers
* `"` the unnamed register, containing the text of the last delete or yank
* `%` the current file name
* `#` the alternate file name
* `*` the clipboard contents (X11: primary selection)
* `+` the clipboard contents
* `/` the last search pattern
* `:` the last command-line
* `.` the last inserted text
* `-` the last small (less than a line) delete "

Ex: when you :%s/toto/tata/g to replace toto by tata in the whole buffer

# Marks

* `ma` mark the actual position with the letter `a`
* ``a`` go to mark `a`
* `'a`  go to the beginning of the `a` marked line
* `:marks` show the mark listing

# TODO sort this

* `t` tag example\: ( `dat` deletes from `<xml-style-tag>` to `</xml-style-tag>`)
* `p` paragraphe
* `B` code block ( `{` ou `}` works only for C style code blocks
* `:25` go to line 25

# Usefull Tricks

* `.` repeat the last text changing command
* `*`  on a word to search for the next instance
* `#`  on a word to search for the previous instance
* `ctx` delete everything until character `x` and enter insert mode
* `dtx` delete everything until character `x` (x exclude) and don't enter insert mode
* `dfx` delete everything until character `x` (x include) and don't enter insert mode
* `da(` inside a parenthesis, deletes parenthesis and inner content
* `da"` inside a string, deletes character string content and quotes
* `ytx` copy until character `x`
* `Shift+j` or `J` add the next line to the end of the current line
* `Shift+k` or `K` show the manual page (manual like in `man`)  for the word under the cursor
* `~` modify the under the cursor character case
* `.` repeat the last command
* `:u` undo
* `Crtl + r` redo
* `Crtl + A` increment "under the cursor" number
* `Crtl + X` decrement "under the cursor" number
* `r` replace a letter
* `R` enter replace mode
* `:w !sudo tee %` edit the file anywway if you forgot to call vim with the sudo command
* `:earlier 15m` reverts the document back to how it was 15 minutes ago.
* `:later` revert previous command
* `diw` to delete the current word
* `di(` to delete within the current parens
* `di"` to delete the text between the quotes
* `ci(okok` remplace current parenthesis content with okok
* `C`  cut the rest of the line and switch to insert mod
* `xp` toggle cuurent char position with the following one
* `xP` copy a character
* `ddp`   move current line one row down
* `bde` delete current word
* `bye` copies current word same as `yiw`
* `d$` (or `D`) delete until the end of line
* `g;` goto last insert mode cursor position and stay in normal mode
* `gi` goto last insert mode cursor position and enter insert mode
* `:g/pattern/y` A copy all lines containing pattern in a buffer you can then paste
* `:sh` open shell and mask vim Crtl-D kills the shell and brings vim back !
* `gf` goto file which name is under the cursor
* `%TOhtml` Creates an html rendering of the current file.
* `gg=G` Corrects indentation for entire file



#Plugins

## TComment

### Commands

* `gc{motion}`    Toggle comments (for small comments within one line the &filetype_inline style will be used, if defined)
* `gc<Count>c{motion}`  Toggle comment text with count argument (see |tcomment#Comment()|)
* `gcc`           Toggle comment for the current line
* `gC{motion}`    Comment region
* `gCc`  Comment the current line

### keymapping
* `<c-_><c-_>` Toggle comment on  line
* `<c-_>p` Comment the current inner paragraph
* `<c-_>i` TCommentInline


Tell me about your favorite shortcut in the comments, I will add them !

