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

Next: [Format Conversion Piecemeal](Format-Conversion-Piecemeal.html), Previous: [Format Conversion Overview](Format-Conversion-Overview.html), Up: [Format Conversion](Format-Conversion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.13.2 Round-Trip Specification

The most general of the two facilities is controlled by the variable `format-alist`, a list of *file format* specifications, which describe textual representations used in files for the data in an Emacs buffer. The descriptions for reading and writing are paired, which is why we call this “round-trip” specification (see [Format Conversion Piecemeal](Format-Conversion-Piecemeal.html), for non-paired specification).

*   Variable: **format-alist**

    This list contains one format definition for each defined file format. Each format definition is a list of this form:

        (name doc-string regexp from-fn to-fn modify mode-fn preserve)

Here is what the elements in a format definition mean:

*   `name`

    The name of this format.

*   `doc-string`

    A documentation string for the format.

*   `regexp`

    A regular expression which is used to recognize files represented in this format. If `nil`, the format is never applied automatically.

*   `from-fn`

    A shell command or function to decode data in this format (to convert file data into the usual Emacs data representation).

    A shell command is represented as a string; Emacs runs the command as a filter to perform the conversion.

    If `from-fn` is a function, it is called with two arguments, `begin` and `end`, which specify the part of the buffer it should convert. It should convert the text by editing it in place. Since this can change the length of the text, `from-fn` should return the modified end position.

    One responsibility of `from-fn` is to make sure that the beginning of the file no longer matches `regexp`. Otherwise it is likely to get called again. Also, `from-fn` must not involve buffers or files other than the one being decoded, otherwise the internal buffer used for formatting might be overwritten.

*   `to-fn`

    A shell command or function to encode data in this format—that is, to convert the usual Emacs data representation into this format.

    If `to-fn` is a string, it is a shell command; Emacs runs the command as a filter to perform the conversion.

    If `to-fn` is a function, it is called with three arguments: `begin` and `end`, which specify the part of the buffer it should convert, and `buffer`, which specifies which buffer. There are two ways it can do the conversion:

    *   By editing the buffer in place. In this case, `to-fn` should return the end-position of the range of text, as modified.

    *   By returning a list of annotations. This is a list of elements of the form `(position . string)`, where `position` is an integer specifying the relative position in the text to be written, and `string` is the annotation to add there. The list must be sorted in order of position when `to-fn` returns it.

        When `write-region` actually writes the text from the buffer to the file, it intermixes the specified annotations at the corresponding positions. All this takes place without modifying the buffer.

    `to-fn` must not involve buffers or files other than the one being encoded, otherwise the internal buffer used for formatting might be overwritten.

*   `modify`

    A flag, `t` if the encoding function modifies the buffer, and `nil` if it works by returning a list of annotations.

*   `mode-fn`

    A minor-mode function to call after visiting a file converted from this format. The function is called with one argument, the integer 1; that tells a minor-mode function to enable the mode.

*   `preserve`

    A flag, `t` if `format-write-file` should not remove this format from `buffer-file-format`.

The function `insert-file-contents` automatically recognizes file formats when it reads the specified file. It checks the text of the beginning of the file against the regular expressions of the format definitions, and if it finds a match, it calls the decoding function for that format. Then it checks all the known formats over again. It keeps checking them until none of them is applicable.

Visiting a file, with `find-file-noselect` or the commands that use it, performs conversion likewise (because it calls `insert-file-contents`); it also calls the mode function for each format that it decodes. It stores a list of the format names in the buffer-local variable `buffer-file-format`.

*   Variable: **buffer-file-format**

    This variable states the format of the visited file. More precisely, this is a list of the file format names that were decoded in the course of visiting the current buffer’s file. It is always buffer-local in all buffers.

When `write-region` writes data into a file, it first calls the encoding functions for the formats listed in `buffer-file-format`, in the order of appearance in the list.

*   Command: **format-write-file** *file format \&optional confirm*

    This command writes the current buffer contents into the file `file` in a format based on `format`, which is a list of format names. It constructs the actual format starting from `format`, then appending any elements from the value of `buffer-file-format` with a non-`nil` `preserve` flag (see above), if they are not already present in `format`. It then updates `buffer-file-format` with this format, making it the default for future saves. Except for the `format` argument, this command is similar to `write-file`. In particular, `confirm` has the same meaning and interactive treatment as the corresponding argument to `write-file`. See [Definition of write-file](Saving-Buffers.html#Definition-of-write_002dfile).

<!---->

*   Command: **format-find-file** *file format*

    This command finds the file `file`, converting it according to format `format`. It also makes `format` the default if the buffer is saved later.

    The argument `format` is a list of format names. If `format` is `nil`, no conversion takes place. Interactively, typing just `RET` for `format` specifies `nil`.

<!---->

*   Command: **format-insert-file** *file format \&optional beg end*

    This command inserts the contents of file `file`, converting it according to format `format`. If `beg` and `end` are non-`nil`, they specify which part of the file to read, as in `insert-file-contents` (see [Reading from Files](Reading-from-Files.html)).

    The return value is like what `insert-file-contents` returns: a list of the absolute file name and the length of the data inserted (after conversion).

    The argument `format` is a list of format names. If `format` is `nil`, no conversion takes place. Interactively, typing just `RET` for `format` specifies `nil`.

<!---->

*   Variable: **buffer-auto-save-file-format**

    This variable specifies the format to use for auto-saving. Its value is a list of format names, just like the value of `buffer-file-format`; however, it is used instead of `buffer-file-format` for writing auto-save files. If the value is `t`, the default, auto-saving uses the same format as a regular save in the same buffer. This variable is always buffer-local in all buffers.

Next: [Format Conversion Piecemeal](Format-Conversion-Piecemeal.html), Previous: [Format Conversion Overview](Format-Conversion-Overview.html), Up: [Format Conversion](Format-Conversion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
