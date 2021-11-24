

#### 25.6.3 Truenames

The *truename* of a file is the name that you get by following symbolic links at all levels until none remain, then simplifying away ‘`.`’ and ‘`..`’ appearing as name components. This results in a sort of canonical name for the file. A file does not always have a unique truename; the number of distinct truenames a file has is equal to the number of hard links to the file. However, truenames are useful because they eliminate symbolic links as a cause of name variation.

### Function: **file-truename** *filename*

This function returns the truename of the file `filename`. If the argument is not an absolute file name, this function first expands it against `default-directory`.

This function does not expand environment variables. Only `substitute-in-file-name` does that. See [Definition of substitute-in-file-name](File-Name-Expansion.html#Definition-of-substitute_002din_002dfile_002dname).

If you may need to follow symbolic links preceding ‘`..`’ appearing as a name component, call `file-truename` without prior direct or indirect calls to `expand-file-name`. Otherwise, the file name component immediately preceding ‘`..`’ will be simplified away before `file-truename` is called. To eliminate the need for a call to `expand-file-name`, `file-truename` handles ‘`~`’ in the same way that `expand-file-name` does.

If the target of a symbolic links has remote file name syntax, `file-truename` returns it quoted. See [Functions that Expand Filenames](File-Name-Expansion.html).

### Function: **file-chase-links** *filename \&optional limit*

This function follows symbolic links, starting with `filename`, until it finds a file name which is not the name of a symbolic link. Then it returns that file name. This function does *not* follow symbolic links at the level of parent directories.

If you specify a number for `limit`, then after chasing through that many links, the function just returns what it has even if that is still a symbolic link.

To illustrate the difference between `file-chase-links` and `file-truename`, suppose that `/usr/foo` is a symbolic link to the directory `/home/foo`, and `/home/foo/hello` is an ordinary file (or at least, not a symbolic link) or nonexistent. Then we would have:

```lisp
(file-chase-links "/usr/foo/hello")
     ;; This does not follow the links in the parent directories.
     ⇒ "/usr/foo/hello"
(file-truename "/usr/foo/hello")
     ;; Assuming that /home is not a symbolic link.
     ⇒ "/home/foo/hello"
```

### Function: **file-equal-p** *file1 file2*

This function returns `t` if the files `file1` and `file2` name the same file. This is similar to comparing their truenames, except that remote file names are also handled in an appropriate manner. If `file1` or `file2` does not exist, the return value is unspecified.

### Function: **file-name-case-insensitive-p** *filename*

Sometimes file names or their parts need to be compared as strings, in which case it’s important to know whether the underlying filesystem is case-insensitive. This function returns `t` if file `filename` is on a case-insensitive filesystem. It always returns `t` on MS-DOS and MS-Windows. On Cygwin and macOS, filesystems may or may not be case-insensitive, and the function tries to determine case-sensitivity by a runtime test. If the test is inconclusive, the function returns `t` on Cygwin and `nil` on macOS.

Currently this function always returns `nil` on platforms other than MS-DOS, MS-Windows, Cygwin, and macOS. It does not detect case-insensitivity of mounted filesystems, such as Samba shares or NFS-mounted Windows volumes. On remote hosts, it assumes `t` for the ‘`smb`’ method. For all other connection methods, runtime tests are performed.

### Function: **file-in-directory-p** *file dir*

This function returns `t` if `file` is a file in directory `dir`, or in a subdirectory of `dir`. It also returns `t` if `file` and `dir` are the same directory. It compares the truenames of the two directories. If `dir` does not name an existing directory, the return value is `nil`.

### Function: **vc-responsible-backend** *file*

This function determines the responsible VC backend of the given `file`. For example, if `emacs.c` is a file tracked by Git, `(vc-responsible-backend "emacs.c")` returns ‘`Git`’. Note that if `file` is a symbolic link, `vc-responsible-backend` will not resolve it—the backend of the symbolic link file itself is reported. To get the backend VC of the file to which `file` refers, wrap `file` with a symbolic link resolving function such as `file-chase-links`:

```lisp
(vc-responsible-backend (file-chase-links "emacs.c"))
```
