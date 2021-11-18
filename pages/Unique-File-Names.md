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

Next: [File Name Completion](File-Name-Completion.html), Previous: [File Name Expansion](File-Name-Expansion.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.9.5 Generating Unique File Names

Some programs need to write temporary files. Here is the usual way to construct a name for such a file:

    (make-temp-file name-of-application)

The job of `make-temp-file` is to prevent two different users or two different jobs from trying to use the exact same file name.

*   Function: **make-temp-file** *prefix \&optional dir-flag suffix text*

    This function creates a temporary file and returns its name. Emacs creates the temporary file’s name by adding to `prefix` some random characters that are different in each Emacs job. The result is guaranteed to be a newly created file, containing `text` if that’s given as a string and empty otherwise. On MS-DOS, this function can truncate `prefix` to fit into the 8+3 file-name limits. If `prefix` is a relative file name, it is expanded against `temporary-file-directory`.

        (make-temp-file "foo")
             ⇒ "/tmp/foo232J6v"

    When `make-temp-file` returns, the file has been created and is empty. At that point, you should write the intended contents into the file.

    If `dir-flag` is non-`nil`, `make-temp-file` creates an empty directory instead of an empty file. It returns the file name, not the directory name, of that directory. See [Directory Names](Directory-Names.html).

    If `suffix` is non-`nil`, `make-temp-file` adds it at the end of the file name.

    If `text` is a string, `make-temp-file` inserts it in the file.

    To prevent conflicts among different libraries running in the same Emacs, each Lisp program that uses `make-temp-file` should have its own `prefix`. The number added to the end of `prefix` distinguishes between the same application running in different Emacs jobs. Additional added characters permit a large number of distinct names even in one Emacs job.

The default directory for temporary files is controlled by the variable `temporary-file-directory`. This variable gives the user a uniform way to specify the directory for all temporary files. Some programs use `small-temporary-file-directory` instead, if that is non-`nil`. To use it, you should expand the prefix against the proper directory before calling `make-temp-file`.

*   User Option: **temporary-file-directory**

    This variable specifies the directory name for creating temporary files. Its value should be a directory name (see [Directory Names](Directory-Names.html)), but it is good for Lisp programs to cope if the value is a directory’s file name instead. Using the value as the second argument to `expand-file-name` is a good way to achieve that.

    The default value is determined in a reasonable way for your operating system; it is based on the `TMPDIR`, `TMP` and `TEMP` environment variables, with a fall-back to a system-dependent name if none of these variables is defined.

    Even if you do not use `make-temp-file` to create the temporary file, you should still use this variable to decide which directory to put the file in. However, if you expect the file to be small, you should use `small-temporary-file-directory` first if that is non-`nil`.

<!---->

*   User Option: **small-temporary-file-directory**

    This variable specifies the directory name for creating certain temporary files, which are likely to be small.

    If you want to write a temporary file which is likely to be small, you should compute the directory like this:

        (make-temp-file
          (expand-file-name prefix
                            (or small-temporary-file-directory
                                temporary-file-directory)))

<!---->

*   Function: **make-temp-name** *base-name*

    This function generates a string that might be a unique file name. The name starts with `base-name`, and has several random characters appended to it, which are different in each Emacs job. It is like `make-temp-file` except that (i) it just constructs a name and does not create a file, (ii) `base-name` should be an absolute file name that is not magic, and (iii) if the returned file name is magic, it might name an existing file. See [Magic File Names](Magic-File-Names.html).

    **Warning:** In most cases, you should not use this function; use `make-temp-file` instead! This function is susceptible to a race condition, between the `make-temp-name` call and the creation of the file, which in some cases may cause a security hole.

Sometimes, it is necessary to create a temporary file on a remote host or a mounted directory. The following two functions support this.

*   Function: **make-nearby-temp-file** *prefix \&optional dir-flag suffix*

    This function is similar to `make-temp-file`, but it creates a temporary file as close as possible to `default-directory`. If `prefix` is a relative file name, and `default-directory` is a remote file name or located on a mounted file systems, the temporary file is created in the directory returned by the function `temporary-file-directory`. Otherwise, the function `make-temp-file` is used. `prefix`, `dir-flag` and `suffix` have the same meaning as in `make-temp-file`.

        (let ((default-directory "/ssh:remotehost:"))
          (make-nearby-temp-file "foo"))
             ⇒ "/ssh:remotehost:/tmp/foo232J6v"

<!---->

*   Function: **temporary-file-directory**

    The directory for writing temporary files via `make-nearby-temp-file`. In case of a remote `default-directory`, this is a directory for temporary files on that remote host. If such a directory does not exist, or `default-directory` ought to be located on a mounted file system (see `mounted-file-systems`), the function returns `default-directory`. For a non-remote and non-mounted `default-directory`, the value of the variable `temporary-file-directory` is returned.

In order to extract the local part of the file’s name of a temporary file, use `file-local-name` (see [Magic File Names](Magic-File-Names.html)).

Next: [File Name Completion](File-Name-Completion.html), Previous: [File Name Expansion](File-Name-Expansion.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
