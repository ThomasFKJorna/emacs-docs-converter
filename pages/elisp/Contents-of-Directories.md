

### 25.10 Contents of Directories

A directory is a kind of file that contains other files entered under various names. Directories are a feature of the file system.

Emacs can list the names of the files in a directory as a Lisp list, or display the names in a buffer using the `ls` shell command. In the latter case, it can optionally display information about each file, depending on the options passed to the `ls` command.

### Function: **directory-files** *directory \&optional full-name match-regexp nosort*

This function returns a list of the names of the files in the directory `directory`. By default, the list is in alphabetical order.

If `full-name` is non-`nil`, the function returns the files’ absolute file names. Otherwise, it returns the names relative to the specified directory.

If `match-regexp` is non-`nil`, this function returns only those file names that contain a match for that regular expression—the other file names are excluded from the list. On case-insensitive filesystems, the regular expression matching is case-insensitive.

If `nosort` is non-`nil`, `directory-files` does not sort the list, so you get the file names in no particular order. Use this if you want the utmost possible speed and don’t care what order the files are processed in. If the order of processing is visible to the user, then the user will probably be happier if you do sort the names.

```lisp
(directory-files "~lewis")
     ⇒ ("#foo#" "#foo.el#" "." ".."
         "dired-mods.el" "files.texi"
         "files.texi.~1~")
```

An error is signaled if `directory` is not the name of a directory that can be read.

### Function: **directory-files-recursively** *directory regexp \&optional include-directories predicate follow-symlinks*

Return all files under `directory` whose names match `regexp`. This function searches the specified `directory` and its sub-directories, recursively, for files whose basenames (i.e., without the leading directories) match the specified `regexp`, and returns a list of the absolute file names of the matching files (see [absolute file names](Relative-File-Names.html)). The file names are returned in depth-first order, meaning that files in some sub-directory are returned before the files in its parent directory. In addition, matching files found in each subdirectory are sorted alphabetically by their basenames. By default, directories whose names match `regexp` are omitted from the list, but if the optional argument `include-directories` is non-`nil`, they are included.

By default, all subdirectories are descended into. If `predicate` is `t`, errors when trying to descend into a subdirectory (for instance, if it’s not readable by this user) are ignored. If it’s neither `nil` nor `t`, it should be a function that takes one parameter (the subdirectory name) and should return non-`nil` if the directory is to be descended into.

Symbolic links to subdirectories are not followed by default, but if `follow-symlinks` is non-`nil`, they are followed.

### Function: **locate-dominating-file** *file name*

Starting at `file`, go up the directory tree hierarchy looking for the first directory where `name`, a string, exists, and return that directory. If `file` is a file, its directory will serve as the starting point for the search; otherwise `file` should be a directory from which to start. The function looks in the starting directory, then in its parent, then in its parent’s parent, etc., until it either finds a directory with `name` or reaches the root directory of the filesystem without finding `name` – in the latter case the function returns `nil`.

The argument `name` can also be a predicate function. The predicate is called for every directory examined by the function, starting from `file` (even if `file` is not a directory). It is called with one argument (the file or directory) and should return non-`nil` if that directory is the one it is looking for.

### Function: **directory-files-and-attributes** *directory \&optional full-name match-regexp nosort id-format*

This is similar to `directory-files` in deciding which files to report on and how to report their names. However, instead of returning a list of file names, it returns for each file a list `(filename . attributes)`, where `attributes` is what `file-attributes` returns for that file. The optional argument `id-format` has the same meaning as the corresponding argument to `file-attributes` (see [Definition of file-attributes](File-Attributes.html#Definition-of-file_002dattributes)).

### Function: **file-expand-wildcards** *pattern \&optional full*

This function expands the wildcard pattern `pattern`, returning a list of file names that match it.

If `pattern` is written as an absolute file name, the values are absolute also.

If `pattern` is written as a relative file name, it is interpreted relative to the current default directory. The file names returned are normally also relative to the current default directory. However, if `full` is non-`nil`, they are absolute.

### Function: **insert-directory** *file switches \&optional wildcard full-directory-p*

This function inserts (in the current buffer) a directory listing for directory `file`, formatted with `ls` according to `switches`. It leaves point after the inserted text. `switches` may be a string of options, or a list of strings representing individual options.

The argument `file` may be either a directory or a file specification including wildcard characters. If `wildcard` is non-`nil`, that means treat `file` as a file specification with wildcards.

If `full-directory-p` is non-`nil`, that means the directory listing is expected to show the full contents of a directory. You should specify `t` when `file` is a directory and switches do not contain ‘`-d`’. (The ‘`-d`’ option to `ls` says to describe a directory itself as a file, rather than showing its contents.)

On most systems, this function works by running a directory listing program whose name is in the variable `insert-directory-program`. If `wildcard` is non-`nil`, it also runs the shell specified by `shell-file-name`, to expand the wildcards.

MS-DOS and MS-Windows systems usually lack the standard Unix program `ls`, so this function emulates the standard Unix program `ls` with Lisp code.

As a technical detail, when `switches` contains the long ‘`--dired`’ option, `insert-directory` treats it specially, for the sake of dired. However, the normally equivalent short ‘`-D`’ option is just passed on to `insert-directory-program`, as any other option.

### Variable: **insert-directory-program**

This variable’s value is the program to run to generate a directory listing for the function `insert-directory`. It is ignored on systems which generate the listing with Lisp code.
