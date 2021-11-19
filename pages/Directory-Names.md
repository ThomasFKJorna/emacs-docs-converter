

Next: [File Name Expansion](File-Name-Expansion.html), Previous: [Relative File Names](Relative-File-Names.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.9.3 Directory Names

A *directory name* is a string that must name a directory if it names any file at all. A directory is actually a kind of file, and it has a file name (called the *directory file name*, which is related to the directory name but is typically not identical. (This is not quite the same as the usual POSIX terminology.) These two names for the same entity are related by a syntactic transformation. On GNU and other POSIX-like systems, this is simple: to obtain a directory name, append a ‘`/`’ to a directory file name that does not already end in ‘`/`’. On MS-DOS the relationship is more complicated.

The difference between a directory name and a directory file name is subtle but crucial. When an Emacs variable or function argument is described as being a directory name, a directory file name is not acceptable. When `file-name-directory` returns a string, that is always a directory name.

The following two functions convert between directory names and directory file names. They do nothing special with environment variable substitutions such as ‘`$HOME`’, and the constructs ‘`~`’, ‘`.`’ and ‘`..`’.

*   Function: **file-name-as-directory** *filename*

    This function returns a string representing `filename` in a form that the operating system will interpret as the name of a directory (a directory name). On most systems, this means appending a slash to the string (if it does not already end in one).

    ```lisp
    (file-name-as-directory "~rms/lewis")
         ⇒ "~rms/lewis/"
    ```

<!---->

*   Function: **directory-name-p** *filename*

    This function returns non-`nil` if `filename` ends with a directory separator character. This is the forward slash ‘`/`’ on GNU and other POSIX-like systems; MS-Windows and MS-DOS recognize both the forward slash and the backslash ‘`\`’ as directory separators.

<!---->

*   Function: **directory-file-name** *dirname*

    This function returns a string representing `dirname` in a form that the operating system will interpret as the name of a file (a directory file name). On most systems, this means removing the final directory separators from the string, unless the string consists entirely of directory separators.

    ```lisp
    (directory-file-name "~lewis/")
         ⇒ "~lewis"
    ```

Given a directory name, you can combine it with a relative file name using `concat`:

```lisp
(concat dirname relfile)
```

Be sure to verify that the file name is relative before doing that. If you use an absolute file name, the results could be syntactically invalid or refer to the wrong file.

If you want to use a directory file name in making such a combination, you must first convert it to a directory name using `file-name-as-directory`:

```lisp
(concat (file-name-as-directory dirfile) relfile)
```

Don’t try concatenating a slash by hand, as in

```lisp
;;; Wrong!
(concat dirfile "/" relfile)
```

because this is not portable. Always use `file-name-as-directory`.

To avoid the issues mentioned above, or if the `dirname` value might be `nil` (for example, from an element of `load-path`), use:

```lisp
(expand-file-name relfile dirname)
```

However, `expand-file-name` expands leading ‘`~`’ in `relfile`, which may not be what you want. See [File Name Expansion](File-Name-Expansion.html).

To convert a directory name to its abbreviation, use this function:

*   Function: **abbreviate-file-name** *filename*

    This function returns an abbreviated form of `filename`. It applies the abbreviations specified in `directory-abbrev-alist` (see [File Aliases](https://www.gnu.org/software/emacs/manual/html_node/emacs/File-Aliases.html#File-Aliases) in The GNU Emacs Manual), then substitutes ‘`~`’ for the user’s home directory if the argument names a file in the home directory or one of its subdirectories. If the home directory is a root directory, it is not replaced with ‘`~`’, because this does not make the result shorter on many systems.

    You can use this function for directory names and for file names, because it recognizes abbreviations even as part of the name.

Next: [File Name Expansion](File-Name-Expansion.html), Previous: [Relative File Names](Relative-File-Names.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
