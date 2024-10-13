---
title: "Vim for Hackers"
date: 2024-05-01T11:00:48+02:00
description: "Advanced hacking skills with VIM to give you superpowers"
tags:
- tutorial
- til
- tips
---

{{< lead >}}
`vim` is a double edged sword. Use it properly and you'll see the benefits,
don't use it properly and it's just going to cause more pain and sorrow.
{{< /lead >}}

Handy vim cheatsheet: https://vim.rtorr.com

# How to add text at the end of each line

Use-case: assume you just copied a whole column of text and just want to
append a comma at the end

```vim
:%s/$/,/
```

```vim
:'<,'>s/$/,/
```


{{< alert "lightbulb" >}}
`'<,'>` assumes you selected text aka _visual select_
{{< /alert >}}

```vim
:'<,'>norm A,
```

# Remove lines that (do not) contain a specific word

Use-case: you created a Kubernetes Secret via `kubectl create secret` and want
to remove all lines containing `creationTimestamp: null`

```vim
:g /word/d
```

The reverse of this (meaning, remove lines that **don't contain** specific
word) would look like this

```vim
:g!/word/d
```

# delete all blank lines

```vim
:g/^$/d
```

# copy file contents to clipboard

```vim
:%w !pbcopy
```

# replace text matching X with Y

with confirmation

```vim
:%s/replacethis/withthis/gc
```

without confirmation

```vim
:%s/replacethis/withthis/g
```

# move existing window to existing tab

As a window is just a viewport into a loaded buffer, you have to:

1. Note the buffer number displayed in the current window.
2. `:close!` the window.
3. Switch to the existing target tab page.
4. `:sbuffer` the buffer number to re-open it.

# save and restore multiple different sessions

Use-case: assume you just upgraded vim or Terminal app and just want to
restart them

```vim
:mksession ~/mysession.vim
```

source session to restore everything back

```vim
:source ~/mysession.vim
```

open vim with the session

```vim
$ vim -S ~/mysession.vim
```

# open filename under cursor like <kbd>gf</kbd>, but in a new tab

- <kbd>gf</kbd> - Edit existing file under cursor in same window
- <kbd>C</kbd>-<kbd>W</kbd> <kbd>f</kbd> - Edit existing file under cursor in split window
- <kbd>C</kbd>-<kbd>W</kbd> <kbd>C</kbd>-<kbd>F</kbd> - Edit existing file under cursor in split window
- <kbd>C</kbd>-<kbd>W</kbd> <kbd>gf</kbd> - Edit existing file under cursor in new tabpage

# make a new directory or file in `netrw`, vim's file explorer

If you are in the file explorer mode, you can use:

- `d` for creating a directory
- `%` for creating a new file

You can get into the explorer mode with issuing a command `:Sexplore` or `:Vexplore`

There is no need to call external commands with `!`

# find and replace all instances of specific string in multiple files in vim

The general workflow is:

1. Search for your pattern across the project.
2. Operate on each match (safer, slower) or on each file with matches (riskier, faster).
3. Write your changes.

The first step can be done with any command that populates the quickfix list: `:help :vimgrep`, `:help :grep`, something from a third-party plugin, etc.

Taking `:grep` as an example:

```
:grep foo **/*.js

```

will populate the quickfix list with an entry for every `foo` found in `*.js` files in the current directory and subcategories. You can see the list with `:cwindow`.

The second step involves `:help :cdo` or `:help :cfdo`:

```
:cdo s/foo/bar/gc

```

which will substitute every `foo` with `bar` on each line in the quickfix list and ask for confirmation. With `:cfdo` it would look like that:

```
:cfdo %s/foo/bar/gc

```

If you are super confident, you can drop the `c` at the end. See `:help :s_flags`.

The third step involves `:help :update`:

```
:cfdo update

```

which will write every file in the quickfix list to disk if they have been changed.

In short:

```
:gr foo **/*.js
:cdo s/foo/bar/gc
:cfdo up

```

# Remove all unwanted whitespaces

Ever had those pesky trailing whitespaces? How about those on a newline? Well, this trick should do it

```vim
:%s/\s\+$//e
```
{{< alert "lightbulb" >}}
In a search,`\s` finds whitespace (a space or a tab), and `\+` finds one or more occurrences.
The following command deletes any trailing whitespace at the end of each line. If no trailing whitespace is found no change occurs, and the `e` flag means no error is displayed
{{< /alert >}}

