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

Next: [Command-Line Arguments](Command_002dLine-Arguments.html), Previous: [Init File](Init-File.html), Up: [Starting Up](Starting-Up.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 40.1.3 Terminal-Specific Initialization

Each terminal type can have its own Lisp library that Emacs loads when run on that type of terminal. The library’s name is constructed by concatenating the value of the variable `term-file-prefix` and the terminal type (specified by the environment variable `TERM`). Normally, `term-file-prefix` has the value `"term/"`; changing this is not recommended. If there is an entry matching `TERM` in the `term-file-aliases` association list, Emacs uses the associated value in place of `TERM`. Emacs finds the file in the normal manner, by searching the `load-path` directories, and trying the ‘`.elc`’ and ‘`.el`’ suffixes.

The usual role of a terminal-specific library is to enable special keys to send sequences that Emacs can recognize. It may also need to set or add to `input-decode-map` if the Termcap or Terminfo entry does not specify all the terminal’s function keys. See [Terminal Input](Terminal-Input.html).

When the name of the terminal type contains a hyphen or underscore, and no library is found whose name is identical to the terminal’s name, Emacs strips from the terminal’s name the last hyphen or underscore and everything that follows it, and tries again. This process is repeated until Emacs finds a matching library, or until there are no more hyphens or underscores in the name (i.e., there is no terminal-specific library). For example, if the terminal name is ‘`xterm-256color`’ and there is no `term/xterm-256color.el` library, Emacs tries to load `term/xterm.el`. If necessary, the terminal library can evaluate `(getenv "TERM")` to find the full name of the terminal type.

Your init file can prevent the loading of the terminal-specific library by setting the variable `term-file-prefix` to `nil`.

You can also arrange to override some of the actions of the terminal-specific library by using `tty-setup-hook`. This is a normal hook that Emacs runs after initializing a new text terminal. You could use this hook to define initializations for terminals that do not have their own libraries. See [Hooks](Hooks.html).

*   User Option: **term-file-prefix**

    If the value of this variable is non-`nil`, Emacs loads a terminal-specific initialization file as follows:

        (load (concat term-file-prefix (getenv "TERM")))

    You may set the `term-file-prefix` variable to `nil` in your init file if you do not wish to load the terminal-initialization file.

    On MS-DOS, Emacs sets the `TERM` environment variable to ‘`internal`’.

<!---->

*   User Option: **term-file-aliases**

    This variable is an association list mapping terminal types to their aliases. For example, an element of the form `("vt102" . "vt100")` means to treat a terminal of type ‘`vt102`’ like one of type ‘`vt100`’.

<!---->

*   Variable: **tty-setup-hook**

    This variable is a normal hook that Emacs runs after initializing a new text terminal. (This applies when Emacs starts up in non-windowed mode, and when making a tty `emacsclient` connection.) The hook runs after loading your init file (if applicable) and the terminal-specific Lisp file, so you can use it to adjust the definitions made by that file.

    For a related feature, see [window-setup-hook](Init-File.html).

Next: [Command-Line Arguments](Command_002dLine-Arguments.html), Previous: [Init File](Init-File.html), Up: [Starting Up](Starting-Up.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
