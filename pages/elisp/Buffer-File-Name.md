

### 27.4 Buffer File Name

The *buffer file name* is the name of the file that is visited in that buffer. When a buffer is not visiting a file, its buffer file name is `nil`. Most of the time, the buffer name is the same as the nondirectory part of the buffer file name, but the buffer file name and the buffer name are distinct and can be set independently. See [Visiting Files](Visiting-Files.html).

### Function: **buffer-file-name** *\&optional buffer*

This function returns the absolute file name of the file that `buffer` is visiting. If `buffer` is not visiting any file, `buffer-file-name` returns `nil`. If `buffer` is not supplied, it defaults to the current buffer.

```lisp
(buffer-file-name (other-buffer))
     ⇒ "/usr/user/lewis/manual/files.texi"
```

### Variable: **buffer-file-name**

This buffer-local variable contains the name of the file being visited in the current buffer, or `nil` if it is not visiting a file. It is a permanent local variable, unaffected by `kill-all-local-variables`.

```lisp
buffer-file-name
     ⇒ "/usr/user/lewis/manual/buffers.texi"
```

It is risky to change this variable’s value without doing various other things. Normally it is better to use `set-visited-file-name` (see below); some of the things done there, such as changing the buffer name, are not strictly necessary, but others are essential to avoid confusing Emacs.

### Variable: **buffer-file-truename**

This buffer-local variable holds the abbreviated truename of the file visited in the current buffer, or `nil` if no file is visited. It is a permanent local, unaffected by `kill-all-local-variables`. See [Truenames](Truenames.html), and [abbreviate-file-name](Directory-Names.html#abbreviate_002dfile_002dname).

### Variable: **buffer-file-number**

This buffer-local variable holds the file number and directory device number of the file visited in the current buffer, or `nil` if no file or a nonexistent file is visited. It is a permanent local, unaffected by `kill-all-local-variables`.

The value is normally a list of the form `(filenum devnum)`. This pair of numbers uniquely identifies the file among all files accessible on the system. See the function `file-attributes`, in [File Attributes](File-Attributes.html), for more information about them.

If `buffer-file-name` is the name of a symbolic link, then both numbers refer to the recursive target.

### Function: **get-file-buffer** *filename*

This function returns the buffer visiting file `filename`. If there is no such buffer, it returns `nil`. The argument `filename`, which must be a string, is expanded (see [File Name Expansion](File-Name-Expansion.html)), then compared against the visited file names of all live buffers. Note that the buffer’s `buffer-file-name` must match the expansion of `filename` exactly. This function will not recognize other names for the same file.

```lisp
(get-file-buffer "buffers.texi")
    ⇒ #<buffer buffers.texi>
```

In unusual circumstances, there can be more than one buffer visiting the same file name. In such cases, this function returns the first such buffer in the buffer list.

### Function: **find-buffer-visiting** *filename \&optional predicate*

This is like `get-file-buffer`, except that it can return any buffer visiting the file *possibly under a different name*. That is, the buffer’s `buffer-file-name` does not need to match the expansion of `filename` exactly, it only needs to refer to the same file. If `predicate` is non-`nil`, it should be a function of one argument, a buffer visiting `filename`. The buffer is only considered a suitable return value if `predicate` returns non-`nil`. If it can not find a suitable buffer to return, `find-buffer-visiting` returns `nil`.

### Command: **set-visited-file-name** *filename \&optional no-query along-with-file*

If `filename` is a non-empty string, this function changes the name of the file visited in the current buffer to `filename`. (If the buffer had no visited file, this gives it one.) The *next time* the buffer is saved it will go in the newly-specified file.

This command marks the buffer as modified, since it does not (as far as Emacs knows) match the contents of `filename`, even if it matched the former visited file. It also renames the buffer to correspond to the new file name, unless the new name is already in use.

If `filename` is `nil` or the empty string, that stands for “no visited file”. In this case, `set-visited-file-name` marks the buffer as having no visited file, without changing the buffer’s modified flag.

Normally, this function asks the user for confirmation if there already is a buffer visiting `filename`. If `no-query` is non-`nil`, that prevents asking this question. If there already is a buffer visiting `filename`, and the user confirms or `no-query` is non-`nil`, this function makes the new buffer name unique by appending a number inside of ‘`<…>`’ to `filename`.

If `along-with-file` is non-`nil`, that means to assume that the former visited file has been renamed to `filename`. In this case, the command does not change the buffer’s modified flag, nor the buffer’s recorded last file modification time as reported by `visited-file-modtime` (see [Modification Time](Modification-Time.html)). If `along-with-file` is `nil`, this function clears the recorded last file modification time, after which `visited-file-modtime` returns zero.

When the function `set-visited-file-name` is called interactively, it prompts for `filename` in the minibuffer.

### Variable: **list-buffers-directory**

This buffer-local variable specifies a string to display in a buffer listing where the visited file name would go, for buffers that don’t have a visited file name. Dired buffers use this variable.
