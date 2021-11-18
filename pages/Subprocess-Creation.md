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

Next: [Shell Arguments](Shell-Arguments.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.1 Functions that Create Subprocesses

There are three primitives that create a new subprocess in which to run a program. One of them, `make-process`, creates an asynchronous process and returns a process object (see [Asynchronous Processes](Asynchronous-Processes.html)). The other two, `call-process` and `call-process-region`, create a synchronous process and do not return a process object (see [Synchronous Processes](Synchronous-Processes.html)). There are various higher-level functions that make use of these primitives to run particular types of process.

Synchronous and asynchronous processes are explained in the following sections. Since the three functions are all called in a similar fashion, their common arguments are described here.

In all cases, the functions specify the program to be run. An error is signaled if the file is not found or cannot be executed. If the file name is relative, the variable `exec-path` contains a list of directories to search. Emacs initializes `exec-path` when it starts up, based on the value of the environment variable `PATH`. The standard file name constructs, ‘`~`’, ‘`.`’, and ‘`..`’, are interpreted as usual in `exec-path`, but environment variable substitutions (‘`$HOME`’, etc.) are not recognized; use `substitute-in-file-name` to perform them (see [File Name Expansion](File-Name-Expansion.html)). `nil` in this list refers to `default-directory`.

Executing a program can also try adding suffixes to the specified name:

*   User Option: **exec-suffixes**

    This variable is a list of suffixes (strings) to try adding to the specified program file name. The list should include `""` if you want the name to be tried exactly as specified. The default value is system-dependent.

**Please note:** The argument `program` contains only the name of the program file; it may not contain any command-line arguments. You must use a separate argument, `args`, to provide those, as described below.

Each of the subprocess-creating functions has a `buffer-or-name` argument that specifies where the output from the program will go. It should be a buffer or a buffer name; if it is a buffer name, that will create the buffer if it does not already exist. It can also be `nil`, which says to discard the output, unless a custom filter function handles it. (See [Filter Functions](Filter-Functions.html), and [Read and Print](Read-and-Print.html).) Normally, you should avoid having multiple processes send output to the same buffer because their output would be intermixed randomly. For synchronous processes, you can send the output to a file instead of a buffer (and the corresponding argument is therefore more appropriately called `destination`). By default, both standard output and standard error streams go to the same destination, but all the 3 primitives allow optionally to direct the standard error stream to a different destination.

All three of the subprocess-creating functions allow to specify command-line arguments for the process to run. For `call-process` and `call-process-region`, these come in the form of a `&rest` argument, `args`. For `make-process`, both the program to run and its command-line arguments are specified as a list of strings. The command-line arguments must all be strings, and they are supplied to the program as separate argument strings. Wildcard characters and other shell constructs have no special meanings in these strings, since the strings are passed directly to the specified program.

The subprocess inherits its environment from Emacs, but you can specify overrides for it with `process-environment`. See [System Environment](System-Environment.html). The subprocess gets its current directory from the value of `default-directory`.

*   Variable: **exec-directory**

    The value of this variable is a string, the name of a directory that contains programs that come with GNU Emacs and are intended for Emacs to invoke. The program `movemail` is an example of such a program; Rmail uses it to fetch new mail from an inbox.

<!---->

*   User Option: **exec-path**

    The value of this variable is a list of directories to search for programs to run in subprocesses. Each element is either the name of a directory (i.e., a string), or `nil`, which stands for the default directory (which is the value of `default-directory`). See [executable-find](Locating-Files.html), for the details of this search.

    The value of `exec-path` is used by `call-process` and `start-process` when the `program` argument is not an absolute file name.

    Generally, you should not modify `exec-path` directly. Instead, ensure that your `PATH` environment variable is set appropriately before starting Emacs. Trying to modify `exec-path` independently of `PATH` can lead to confusing results.

<!---->

*   Function: **exec-path**

    This function is an extension of the variable `exec-path`. If `default-directory` indicates a remote directory, this function returns a list of directories used for searching programs on the respective remote host. In case of a local `default-directory`, the function returns just the value of the variable `exec-path`.

Next: [Shell Arguments](Shell-Arguments.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
