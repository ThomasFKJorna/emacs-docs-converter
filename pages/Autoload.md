

Next: [Repeated Loading](Repeated-Loading.html), Previous: [Loading Non-ASCII](Loading-Non_002dASCII.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 16.5 Autoload

The *autoload* facility lets you register the existence of a function or macro, but put off loading the file that defines it. The first call to the function automatically loads the proper library, in order to install the real definition and other associated code, then runs the real definition as if it had been loaded all along. Autoloading can also be triggered by looking up the documentation of the function or macro (see [Documentation Basics](Documentation-Basics.html)), and completion of variable and function names (see [Autoload by Prefix](Autoload-by-Prefix.html) below).

|                                                 |    |                       |
| :---------------------------------------------- | -- | :-------------------- |
| • [Autoload by Prefix](Autoload-by-Prefix.html) |    | Autoload by Prefix.   |
| • [When to Autoload](When-to-Autoload.html)     |    | When to Use Autoload. |

There are two ways to set up an autoloaded function: by calling `autoload`, and by writing a “magic” comment in the source before the real definition. `autoload` is the low-level primitive for autoloading; any Lisp program can call `autoload` at any time. Magic comments are the most convenient way to make a function autoload, for packages installed along with Emacs. These comments do nothing on their own, but they serve as a guide for the command `update-file-autoloads`, which constructs calls to `autoload` and arranges to execute them when Emacs is built.

*   Function: **autoload** *function filename \&optional docstring interactive type*

    This function defines the function (or macro) named `function` so as to load automatically from `filename`. The string `filename` specifies the file to load to get the real definition of `function`.

    If `filename` does not contain either a directory name, or the suffix `.el` or `.elc`, this function insists on adding one of these suffixes, and it will not load from a file whose name is just `filename` with no added suffix. (The variable `load-suffixes` specifies the exact required suffixes.)

    The argument `docstring` is the documentation string for the function. Specifying the documentation string in the call to `autoload` makes it possible to look at the documentation without loading the function’s real definition. Normally, this should be identical to the documentation string in the function definition itself. If it isn’t, the function definition’s documentation string takes effect when it is loaded.

    If `interactive` is non-`nil`, that says `function` can be called interactively. This lets completion in `M-x` work without loading `function`’s real definition. The complete interactive specification is not given here; it’s not needed unless the user actually calls `function`, and when that happens, it’s time to load the real definition.

    You can autoload macros and keymaps as well as ordinary functions. Specify `type` as `macro` if `function` is really a macro. Specify `type` as `keymap` if `function` is really a keymap. Various parts of Emacs need to know this information without loading the real definition.

    An autoloaded keymap loads automatically during key lookup when a prefix key’s binding is the symbol `function`. Autoloading does not occur for other kinds of access to the keymap. In particular, it does not happen when a Lisp program gets the keymap from the value of a variable and calls `define-key`; not even if the variable name is the same symbol `function`.

    If `function` already has a non-void function definition that is not an autoload object, this function does nothing and returns `nil`. Otherwise, it constructs an autoload object (see [Autoload Type](Autoload-Type.html)), and stores it as the function definition for `function`. The autoload object has this form:

    ```lisp
    (autoload filename docstring interactive type)
    ```

    For example,

    ```lisp
    (symbol-function 'run-prolog)
         ⇒ (autoload "prolog" 169681 t nil)
    ```

    In this case, `"prolog"` is the name of the file to load, 169681 refers to the documentation string in the `emacs/etc/DOC` file (see [Documentation Basics](Documentation-Basics.html)), `t` means the function is interactive, and `nil` that it is not a macro or a keymap.

<!---->

*   Function: **autoloadp** *object*

    This function returns non-`nil` if `object` is an autoload object. For example, to check if `run-prolog` is defined as an autoloaded function, evaluate

    ```lisp
    (autoloadp (symbol-function 'run-prolog))
    ```

The autoloaded file usually contains other definitions and may require or provide one or more features. If the file is not completely loaded (due to an error in the evaluation of its contents), any function definitions or `provide` calls that occurred during the load are undone. This is to ensure that the next attempt to call any function autoloading from this file will try again to load the file. If not for this, then some of the functions in the file might be defined by the aborted load, but fail to work properly for the lack of certain subroutines not loaded successfully because they come later in the file.

If the autoloaded file fails to define the desired Lisp function or macro, then an error is signaled with data `"Autoloading failed to define function function-name"`.

A magic autoload comment (often called an *autoload cookie*) consists of ‘`;;;###autoload`’, on a line by itself, just before the real definition of the function in its autoloadable source file. The command `M-x update-file-autoloads` writes a corresponding `autoload` call into `loaddefs.el`. (The string that serves as the autoload cookie and the name of the file generated by `update-file-autoloads` can be changed from the above defaults, see below.) Building Emacs loads `loaddefs.el` and thus calls `autoload`. `M-x update-directory-autoloads` is even more powerful; it updates autoloads for all files in the current directory.

The same magic comment can copy any kind of form into `loaddefs.el`. The form following the magic comment is copied verbatim, *except* if it is one of the forms which the autoload facility handles specially (e.g., by conversion into an `autoload` call). The forms which are not copied verbatim are the following:

*   Definitions for function or function-like objects:

    `defun` and `defmacro`; also `cl-defun` and `cl-defmacro` (see [Argument Lists](https://www.gnu.org/software/emacs/manual/html_node/cl/Argument-Lists.html#Argument-Lists) in Common Lisp Extensions), and `define-overloadable-function` (see the commentary in `mode-local.el`).

*   Definitions for major or minor modes:

    `define-minor-mode`, `define-globalized-minor-mode`, `define-generic-mode`, `define-derived-mode`, `easy-mmode-define-minor-mode`, `easy-mmode-define-global-mode`, `define-compilation-mode`, and `define-global-minor-mode`.

*   Other definition types:

    `defcustom`, `defgroup`, `defclass` (see [EIEIO](https://www.gnu.org/software/emacs/manual/html_node/eieio/index.html#Top) in EIEIO), and `define-skeleton` (see [Autotyping](https://www.gnu.org/software/emacs/manual/html_node/autotype/index.html#Top) in Autotyping).

You can also use a magic comment to execute a form at build time *without* executing it when the file itself is loaded. To do this, write the form *on the same line* as the magic comment. Since it is in a comment, it does nothing when you load the source file; but `M-x update-file-autoloads` copies it to `loaddefs.el`, where it is executed while building Emacs.

The following example shows how `doctor` is prepared for autoloading with a magic comment:

```lisp
;;;###autoload
(defun doctor ()
  "Switch to *doctor* buffer and start giving psychotherapy."
  (interactive)
  (switch-to-buffer "*doctor*")
  (doctor-mode))
```

Here’s what that produces in `loaddefs.el`:

```lisp
(autoload 'doctor "doctor" "\
Switch to *doctor* buffer and start giving psychotherapy.

\(fn)" t nil)
```

The backslash and newline immediately following the double-quote are a convention used only in the preloaded uncompiled Lisp files such as `loaddefs.el`; they tell `make-docfile` to put the documentation string in the `etc/DOC` file. See [Building Emacs](Building-Emacs.html). See also the commentary in `lib-src/make-docfile.c`. ‘`(fn)`’ in the usage part of the documentation string is replaced with the function’s name when the various help functions (see [Help Functions](Help-Functions.html)) display it.

If you write a function definition with an unusual macro that is not one of the known and recognized function definition methods, use of an ordinary magic autoload comment would copy the whole definition into `loaddefs.el`. That is not desirable. You can put the desired `autoload` call into `loaddefs.el` instead by writing this:

```lisp
;;;###autoload (autoload 'foo "myfile")
(mydefunmacro foo
  ...)
```

You can use a non-default string as the autoload cookie and have the corresponding autoload calls written into a file whose name is different from the default `loaddefs.el`. Emacs provides two variables to control this:

*   Variable: **generate-autoload-cookie**

    The value of this variable should be a string whose syntax is a Lisp comment. `M-x update-file-autoloads` copies the Lisp form that follows the cookie into the autoload file it generates. The default value of this variable is `";;;###autoload"`.

<!---->

*   Variable: **generated-autoload-file**

    The value of this variable names an Emacs Lisp file where the autoload calls should go. The default value is `loaddefs.el`, but you can override that, e.g., in the local variables section of a `.el` file (see [File Local Variables](File-Local-Variables.html)). The autoload file is assumed to contain a trailer starting with a formfeed character.

The following function may be used to explicitly load the library specified by an autoload object:

*   Function: **autoload-do-load** *autoload \&optional name macro-only*

    This function performs the loading specified by `autoload`, which should be an autoload object. The optional argument `name`, if non-`nil`, should be a symbol whose function value is `autoload`; in that case, the return value of this function is the symbol’s new function value. If the value of the optional argument `macro-only` is `macro`, this function avoids loading a function, only a macro.

Next: [Repeated Loading](Repeated-Loading.html), Previous: [Loading Non-ASCII](Loading-Non_002dASCII.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
