<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Magic File Names](Magic-File-Names.html), Previous: [Contents of Directories](Contents-of-Directories.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 25.11 Creating, Copying and Deleting Directories

Most Emacs Lisp file-manipulation functions get errors when used on files that are directories. For example, you cannot delete a directory with `delete-file`. These special functions exist to create and delete directories.

*   Command: **make-directory** *dirname \&optional parents*

    This command creates a directory named `dirname`. If `parents` is non-`nil`, as is always the case in an interactive call, that means to create the parent directories first, if they don’t already exist. `mkdir` is an alias for this.

<!---->

*   Command: **make-empty-file** *filename \&optional parents*

    This command creates an empty file named `filename`. As `make-directory`, this command creates parent directories if `parents` is non-`nil`. If `filename` already exists, this command signals an error.

<!---->

*   Command: **copy-directory** *dirname newname \&optional keep-time parents copy-contents*

    This command copies the directory named `dirname` to `newname`. If `newname` is a directory name, `dirname` will be copied to a subdirectory there. See [Directory Names](Directory-Names.html).

    It always sets the file modes of the copied files to match the corresponding original file.

    The third argument `keep-time` non-`nil` means to preserve the modification time of the copied files. A prefix arg makes `keep-time` non-`nil`.

    The fourth argument `parents` says whether to create parent directories if they don’t exist. Interactively, this happens by default.

    The fifth argument `copy-contents`, if non-`nil`, means to copy the contents of `dirname` directly into `newname` if the latter is a directory name, instead of copying `dirname` into it as a subdirectory.

<!---->

*   Command: **delete-directory** *dirname \&optional recursive trash*

    This command deletes the directory named `dirname`. The function `delete-file` does not work for files that are directories; you must use `delete-directory` for them. If `recursive` is `nil`, and the directory contains any files, `delete-directory` signals an error. If recursive is non-`nil`, there is no error merely because the directory or its files are deleted by some other process before `delete-directory` gets to them.

    `delete-directory` only follows symbolic links at the level of parent directories.

    If the optional argument `trash` is non-`nil` and the variable `delete-by-moving-to-trash` is non-`nil`, this command moves the file into the system Trash instead of deleting it. See [Miscellaneous File Operations](https://www.gnu.org/software/emacs/manual/html_node/emacs/Misc-File-Ops.html#Misc-File-Ops) in The GNU Emacs Manual. When called interactively, `trash` is `t` if no prefix argument is given, and `nil` otherwise.

Next: [Magic File Names](Magic-File-Names.html), Previous: [Contents of Directories](Contents-of-Directories.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
