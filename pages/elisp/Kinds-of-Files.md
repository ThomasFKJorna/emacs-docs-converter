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

Next: [Truenames](Truenames.html), Previous: [Testing Accessibility](Testing-Accessibility.html), Up: [Information about Files](Information-about-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.6.2 Distinguishing Kinds of Files

This section describes how to distinguish various kinds of files, such as directories, symbolic links, and ordinary files.

Symbolic links are ordinarily followed wherever they appear. For example, to interpret the file name `a/b/c`, any of `a`, `a/b`, and `a/b/c` can be symbolic links that are followed, possibly recursively if the link targets are themselves symbolic links. However, a few functions do not follow symbolic links at the end of a file name (`a/b/c` in this example). Such a function is said to *not follow symbolic links*.

*   Function: **file-symlink-p** *filename*

    If the file `filename` is a symbolic link, this function does not follow it and instead returns its link target as a string. (The link target string is not necessarily the full absolute file name of the target; determining the full file name that the link points to is nontrivial, see below.)

    If the file `filename` is not a symbolic link, or does not exist, or if there is trouble determining whether it is a symbolic link, `file-symlink-p` returns `nil`.

    Here are a few examples of using this function:

        (file-symlink-p "not-a-symlink")
             ⇒ nil

    <!---->

        (file-symlink-p "sym-link")
             ⇒ "not-a-symlink"

    <!---->

        (file-symlink-p "sym-link2")
             ⇒ "sym-link"

    <!---->

        (file-symlink-p "/bin")
             ⇒ "/pub/bin"

    Note that in the third example, the function returned `sym-link`, but did not proceed to resolve it, although that file is itself a symbolic link. That is because this function does not follow symbolic links—the process of following the symbolic links does not apply to the last component of the file name.

    The string that this function returns is what is recorded in the symbolic link; it may or may not include any leading directories. This function does *not* expand the link target to produce a fully-qualified file name, and in particular does not use the leading directories, if any, of the `filename` argument if the link target is not an absolute file name. Here’s an example:

        (file-symlink-p "/foo/bar/baz")
             ⇒ "some-file"

    Here, although `/foo/bar/baz` was given as a fully-qualified file name, the result is not, and in fact does not have any leading directories at all. And since `some-file` might itself be a symbolic link, you cannot simply prepend leading directories to it, nor even naively use `expand-file-name` (see [File Name Expansion](File-Name-Expansion.html)) to produce its absolute file name.

    For this reason, this function is seldom useful if you need to determine more than just the fact that a file is or isn’t a symbolic link. If you actually need the file name of the link target, use `file-chase-links` or `file-truename`, described in [Truenames](Truenames.html).

<!---->

*   Function: **file-directory-p** *filename*

    This function returns `t` if `filename` is the name of an existing directory. It returns `nil` if `filename` does not name a directory, or if there is trouble determining whether it is a directory. This function follows symbolic links.

        (file-directory-p "~rms")
             ⇒ t

    <!---->

        (file-directory-p "~rms/lewis/files.texi")
             ⇒ nil

    <!---->

        (file-directory-p "~rms/lewis/no-such-file")
             ⇒ nil

    <!---->

        (file-directory-p "$HOME")
             ⇒ nil

    <!---->

        (file-directory-p
         (substitute-in-file-name "$HOME"))
             ⇒ t

<!---->

*   Function: **file-regular-p** *filename*

    This function returns `t` if the file `filename` exists and is a regular file (not a directory, named pipe, terminal, or other I/O device). It returns `nil` if `filename` does not exist or is not a regular file, or if there is trouble determining whether it is a regular file. This function follows symbolic links.

Next: [Truenames](Truenames.html), Previous: [Testing Accessibility](Testing-Accessibility.html), Up: [Information about Files](Information-about-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
