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

Next: [Files and Storage](Files-and-Storage.html), Previous: [Information about Files](Information-about-Files.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 25.7 Changing File Names and Attributes

The functions in this section rename, copy, delete, link, and set the modes (permissions) of files. Typically, they signal a `file-error` error if they fail to perform their function, reporting the system-dependent error message that describes the reason for the failure. If they fail because a file is missing, they signal a `file-missing` error instead.

For performance, the operating system may cache or alias changes made by these functions instead of writing them immediately to secondary storage. See [Files and Storage](Files-and-Storage.html).

In the functions that have an argument `newname`, if this argument is a directory name it is treated as if the nondirectory part of the source name were appended. Typically, a directory name is one that ends in ‘`/`’ (see [Directory Names](Directory-Names.html)). For example, if the old name is `a/b/c`, the `newname` `d/e/f/` is treated as if it were `d/e/f/c`. This special treatment does not apply if `newname` is not a directory name but names a file that is a directory; for example, the `newname` `d/e/f` is left as-is even if `d/e/f` happens to be a directory.

In the functions that have an argument `newname`, if a file by the name of `newname` already exists, the actions taken depend on the value of the argument `ok-if-already-exists`:

*   Signal a `file-already-exists` error if `ok-if-already-exists` is `nil`.
*   Request confirmation if `ok-if-already-exists` is a number.
*   Replace the old file without confirmation if `ok-if-already-exists` is any other value.

<!---->

*   Command: **add-name-to-file** *oldname newname \&optional ok-if-already-exists*

    This function gives the file named `oldname` the additional name `newname`. This means that `newname` becomes a new hard link to `oldname`.

    If `newname` is a symbolic link, its directory entry is replaced, not the directory entry it points to. If `oldname` is a symbolic link, this function might or might not follow the link; it does not follow the link on GNU platforms. If `oldname` is a directory, this function typically fails, although for the superuser on a few old-fashioned non-GNU platforms it can succeed and create a filesystem that is not tree-structured.

    In the first part of the following example, we list two files, `foo` and `foo3`.

        $ ls -li fo*
        81908 -rw-rw-rw- 1 rms rms 29 Aug 18 20:32 foo
        84302 -rw-rw-rw- 1 rms rms 24 Aug 18 20:31 foo3

    Now we create a hard link, by calling `add-name-to-file`, then list the files again. This shows two names for one file, `foo` and `foo2`.

        (add-name-to-file "foo" "foo2")
             ⇒ nil

    ```
    ```

        $ ls -li fo*
        81908 -rw-rw-rw- 2 rms rms 29 Aug 18 20:32 foo
        81908 -rw-rw-rw- 2 rms rms 29 Aug 18 20:32 foo2
        84302 -rw-rw-rw- 1 rms rms 24 Aug 18 20:31 foo3

    Finally, we evaluate the following:

        (add-name-to-file "foo" "foo3" t)

    and list the files again. Now there are three names for one file: `foo`, `foo2`, and `foo3`. The old contents of `foo3` are lost.

        (add-name-to-file "foo1" "foo3")
             ⇒ nil

    ```
    ```

        $ ls -li fo*
        81908 -rw-rw-rw- 3 rms rms 29 Aug 18 20:32 foo
        81908 -rw-rw-rw- 3 rms rms 29 Aug 18 20:32 foo2
        81908 -rw-rw-rw- 3 rms rms 29 Aug 18 20:32 foo3

    This function is meaningless on operating systems where multiple names for one file are not allowed. Some systems implement multiple names by copying the file instead.

    See also `file-nlinks` in [File Attributes](File-Attributes.html).

<!---->

*   Command: **rename-file** *filename newname \&optional ok-if-already-exists*

    This command renames the file `filename` as `newname`.

    If `filename` has additional names aside from `filename`, it continues to have those names. In fact, adding the name `newname` with `add-name-to-file` and then deleting `filename` has the same effect as renaming, aside from momentary intermediate states and treatment of errors, directories and symbolic links.

    This command does not follow symbolic links. If `filename` is a symbolic link, this command renames the symbolic link, not the file it points to. If `newname` is a symbolic link, its directory entry is replaced, not the directory entry it points to.

    This command does nothing if `filename` and `newname` are the same directory entry, i.e., if they refer to the same parent directory and give the same name within that directory. Otherwise, if `filename` and `newname` name the same file, this command does nothing on POSIX-conforming systems, and removes `filename` on some non-POSIX systems.

    If `newname` exists, then it must be an empty directory if `oldname` is a directory and a non-directory otherwise.

<!---->

*   Command: **copy-file** *oldname newname \&optional ok-if-already-exists time preserve-uid-gid preserve-extended-attributes*

    This command copies the file `oldname` to `newname`. An error is signaled if `oldname` is not a regular file. If `newname` names a directory, it copies `oldname` into that directory, preserving its final name component.

    This function follows symbolic links, except that it does not follow a dangling symbolic link to create `newname`.

    If `time` is non-`nil`, then this function gives the new file the same last-modified time that the old one has. (This works on only some operating systems.) If setting the time gets an error, `copy-file` signals a `file-date-error` error. In an interactive call, a prefix argument specifies a non-`nil` value for `time`.

    If argument `preserve-uid-gid` is `nil`, we let the operating system decide the user and group ownership of the new file (this is usually set to the user running Emacs). If `preserve-uid-gid` is non-`nil`, we attempt to copy the user and group ownership of the file. This works only on some operating systems, and only if you have the correct permissions to do so.

    If the optional argument `preserve-permissions` is non-`nil`, this function copies the file modes (or “permissions”) of `oldname` to `newname`, as well as the Access Control List and SELinux context (if any). See [Information about Files](Information-about-Files.html).

    Otherwise, the file modes of `newname` are left unchanged if it is an existing file, and set to those of `oldname`, masked by the default file permissions (see `set-default-file-modes` below), if `newname` is to be newly created. The Access Control List or SELinux context are not copied over in either case.

<!---->

*   Command: **make-symbolic-link** *target linkname \&optional ok-if-already-exists*

    This command makes a symbolic link to `target`, named `linkname`. This is like the shell command ‘`ln -s target linkname`’. The `target` argument is treated only as a string; it need not name an existing file. If `ok-if-already-exists` is an integer, indicating interactive use, then leading ‘`~`’ is expanded and leading ‘`/:`’ is stripped in the `target` string.

    If `target` is a relative file name, the resulting symbolic link is interpreted relative to the directory containing the symbolic link. See [Relative File Names](Relative-File-Names.html).

    If both `target` and `linkname` have remote file name syntax, and if both remote identifications are equal, the symbolic link points to the local file name part of `target`.

    This function is not available on systems that don’t support symbolic links.

<!---->

*   Command: **delete-file** *filename \&optional trash*

    This command deletes the file `filename`. If the file has multiple names, it continues to exist under the other names. If `filename` is a symbolic link, `delete-file` deletes only the symbolic link and not its target.

    A suitable kind of `file-error` error is signaled if the file does not exist, or is not deletable. (On GNU and other POSIX-like systems, a file is deletable if its directory is writable.)

    If the optional argument `trash` is non-`nil` and the variable `delete-by-moving-to-trash` is non-`nil`, this command moves the file into the system Trash instead of deleting it. See [Miscellaneous File Operations](https://www.gnu.org/software/emacs/manual/html_node/emacs/Misc-File-Ops.html#Misc-File-Ops) in The GNU Emacs Manual. When called interactively, `trash` is `t` if no prefix argument is given, and `nil` otherwise.

    See also `delete-directory` in [Create/Delete Dirs](Create_002fDelete-Dirs.html).

<!---->

*   Command: **set-file-modes** *filename mode*

    This function sets the *file mode* (or *permissions*) of `filename` to `mode`. This function follows symbolic links.

    If called non-interactively, `mode` must be an integer. Only the lowest 12 bits of the integer are used; on most systems, only the lowest 9 bits are meaningful. You can use the Lisp construct for octal numbers to enter `mode`. For example,

        (set-file-modes #o644)

    specifies that the file should be readable and writable for its owner, readable for group members, and readable for all other users. See [File permissions](https://www.gnu.org/software/coreutils/manual/html_node/File-permissions.html#File-permissions) in The GNU `Coreutils` Manual, for a description of mode bit specifications.

    Interactively, `mode` is read from the minibuffer using `read-file-modes` (see below), which lets the user type in either an integer or a string representing the permissions symbolically.

    See [File Attributes](File-Attributes.html), for the function `file-modes`, which returns the permissions of a file.

<!---->

*   Function: **set-default-file-modes** *mode*

    This function sets the default permissions for new files created by Emacs and its subprocesses. Every file created with Emacs initially has these permissions, or a subset of them (`write-region` will not grant execute permissions even if the default file permissions allow execution). On GNU and other POSIX-like systems, the default permissions are given by the bitwise complement of the ‘`umask`’ value, i.e. each bit that is set in the argument `mode` will be *reset* in the default permissions with which Emacs creates files.

    The argument `mode` should be an integer which specifies the permissions, similar to `set-file-modes` above. Only the lowest 9 bits are meaningful.

    The default file permissions have no effect when you save a modified version of an existing file; saving a file preserves its existing permissions.

<!---->

*   Macro: **with-file-modes** *mode body…*

    This macro evaluates the `body` forms with the default permissions for new files temporarily set to `modes` (whose value is as for `set-file-modes` above). When finished, it restores the original default file permissions, and returns the value of the last form in `body`.

    This is useful for creating private files, for example.

<!---->

*   Function: **default-file-modes**

    This function returns the default file permissions, as an integer.

<!---->

*   Function: **read-file-modes** *\&optional prompt base-file*

    This function reads a set of file mode bits from the minibuffer. The first optional argument `prompt` specifies a non-default prompt. Second second optional argument `base-file` is the name of a file on whose permissions to base the mode bits that this function returns, if what the user types specifies mode bits relative to permissions of an existing file.

    If user input represents an octal number, this function returns that number. If it is a complete symbolic specification of mode bits, as in `"u=rwx"`, the function converts it to the equivalent numeric value using `file-modes-symbolic-to-number` and returns the result. If the specification is relative, as in `"o+g"`, then the permissions on which the specification is based are taken from the mode bits of `base-file`. If `base-file` is omitted or `nil`, the function uses `0` as the base mode bits. The complete and relative specifications can be combined, as in `"u+r,g+rx,o+r,g-w"`. See [File permissions](https://www.gnu.org/software/coreutils/manual/html_node/File-permissions.html#File-permissions) in The GNU `Coreutils` Manual, for a description of file mode specifications.

<!---->

*   Function: **file-modes-symbolic-to-number** *modes \&optional base-modes*

    This function converts a symbolic file mode specification in `modes` into the equivalent integer. If the symbolic specification is based on an existing file, that file’s mode bits are taken from the optional argument `base-modes`; if that argument is omitted or `nil`, it defaults to 0, i.e., no access rights at all.

<!---->

*   Function: **set-file-times** *filename \&optional time*

    This function sets the access and modification times of `filename` to `time`. The return value is `t` if the times are successfully set, otherwise it is `nil`. `time` defaults to the current time and must be a time value (see [Time of Day](Time-of-Day.html)).

<!---->

*   Function: **set-file-extended-attributes** *filename attribute-alist*

    This function sets the Emacs-recognized extended file attributes for `filename`. The second argument `attribute-alist` should be an alist of the same form returned by `file-extended-attributes`. The return value is `t` if the attributes are successfully set, otherwise it is `nil`. See [Extended Attributes](Extended-Attributes.html).

<!---->

*   Function: **set-file-selinux-context** *filename context*

    This function sets the SELinux security context for `filename` to `context`. The `context` argument should be a list `(user role type range)`, where each element is a string. See [Extended Attributes](Extended-Attributes.html).

    The function returns `t` if it succeeds in setting the SELinux context of `filename`. It returns `nil` if the context was not set (e.g., if SELinux is disabled, or if Emacs was compiled without SELinux support).

<!---->

*   Function: **set-file-acl** *filename acl*

    This function sets the Access Control List for `filename` to `acl`. The `acl` argument should have the same form returned by the function `file-acl`. See [Extended Attributes](Extended-Attributes.html).

    The function returns `t` if it successfully sets the ACL of `filename`, `nil` otherwise.

Next: [Files and Storage](Files-and-Storage.html), Previous: [Information about Files](Information-about-Files.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
