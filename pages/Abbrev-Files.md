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

Next: [Abbrev Expansion](Abbrev-Expansion.html), Previous: [Defining Abbrevs](Defining-Abbrevs.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 36.3 Saving Abbrevs in Files

A file of saved abbrev definitions is actually a file of Lisp code. The abbrevs are saved in the form of a Lisp program to define the same abbrev tables with the same contents. Therefore, you can load the file with `load` (see [How Programs Do Loading](How-Programs-Do-Loading.html)). However, the function `quietly-read-abbrev-file` is provided as a more convenient interface. Emacs automatically calls this function at startup.

User-level facilities such as `save-some-buffers` can save abbrevs in a file automatically, under the control of variables described here.

*   User Option: **abbrev-file-name**

    This is the default file name for reading and saving abbrevs. By default, Emacs will look for `~/.emacs.d/abbrev_defs`, and, if not found, for `~/.abbrev_defs`; if neither file exists, Emacs will create `~/.emacs.d/abbrev_defs`.

<!---->

*   Function: **quietly-read-abbrev-file** *\&optional filename*

    This function reads abbrev definitions from a file named `filename`, previously written with `write-abbrev-file`. If `filename` is omitted or `nil`, the file specified in `abbrev-file-name` is used.

    As the name implies, this function does not display any messages.

<!---->

*   User Option: **save-abbrevs**

    A non-`nil` value for `save-abbrevs` means that Emacs should offer to save abbrevs (if any have changed) when files are saved. If the value is `silently`, Emacs saves the abbrevs without asking the user. `abbrev-file-name` specifies the file to save the abbrevs in. The default value is `t`.

<!---->

*   Variable: **abbrevs-changed**

    This variable is set non-`nil` by defining or altering any abbrevs (except system abbrevs). This serves as a flag for various Emacs commands to offer to save your abbrevs.

<!---->

*   Command: **write-abbrev-file** *\&optional filename*

    Save all abbrev definitions (except system abbrevs), for all abbrev tables listed in `abbrev-table-name-list`, in the file `filename`, in the form of a Lisp program that when loaded will define the same abbrevs. Tables that do not have any abbrevs to save are omitted. If `filename` is `nil` or omitted, `abbrev-file-name` is used. This function returns `nil`.

Next: [Abbrev Expansion](Abbrev-Expansion.html), Previous: [Defining Abbrevs](Defining-Abbrevs.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
