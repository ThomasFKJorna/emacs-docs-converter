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

Next: [Search-based Fontification](Search_002dbased-Fontification.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.6.1 Font Lock Basics

The Font Lock functionality is based on several basic functions. Each of these calls the function specified by the corresponding variable. This indirection allows major and minor modes to modify the way fontification works in the buffers of that mode, and even use the Font Lock mechanisms for features that have nothing to do with fontification. (This is why the description below says “should” when it describes what the functions do: the mode can customize the values of the corresponding variables to do something entirely different.) The variables mentioned below are described in [Other Font Lock Variables](Other-Font-Lock-Variables.html).

*   `font-lock-fontify-buffer`

    This function should fontify the current buffer’s accessible portion, by calling the function specified by `font-lock-fontify-buffer-function`.

*   `font-lock-unfontify-buffer`

    Used when turning Font Lock off to remove the fontification. Calls the function specified by `font-lock-unfontify-buffer-function`.

*   `font-lock-fontify-region beg end &optional loudly`

    Should fontify the region between `beg` and `end`. If `loudly` is non-`nil`, should display status messages while fontifying. Calls the function specified by `font-lock-fontify-region-function`.

*   `font-lock-unfontify-region beg end`

    Should remove fontification from the region between `beg` and `end`. Calls the function specified by `font-lock-unfontify-region-function`.

*   `font-lock-flush &optional beg end`

    This function should mark the fontification of the region between `beg` and `end` as outdated. If not specified or `nil`, `beg` and `end` default to the beginning and end of the buffer’s accessible portion. Calls the function specified by `font-lock-flush-function`.

*   `font-lock-ensure &optional beg end`

    This function should make sure the region between `beg` and `end` has been fontified. The optional arguments `beg` and `end` default to the beginning and the end of the buffer’s accessible portion. Calls the function specified by `font-lock-ensure-function`.

*   `font-lock-debug-fontify`

    This is a convenience command meant to be used when developing font locking for a mode, and should not be called from Lisp code. It recomputes all the relevant variables and then calls `font-lock-fontify-region` on the entire buffer.

There are several variables that control how Font Lock mode highlights text. But major modes should not set any of these variables directly. Instead, they should set `font-lock-defaults` as a buffer-local variable. The value assigned to this variable is used, if and when Font Lock mode is enabled, to set all the other variables.

*   Variable: **font-lock-defaults**

    This variable is set by modes to specify how to fontify text in that mode. It automatically becomes buffer-local when set. If its value is `nil`, Font Lock mode does no highlighting, and you can use the ‘`Faces`’ menu (under ‘`Edit`’ and then ‘`Text Properties`’ in the menu bar) to assign faces explicitly to text in the buffer.

    If non-`nil`, the value should look like this:

        (keywords [keywords-only [case-fold
         [syntax-alist other-vars…]]])

    The first element, `keywords`, indirectly specifies the value of `font-lock-keywords` which directs search-based fontification. It can be a symbol, a variable or a function whose value is the list to use for `font-lock-keywords`. It can also be a list of several such symbols, one for each possible level of fontification. The first symbol specifies the ‘`mode default`’ level of fontification, the next symbol level 1 fontification, the next level 2, and so on. The ‘`mode default`’ level is normally the same as level 1. It is used when `font-lock-maximum-decoration` has a `nil` value. See [Levels of Font Lock](Levels-of-Font-Lock.html).

    The second element, `keywords-only`, specifies the value of the variable `font-lock-keywords-only`. If this is omitted or `nil`, syntactic fontification (of strings and comments) is also performed. If this is non-`nil`, syntactic fontification is not performed. See [Syntactic Font Lock](Syntactic-Font-Lock.html).

    The third element, `case-fold`, specifies the value of `font-lock-keywords-case-fold-search`. If it is non-`nil`, Font Lock mode ignores case during search-based fontification.

    If the fourth element, `syntax-alist`, is non-`nil`, it should be a list of cons cells of the form `(char-or-string . string)`. These are used to set up a syntax table for syntactic fontification; the resulting syntax table is stored in `font-lock-syntax-table`. If `syntax-alist` is omitted or `nil`, syntactic fontification uses the syntax table returned by the `syntax-table` function. See [Syntax Table Functions](Syntax-Table-Functions.html).

    All the remaining elements (if any) are collectively called `other-vars`. Each of these elements should have the form `(variable . value)`—which means, make `variable` buffer-local and then set it to `value`. You can use these `other-vars` to set other variables that affect fontification, aside from those you can control with the first five elements. See [Other Font Lock Variables](Other-Font-Lock-Variables.html).

If your mode fontifies text explicitly by adding `font-lock-face` properties, it can specify `(nil t)` for `font-lock-defaults` to turn off all automatic fontification. However, this is not required; it is possible to fontify some things using `font-lock-face` properties and set up automatic fontification for other parts of the text.

Next: [Search-based Fontification](Search_002dbased-Fontification.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
