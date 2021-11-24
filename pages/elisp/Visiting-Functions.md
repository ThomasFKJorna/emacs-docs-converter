

#### 25.1.1 Functions for Visiting Files

This section describes the functions normally used to visit files. For historical reasons, these functions have names starting with ‘`find-`’ rather than ‘`visit-`’. See [Buffer File Name](Buffer-File-Name.html), for functions and variables that access the visited file name of a buffer or that find an existing buffer by its visited file name.

In a Lisp program, if you want to look at the contents of a file but not alter it, the fastest way is to use `insert-file-contents` in a temporary buffer. Visiting the file is not necessary and takes longer. See [Reading from Files](Reading-from-Files.html).

### Command: **find-file** *filename \&optional wildcards*

This command selects a buffer visiting the file `filename`, using an existing buffer if there is one, and otherwise creating a new buffer and reading the file into it. It also returns that buffer.

Aside from some technical details, the body of the `find-file` function is basically equivalent to:

```lisp
(switch-to-buffer (find-file-noselect filename nil nil wildcards))
```

(See `switch-to-buffer` in [Switching Buffers](Switching-Buffers.html).)

If `wildcards` is non-`nil`, which is always true in an interactive call, then `find-file` expands wildcard characters in `filename` and visits all the matching files.

When `find-file` is called interactively, it prompts for `filename` in the minibuffer.

### Command: **find-file-literally** *filename*

This command visits `filename`, like `find-file` does, but it does not perform any format conversions (see [Format Conversion](Format-Conversion.html)), character code conversions (see [Coding Systems](Coding-Systems.html)), or end-of-line conversions (see [End of line conversion](Coding-System-Basics.html)). The buffer visiting the file is made unibyte, and its major mode is Fundamental mode, regardless of the file name. File local variable specifications in the file (see [File Local Variables](File-Local-Variables.html)) are ignored, and automatic decompression and adding a newline at the end of the file due to `require-final-newline` (see [require-final-newline](Saving-Buffers.html)) are also disabled.

Note that if Emacs already has a buffer visiting the same file non-literally, it will not visit the same file literally, but instead just switch to the existing buffer. If you want to be sure of accessing a file’s contents literally, you should create a temporary buffer and then read the file contents into it using `insert-file-contents-literally` (see [Reading from Files](Reading-from-Files.html)).

### Function: **find-file-noselect** *filename \&optional nowarn rawfile wildcards*

This function is the guts of all the file-visiting functions. It returns a buffer visiting the file `filename`. You may make the buffer current or display it in a window if you wish, but this function does not do so.

The function returns an existing buffer if there is one; otherwise it creates a new buffer and reads the file into it. When `find-file-noselect` uses an existing buffer, it first verifies that the file has not changed since it was last visited or saved in that buffer. If the file has changed, this function asks the user whether to reread the changed file. If the user says ‘`yes`’, any edits previously made in the buffer are lost.

Reading the file involves decoding the file’s contents (see [Coding Systems](Coding-Systems.html)), including end-of-line conversion, and format conversion (see [Format Conversion](Format-Conversion.html)). If `wildcards` is non-`nil`, then `find-file-noselect` expands wildcard characters in `filename` and visits all the matching files.

This function displays warning or advisory messages in various peculiar cases, unless the optional argument `nowarn` is non-`nil`. For example, if it needs to create a buffer, and there is no file named `filename`, it displays the message ‘`(New file)`’ in the echo area, and leaves the buffer empty.

The `find-file-noselect` function normally calls `after-find-file` after reading the file (see [Subroutines of Visiting](Subroutines-of-Visiting.html)). That function sets the buffer major mode, parses local variables, warns the user if there exists an auto-save file more recent than the file just visited, and finishes by running the functions in `find-file-hook`.

If the optional argument `rawfile` is non-`nil`, then `after-find-file` is not called, and the `find-file-not-found-functions` are not run in case of failure. What’s more, a non-`nil` `rawfile` value suppresses coding system conversion and format conversion.

The `find-file-noselect` function usually returns the buffer that is visiting the file `filename`. But, if wildcards are actually used and expanded, it returns a list of buffers that are visiting the various files.

```lisp
(find-file-noselect "/etc/fstab")
     ⇒ #<buffer fstab>
```

### Command: **find-file-other-window** *filename \&optional wildcards*

This command selects a buffer visiting the file `filename`, but does so in a window other than the selected window. It may use another existing window or split a window; see [Switching Buffers](Switching-Buffers.html).

When this command is called interactively, it prompts for `filename`.

### Command: **find-file-read-only** *filename \&optional wildcards*

This command selects a buffer visiting the file `filename`, like `find-file`, but it marks the buffer as read-only. See [Read Only Buffers](Read-Only-Buffers.html), for related functions and variables.

When this command is called interactively, it prompts for `filename`.

### User Option: **find-file-wildcards**

If this variable is non-`nil`, then the various `find-file` commands check for wildcard characters and visit all the files that match them (when invoked interactively or when their `wildcards` argument is non-`nil`). If this option is `nil`, then the `find-file` commands ignore their `wildcards` argument and never treat wildcard characters specially.

### User Option: **find-file-hook**

The value of this variable is a list of functions to be called after a file is visited. The file’s local-variables specification (if any) will have been processed before the hooks are run. The buffer visiting the file is current when the hook functions are run.

This variable is a normal hook. See [Hooks](Hooks.html).

### Variable: **find-file-not-found-functions**

The value of this variable is a list of functions to be called when `find-file` or `find-file-noselect` is passed a nonexistent file name. `find-file-noselect` calls these functions as soon as it detects a nonexistent file. It calls them in the order of the list, until one of them returns non-`nil`. `buffer-file-name` is already set up.

This is not a normal hook because the values of the functions are used, and in many cases only some of the functions are called.

### Variable: **find-file-literally**

This buffer-local variable, if set to a non-`nil` value, makes `save-buffer` behave as if the buffer were visiting its file literally, i.e., without conversions of any kind. The command `find-file-literally` sets this variable’s local value, but other equivalent functions and commands can do that as well, e.g., to avoid automatic addition of a newline at the end of the file. This variable is permanent local, so it is unaffected by changes of major modes.
