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

Next: [Where Defined](Where-Defined.html), Previous: [Repeated Loading](Repeated-Loading.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 16.7 Features

`provide` and `require` are an alternative to `autoload` for loading files automatically. They work in terms of named *features*. Autoloading is triggered by calling a specific function, but a feature is loaded the first time another program asks for it by name.

A feature name is a symbol that stands for a collection of functions, variables, etc. The file that defines them should *provide* the feature. Another program that uses them may ensure they are defined by *requiring* the feature. This loads the file of definitions if it hasn’t been loaded already.

To require the presence of a feature, call `require` with the feature name as argument. `require` looks in the global variable `features` to see whether the desired feature has been provided already. If not, it loads the feature from the appropriate file. This file should call `provide` at the top level to add the feature to `features`; if it fails to do so, `require` signals an error.

For example, in `idlwave.el`, the definition for `idlwave-complete-filename` includes the following code:

    (defun idlwave-complete-filename ()
      "Use the comint stuff to complete a file name."
       (require 'comint)
       (let* ((comint-file-name-chars "~/A-Za-z0-9+@:_.$#%={}\\-")
              (comint-completion-addsuffix nil)
              ...)
           (comint-dynamic-complete-filename)))

The expression `(require 'comint)` loads the file `comint.el` if it has not yet been loaded, ensuring that `comint-dynamic-complete-filename` is defined. Features are normally named after the files that provide them, so that `require` need not be given the file name. (Note that it is important that the `require` statement be outside the body of the `let`. Loading a library while its variables are let-bound can have unintended consequences, namely the variables becoming unbound after the let exits.)

The `comint.el` file contains the following top-level expression:

    (provide 'comint)

This adds `comint` to the global `features` list, so that `(require 'comint)` will henceforth know that nothing needs to be done.

When `require` is used at top level in a file, it takes effect when you byte-compile that file (see [Byte Compilation](Byte-Compilation.html)) as well as when you load it. This is in case the required package contains macros that the byte compiler must know about. It also avoids byte compiler warnings for functions and variables defined in the file loaded with `require`.

Although top-level calls to `require` are evaluated during byte compilation, `provide` calls are not. Therefore, you can ensure that a file of definitions is loaded before it is byte-compiled by including a `provide` followed by a `require` for the same feature, as in the following example.

    (provide 'my-feature)  ; Ignored by byte compiler,
                           ;   evaluated by load.
    (require 'my-feature)  ; Evaluated by byte compiler.

The compiler ignores the `provide`, then processes the `require` by loading the file in question. Loading the file does execute the `provide` call, so the subsequent `require` call does nothing when the file is loaded.

*   Function: **provide** *feature \&optional subfeatures*

    This function announces that `feature` is now loaded, or being loaded, into the current Emacs session. This means that the facilities associated with `feature` are or will be available for other Lisp programs.

    The direct effect of calling `provide` is to add `feature` to the front of `features` if it is not already in that list and call any `eval-after-load` code waiting for it (see [Hooks for Loading](Hooks-for-Loading.html)). The argument `feature` must be a symbol. `provide` returns `feature`.

    If provided, `subfeatures` should be a list of symbols indicating a set of specific subfeatures provided by this version of `feature`. You can test the presence of a subfeature using `featurep`. The idea of subfeatures is that you use them when a package (which is one `feature`) is complex enough to make it useful to give names to various parts or functionalities of the package, which might or might not be loaded, or might or might not be present in a given version. See [Network Feature Testing](Network-Feature-Testing.html), for an example.

        features
             ⇒ (bar bish)

        (provide 'foo)
             ⇒ foo
        features
             ⇒ (foo bar bish)

    When a file is loaded to satisfy an autoload, and it stops due to an error in the evaluation of its contents, any function definitions or `provide` calls that occurred during the load are undone. See [Autoload](Autoload.html).

<!---->

*   Function: **require** *feature \&optional filename noerror*

    This function checks whether `feature` is present in the current Emacs session (using `(featurep feature)`; see below). The argument `feature` must be a symbol.

    If the feature is not present, then `require` loads `filename` with `load`. If `filename` is not supplied, then the name of the symbol `feature` is used as the base file name to load. However, in this case, `require` insists on finding `feature` with an added ‘`.el`’ or ‘`.elc`’ suffix (possibly extended with a compression suffix); a file whose name is just `feature` won’t be used. (The variable `load-suffixes` specifies the exact required Lisp suffixes.)

    If `noerror` is non-`nil`, that suppresses errors from actual loading of the file. In that case, `require` returns `nil` if loading the file fails. Normally, `require` returns `feature`.

    If loading the file succeeds but does not provide `feature`, `require` signals an error about the missing feature.

<!---->

*   Function: **featurep** *feature \&optional subfeature*

    This function returns `t` if `feature` has been provided in the current Emacs session (i.e., if `feature` is a member of `features`.) If `subfeature` is non-`nil`, then the function returns `t` only if that subfeature is provided as well (i.e., if `subfeature` is a member of the `subfeature` property of the `feature` symbol.)

<!---->

*   Variable: **features**

    The value of this variable is a list of symbols that are the features loaded in the current Emacs session. Each symbol was put in this list with a call to `provide`. The order of the elements in the `features` list is not significant.

Next: [Where Defined](Where-Defined.html), Previous: [Repeated Loading](Repeated-Loading.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
