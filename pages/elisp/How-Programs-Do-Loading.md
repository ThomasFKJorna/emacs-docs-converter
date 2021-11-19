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

Next: [Load Suffixes](Load-Suffixes.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 16.1 How Programs Do Loading

Emacs Lisp has several interfaces for loading. For example, `autoload` creates a placeholder object for a function defined in a file; trying to call the autoloading function loads the file to get the function’s real definition (see [Autoload](Autoload.html)). `require` loads a file if it isn’t already loaded (see [Named Features](Named-Features.html)). Ultimately, all these facilities call the `load` function to do the work.

*   Function: **load** *filename \&optional missing-ok nomessage nosuffix must-suffix*

    This function finds and opens a file of Lisp code, evaluates all the forms in it, and closes the file.

    To find the file, `load` first looks for a file named `filename.elc`, that is, for a file whose name is `filename` with the extension ‘`.elc`’ appended. If such a file exists, it is loaded. If there is no file by that name, then `load` looks for a file named `filename.el`. If that file exists, it is loaded. If Emacs was compiled with support for dynamic modules (see [Dynamic Modules](Dynamic-Modules.html)), `load` next looks for a file named `filename.ext`, where `ext` is a system-dependent file-name extension of shared libraries. Finally, if neither of those names is found, `load` looks for a file named `filename` with nothing appended, and loads it if it exists. (The `load` function is not clever about looking at `filename`. In the perverse case of a file named `foo.el.el`, evaluation of `(load "foo.el")` will indeed find it.)

    If Auto Compression mode is enabled, as it is by default, then if `load` can not find a file, it searches for a compressed version of the file before trying other file names. It decompresses and loads it if it exists. It looks for compressed versions by appending each of the suffixes in `jka-compr-load-suffixes` to the file name. The value of this variable must be a list of strings. Its standard value is `(".gz")`.

    If the optional argument `nosuffix` is non-`nil`, then `load` does not try the suffixes ‘`.elc`’ and ‘`.el`’. In this case, you must specify the precise file name you want, except that, if Auto Compression mode is enabled, `load` will still use `jka-compr-load-suffixes` to find compressed versions. By specifying the precise file name and using `t` for `nosuffix`, you can prevent file names like `foo.el.el` from being tried.

    If the optional argument `must-suffix` is non-`nil`, then `load` insists that the file name used must end in either ‘`.el`’ or ‘`.elc`’ (possibly extended with a compression suffix) or the shared-library extension, unless it contains an explicit directory name.

    If the option `load-prefer-newer` is non-`nil`, then when searching suffixes, `load` selects whichever version of a file (‘`.elc`’, ‘`.el`’, etc.) has been modified most recently.

    If `filename` is a relative file name, such as `foo` or `baz/foo.bar`, `load` searches for the file using the variable `load-path`. It appends `filename` to each of the directories listed in `load-path`, and loads the first file it finds whose name matches. The current default directory is tried only if it is specified in `load-path`, where `nil` stands for the default directory. `load` tries all three possible suffixes in the first directory in `load-path`, then all three suffixes in the second directory, and so on. See [Library Search](Library-Search.html).

    Whatever the name under which the file is eventually found, and the directory where Emacs found it, Emacs sets the value of the variable `load-file-name` to that file’s name.

    If you get a warning that `foo.elc` is older than `foo.el`, it means you should consider recompiling `foo.el`. See [Byte Compilation](Byte-Compilation.html).

    When loading a source file (not compiled), `load` performs character set translation just as Emacs would do when visiting the file. See [Coding Systems](Coding-Systems.html).

    When loading an uncompiled file, Emacs tries to expand any macros that the file contains (see [Macros](Macros.html)). We refer to this as *eager macro expansion*. Doing this (rather than deferring the expansion until the relevant code runs) can significantly speed up the execution of uncompiled code. Sometimes, this macro expansion cannot be done, owing to a cyclic dependency. In the simplest example of this, the file you are loading refers to a macro defined in another file, and that file in turn requires the file you are loading. This is generally harmless. Emacs prints a warning (‘`Eager macro-expansion skipped due to cycle…`’) giving details of the problem, but it still loads the file, just leaving the macro unexpanded for now. You may wish to restructure your code so that this does not happen. Loading a compiled file does not cause macroexpansion, because this should already have happened during compilation. See [Compiling Macros](Compiling-Macros.html).

    Messages like ‘`Loading foo...`’ and ‘`Loading foo...done`’ appear in the echo area during loading unless `nomessage` is non-`nil`.

    Any unhandled errors while loading a file terminate loading. If the load was done for the sake of `autoload`, any function definitions made during the loading are undone.

    If `load` can’t find the file to load, then normally it signals a `file-error` (with ‘`Cannot open load file filename`’). But if `missing-ok` is non-`nil`, then `load` just returns `nil`.

    You can use the variable `load-read-function` to specify a function for `load` to use instead of `read` for reading expressions. See below.

    `load` returns `t` if the file loads successfully.

<!---->

*   Command: **load-file** *filename*

    This command loads the file `filename`. If `filename` is a relative file name, then the current default directory is assumed. This command does not use `load-path`, and does not append suffixes. However, it does look for compressed versions (if Auto Compression Mode is enabled). Use this command if you wish to specify precisely the file name to load.

<!---->

*   Command: **load-library** *library*

    This command loads the library named `library`. It is equivalent to `load`, except for the way it reads its argument interactively. See [Lisp Libraries](https://www.gnu.org/software/emacs/manual/html_node/emacs/Lisp-Libraries.html#Lisp-Libraries) in The GNU Emacs Manual.

<!---->

*   Variable: **load-in-progress**

    This variable is non-`nil` if Emacs is in the process of loading a file, and it is `nil` otherwise.

<!---->

*   Variable: **load-file-name**

    When Emacs is in the process of loading a file, this variable’s value is the name of that file, as Emacs found it during the search described earlier in this section.

<!---->

*   Variable: **load-read-function**

    This variable specifies an alternate expression-reading function for `load` and `eval-region` to use instead of `read`. The function should accept one argument, just as `read` does.

    By default, this variable’s value is `read`. See [Input Functions](Input-Functions.html).

    Instead of using this variable, it is cleaner to use another, newer feature: to pass the function as the `read-function` argument to `eval-region`. See [Eval](Eval.html#Definition-of-eval_002dregion).

For information about how `load` is used in building Emacs, see [Building Emacs](Building-Emacs.html).

Next: [Load Suffixes](Load-Suffixes.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
