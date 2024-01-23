---
published: false
id: search-replace
slug: search-replace
title: Search Replace
description: search replace
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## Search & Replace Special Characters

### Syntax

```
:[range]s/old-string/new-string/options
```

range
`comma(,)` : 1,10 means line 1 to 10
`colon(;)` : start from first number
`period(.)` : current line
`dollar($)` : last line
`percentage(%)` : every lines

options
`c` : confirmation

## Example text

```
This is a test
test is awesome, need more test
testing Search and Replace in this example.
now, test is done.
search for /usr/location/bin
```

## Basic Example

```bash
:10;+2s/test/TEST/g # from line 10 to 12(10+2) change is to IS
:10;+2s/test/TEST/gc # from line 10 to 12(10+2) change is to IS with confirmation
:1,$s/test/TEST/g # from line 1 to last line
```

## Advanced Example

### Replace with visual block word

- visual block of word will be recognized as an old string

1. `/test`
2. `:%s//TEST/g`

### Replace without escape(`\`) character

use pound(#) sign to use literal special charater
(both are same result)

```
:%s#/usr/#./
:%s/\/usr\//.\/
```

### Notes:

- The `%` is a line range that specifies *every* line
- The `g` flag says to substitute *all* occurrences in each line
- The `c` flag causes vim to ask you to confirm each replacement individually (you might want to leave this out)

## Search & Replace in specific directory

Let's say we have a simple project structure like this:

```
.
+-- greeting.txt
+-- info/
    +-- age.txt
```

`greeting.txt` looks like

```
Hello, my name is Sam, and Sam is my name
```

and `info/age.txt` looks like

```
Sam's age is 25
```

Let's say we want to replace all occurrences of `Sam` with `Bob`. Here's what we would do:

### 1. Set working directory

Make sure Vim's current working directory is the root of the project:

```
:cd {path to root directory}
```

You can use `:pwd` to print the current working directory and ensure that it is correct.

### 2. Find files that contain 'Sam'

Use Vim's `:vimgrep` command to search for all occurrences of `Sam` within the project:

```
:vimgrep /Sam/gj **/*
```

### Notes:

- `Sam` is the search "pattern" sandwiched between two forward slashes
- The `**/*` says to search in all files recursively
- The `g` flag says to search for all occurrences in each line (this is actually overkill here, but it does not hurt either)
  - The `j` flag prevents vim from automatically jumping to the first match

This will populate the quickfix list with all instances of `Sam`. If you want to view the quickfix list, you can use the Vim command `:copen`
![[search-and-replace-in-quick-fix-list.png]]

### 3. Substitute within all files that contain 'Sam'

Now we want to run Vim's `:substitute` command inside every file in the quickfix list. We can do this using the `:cfdo {cmd}` command which executes `{cmd}` in each file in the quickfix list. The specific `{cmd}` we want to use is `:substitute` or `:s` for short. Adding the `update` command at the end ensures that each file is saved before moving on to the next one (this is necessary if you don't `:set hidden`). The full line would look like:

```
:cfdo %s/Sam/Bob/gc | update
```

If you do `:set hidden`, you can optionally leave off `| update`, and then do `:cfdo update` or `:wall`.

## Tips

get state, setState from useState

```
:'<,'>s/.*\[\(.*\)\].*/\1,
```

## Ref

- https://www.youtube.com/watch?v=U9bsqulWgqc&t=326
- https://vi.stackexchange.com/questions/2776/vim-search-replace-all-files-in-current-project-folder
