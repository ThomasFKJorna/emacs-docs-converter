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

Next: [Output Streams](Output-Streams.html), Previous: [Input Streams](Input-Streams.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 19.3 Input Functions

This section describes the Lisp functions and variables that pertain to reading.

In the functions below, `stream` stands for an input stream (see the previous section). If `stream` is `nil` or omitted, it defaults to the value of `standard-input`.

An `end-of-file` error is signaled if reading encounters an unterminated list, vector, or string.

*   Function: **read** *\&optional stream*

    This function reads one textual Lisp expression from `stream`, returning it as a Lisp object. This is the basic Lisp input function.

<!---->

*   Function: **read-from-string** *string \&optional start end*

    This function reads the first textual Lisp expression from the text in `string`. It returns a cons cell whose CAR is that expression, and whose CDR is an integer giving the position of the next remaining character in the string (i.e., the first one not read).

    If `start` is supplied, then reading begins at index `start` in the string (where the first character is at index 0). If you specify `end`, then reading is forced to stop just before that index, as if the rest of the string were not there.

    For example:

        (read-from-string "(setq x 55) (setq y 5)")
             ⇒ ((setq x 55) . 11)

    <!---->

        (read-from-string "\"A short string\"")
             ⇒ ("A short string" . 16)

    ```
    ```

        ;; Read starting at the first character.
        (read-from-string "(list 112)" 0)
             ⇒ ((list 112) . 10)

    <!---->

        ;; Read starting at the second character.
        (read-from-string "(list 112)" 1)
             ⇒ (list . 5)

    <!---->

        ;; Read starting at the seventh character,
        ;;   and stopping at the ninth.
        (read-from-string "(list 112)" 6 8)
             ⇒ (11 . 8)

<!---->

*   Variable: **standard-input**

    This variable holds the default input stream—the stream that `read` uses when the `stream` argument is `nil`. The default is `t`, meaning use the minibuffer.

<!---->

*   Variable: **read-circle**

    If non-`nil`, this variable enables the reading of circular and shared structures. See [Circular Objects](Circular-Objects.html). Its default value is `t`.

When reading or writing from the standard input/output streams of the Emacs process in batch mode, it is sometimes required to make sure any arbitrary binary data will be read/written verbatim, and/or that no translation of newlines to or from CR-LF pairs is performed. This issue does not exist on POSIX hosts, only on MS-Windows and MS-DOS. The following function allows you to control the I/O mode of any standard stream of the Emacs process.

*   Function: **set-binary-mode** *stream mode*

    Switch `stream` into binary or text I/O mode. If `mode` is non-`nil`, switch to binary mode, otherwise switch to text mode. The value of `stream` can be one of `stdin`, `stdout`, or `stderr`. This function flushes any pending output data of `stream` as a side effect, and returns the previous value of I/O mode for `stream`. On POSIX hosts, it always returns a non-`nil` value and does nothing except flushing pending output.

Next: [Output Streams](Output-Streams.html), Previous: [Input Streams](Input-Streams.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
