

Previous: [Terminal-Specific](Terminal_002dSpecific.html), Up: [Starting Up](Starting-Up.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 40.1.4 Command-Line Arguments

You can use command-line arguments to request various actions when you start Emacs. Note that the recommended way of using Emacs is to start it just once, after logging in, and then do all editing in the same Emacs session (see [Entering Emacs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Entering-Emacs.html#Entering-Emacs) in The GNU Emacs Manual). For this reason, you might not use command-line arguments very often; nonetheless, they can be useful when invoking Emacs from session scripts or debugging Emacs. This section describes how Emacs processes command-line arguments.

*   Function: **command-line**

    This function parses the command line that Emacs was called with, processes it, and (amongst other things) loads the user’s init file and displays the startup messages.

<!---->

*   Variable: **command-line-processed**

    The value of this variable is `t` once the command line has been processed.

    If you redump Emacs by calling `dump-emacs` (see [Building Emacs](Building-Emacs.html)), you may wish to set this variable to `nil` first in order to cause the new dumped Emacs to process its new command-line arguments.

<!---->

*   Variable: **command-switch-alist**

    This variable is an alist of user-defined command-line options and associated handler functions. By default it is empty, but you can add elements if you wish.

    A *command-line option* is an argument on the command line, which has the form:

    ```lisp
    -option
    ```

    The elements of the `command-switch-alist` look like this:

    ```lisp
    (option . handler-function)
    ```

    The CAR, `option`, is a string, the name of a command-line option (including the initial hyphen). The `handler-function` is called to handle `option`, and receives the option name as its sole argument.

    In some cases, the option is followed in the command line by an argument. In these cases, the `handler-function` can find all the remaining command-line arguments in the variable `command-line-args-left` (see below). (The entire list of command-line arguments is in `command-line-args`.)

    Note that the handling of `command-switch-alist` doesn’t treat equals signs in `option` specially. That is, if there’s an option like `--name=value` on the command line, then only a `command-switch-alist` member whose `car` is literally `--name=value` will match this option. If you want to parse such options, you need to use `command-line-functions` instead (see below).

    The command-line arguments are parsed by the `command-line-1` function in the `startup.el` file. See also [Command Line Arguments for Emacs Invocation](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Invocation.html#Emacs-Invocation) in The GNU Emacs Manual.

<!---->

*   Variable: **command-line-args**

    The value of this variable is the list of command-line arguments passed to Emacs.

<!---->

*   Variable: **command-line-args-left**

    The value of this variable is the list of command-line arguments that have not yet been processed.

<!---->

*   Variable: **command-line-functions**

    This variable’s value is a list of functions for handling an unrecognized command-line argument. Each time the next argument to be processed has no special meaning, the functions in this list are called, in order of appearance, until one of them returns a non-`nil` value.

    These functions are called with no arguments. They can access the command-line argument under consideration through the variable `argi`, which is bound temporarily at this point. The remaining arguments (not including the current one) are in the variable `command-line-args-left`.

    When a function recognizes and processes the argument in `argi`, it should return a non-`nil` value to say it has dealt with that argument. If it has also dealt with some of the following arguments, it can indicate that by deleting them from `command-line-args-left`.

    If all of these functions return `nil`, then the argument is treated as a file name to visit.

Previous: [Terminal-Specific](Terminal_002dSpecific.html), Up: [Starting Up](Starting-Up.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
