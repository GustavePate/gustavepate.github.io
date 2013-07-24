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

# Moves

* `s` sentence
* `w` word 
* `W` WORD
* `t` tag example\: ( `dat` deletes from `<xml-style-tag>` to `</xml-style-tag>`)
* `p` paragraphe
* `B` code block ( `{` ou `}` works only for C style code blocks
* `)` sentence end
* `}` paragraph end
* `$` line end 
* `^` line beginning
* `gg` file beginning
* `G` end of file
* `gD` go to the current method definition
* `%` on a `(` or `\[` go to the ending brace `)` or `\]` otherwise go to the preceding one. 
* `:25` go to line 25

## Parenthèsis

* `(` ou `)` -  ( ... )
* `[` ou `]` - \[ .. \] 
* `<` ou `>` -  <..>
* `{` ou `}` - 

# Marks

* `ma` mark the actual position with the letter `a`
* ``a`` go to mark `a`
* `'a`  go to the beginning of the `a` marked line
* `:marks` show the mark listing

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
* `Crtl + w` then  `arrow` navigate through windows
* `Ctrl + w` then `+`  increase the actual viewport size
* `Ctrl + w` then `-`  decrease the actual viewport size
* `Ctrl + w` then `=`  equalize viewports sizes
* `Ctrl + w` then `r`  exchange  viewport positions 
* `Ctrl + w` then `R`  exchange viewport in reverse order
* `Ctrl + w` then `q`  close the actual viewport


# External commands

* `:!cmd` executes a linux command (ex\: `:!echo 'toto'`)
If vim runs at project root:
* `:make` works
* `:make target` works great


# Usefull Tricks

* `ctx` delete everything until character `x` and enter insert mode
* `dtx` delete everything until character `x` and don't enter insert mode
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


# Search / Replace

* `:s/toto//` search the first occurence of `toto` on the line
* `:s/toto//g` search all occurences of `toto` on the line
* `:%s/toto//g` search all occurences of `toto` for each line of the buffer (or opened file)
* `n` go to the next occurence of what was previously searched 
* `:s/toto/tata/` replace the first occurence of `toto` on the line by  `tata`  
* `:s/toto/tata/g` replace all occurences of `toto` on the line by `tata`  
* `:%s/toto/tata/g` replace all occurences of `toto` in the buffer by `tata`  
* regex allowed ;)

Tell me about your favorite shortcut in the comments, I will add them !

