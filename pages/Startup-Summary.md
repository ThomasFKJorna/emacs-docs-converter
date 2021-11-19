

Next: [Init File](Init-File.html), Up: [Starting Up](Starting-Up.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 40.1.1 Summary: Sequence of Actions at Startup

When Emacs is started up, it performs the following operations (see `normal-top-level` in `startup.el`):

1.  It adds subdirectories to `load-path`, by running the file named `subdirs.el` in each directory in the list. Normally, this file adds the directory’s subdirectories to the list, and those are scanned in their turn. The files `subdirs.el` are normally generated automatically when Emacs is installed.
2.  It loads any `leim-list.el` that it finds in the `load-path` directories. This file is intended for registering input methods. The search is only for any personal `leim-list.el` files that you may have created; it skips the directories containing the standard Emacs libraries (these should contain only a single `leim-list.el` file, which is compiled into the Emacs executable).
3.  It sets the variable `before-init-time` to the value of `current-time` (see [Time of Day](Time-of-Day.html)). It also sets `after-init-time` to `nil`, which signals to Lisp programs that Emacs is being initialized.
4.  It sets the language environment and the terminal coding system, if requested by environment variables such as `LANG`.
5.  It does some basic parsing of the command-line arguments.
6.  It loads your early init file (see [Early Init File](https://www.gnu.org/software/emacs/manual/html_node/emacs/Early-Init-File.html#Early-Init-File) in The GNU Emacs Manual). This is not done if the options ‘`-q`’, ‘`-Q`’, or ‘`--batch`’ were specified. If the ‘`-u`’ option was specified, Emacs looks for the init file in that user’s home directory instead.
7.  It calls the function `package-activate-all` to activate any optional Emacs Lisp package that has been installed. See [Packaging Basics](Packaging-Basics.html). However, Emacs doesn’t activate the packages when `package-enable-at-startup` is `nil` or when it’s started with one of the options ‘`-q`’, ‘`-Q`’, or ‘`--batch`’. To activate the packages in the latter case, `package-activate-all` should be called explicitly (e.g., via the ‘`--funcall`’ option).
8.  If not running in batch mode, it initializes the window system that the variable `initial-window-system` specifies (see [initial-window-system](Window-Systems.html)). The initialization function, `window-system-initialization`, is a *generic function* (see [Generic Functions](Generic-Functions.html)) whose actual implementation is different for each supported window system. If the value of `initial-window-system` is `windowsystem`, then the appropriate implementation of the initialization function is defined in the file `term/windowsystem-win.el`. This file should have been compiled into the Emacs executable when it was built.
9.  It runs the normal hook `before-init-hook`.
10. If appropriate, it creates a graphical frame. As part of creating the graphical frame, it initializes the window system specified by `initial-frame-alist` and `default-frame-alist` (see [Initial Parameters](Initial-Parameters.html)) for the graphical frame, by calling the `window-system-initialization` function for that window system. This is not done in batch (noninteractive) or daemon mode.
11. It initializes the initial frame’s faces, and sets up the menu bar and tool bar if needed. If graphical frames are supported, it sets up the tool bar even if the current frame is not a graphical one, since a graphical frame may be created later on.
12. It use `custom-reevaluate-setting` to re-initialize the members of the list `custom-delayed-init-variables`. These are any pre-loaded user options whose default value depends on the run-time, rather than build-time, context. See [custom-initialize-delay](Building-Emacs.html).
13. It loads the library `site-start`, if it exists. This is not done if the options ‘`-Q`’ or ‘`--no-site-file`’ were specified.
14. It loads your init file (see [Init File](Init-File.html)). This is not done if the options ‘`-q`’, ‘`-Q`’, or ‘`--batch`’ were specified. If the ‘`-u`’ option was specified, Emacs looks for the init file in that user’s home directory instead.
15. It loads the library `default`, if it exists. This is not done if `inhibit-default-init` is non-`nil`, nor if the options ‘`-q`’, ‘`-Q`’, or ‘`--batch`’ were specified.
16. It loads your abbrevs from the file specified by `abbrev-file-name`, if that file exists and can be read (see [abbrev-file-name](Abbrev-Files.html)). This is not done if the option ‘`--batch`’ was specified.
17. It sets the variable `after-init-time` to the value of `current-time`. This variable was set to `nil` earlier; setting it to the current time signals that the initialization phase is over, and, together with `before-init-time`, provides the measurement of how long it took.
18. It runs the normal hook `after-init-hook`.
19. If the buffer `*scratch*` exists and is still in Fundamental mode (as it should be by default), it sets its major mode according to `initial-major-mode`.
20. If started on a text terminal, it loads the terminal-specific Lisp library (see [Terminal-Specific](Terminal_002dSpecific.html)), and runs the hook `tty-setup-hook`. This is not done in `--batch` mode, nor if `term-file-prefix` is `nil`.
21. It displays the initial echo area message, unless you have suppressed that with `inhibit-startup-echo-area-message`.
22. It processes any command-line options that were not handled earlier.
23. It now exits if the option `--batch` was specified.
24. If the `*scratch*` buffer exists and is empty, it inserts `(substitute-command-keys initial-scratch-message)` into that buffer.
25. If `initial-buffer-choice` is a string, it visits the file (or directory) with that name. If it is a function, it calls the function with no arguments and selects the buffer that it returns. If one file is given as a command line argument, that file is visited and its buffer displayed alongside `initial-buffer-choice`. If more than one file is given, all of the files are visited and the `*Buffer List*` buffer is displayed alongside `initial-buffer-choice`.
26. It runs `emacs-startup-hook`.
27. It calls `frame-notice-user-settings`, which modifies the parameters of the selected frame according to whatever the init files specify.
28. It runs `window-setup-hook`. The only difference between this hook and `emacs-startup-hook` is that this one runs after the previously mentioned modifications to the frame parameters.
29. It displays the *startup screen*, which is a special buffer that contains information about copyleft and basic Emacs usage. This is not done if `inhibit-startup-screen` or `initial-buffer-choice` are non-`nil`, or if the ‘`--no-splash`’ or ‘`-Q`’ command-line options were specified.
30. If a daemon was requested, it calls `server-start`. (On POSIX systems, if a background daemon was requested, it then detaches from the controlling terminal.) See [Emacs Server](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Server.html#Emacs-Server) in The GNU Emacs Manual.
31. If started by the X session manager, it calls `emacs-session-restore` passing it as argument the ID of the previous session. See [Session Management](Session-Management.html).

The following options affect some aspects of the startup sequence.

*   User Option: **inhibit-startup-screen**

    This variable, if non-`nil`, inhibits the startup screen. In that case, Emacs typically displays the `*scratch*` buffer; but see `initial-buffer-choice`, below.

    Do not set this variable in the init file of a new user, or in a way that affects more than one user, as that would prevent new users from receiving information about copyleft and basic Emacs usage.

    `inhibit-startup-message` and `inhibit-splash-screen` are aliases for this variable.

<!---->

*   User Option: **initial-buffer-choice**

    If non-`nil`, this variable is a string that specifies a file or directory for Emacs to display after starting up, instead of the startup screen. If its value is a function, Emacs calls that function which must return a buffer which is then displayed. If its value is `t`, Emacs displays the `*scratch*` buffer.

<!---->

*   User Option: **inhibit-startup-echo-area-message**

    This variable controls the display of the startup echo area message. You can suppress the startup echo area message by adding text with this form to your init file:

    ```lisp
    (setq inhibit-startup-echo-area-message
          "your-login-name")
    ```

    Emacs explicitly checks for an expression as shown above in your init file; your login name must appear in the expression as a Lisp string constant. You can also use the Customize interface. Other methods of setting `inhibit-startup-echo-area-message` to the same value do not inhibit the startup message. This way, you can easily inhibit the message for yourself if you wish, but thoughtless copying of your init file will not inhibit the message for someone else.

<!---->

*   User Option: **initial-scratch-message**

    This variable, if non-`nil`, should be a string, which is treated as documentation to be inserted into the `*scratch*` buffer when Emacs starts up. If it is `nil`, the `*scratch*` buffer is empty.

The following command-line options affect some aspects of the startup sequence. See [Initial Options](https://www.gnu.org/software/emacs/manual/html_node/emacs/Initial-Options.html#Initial-Options) in The GNU Emacs Manual.

*   `--no-splash`

    Do not display a splash screen.

*   `--batch`

    Run without an interactive terminal. See [Batch Mode](Batch-Mode.html).

*   *   `--daemon`
    *   `--bg-daemon`
    *   `--fg-daemon`

    Do not initialize any display; just start a server. (A “background” daemon automatically runs in the background.)

*   *   `--no-init-file`
    *   `-q`

    Do not load either the init file, or the `default` library.

*   `--no-site-file`

    Do not load the `site-start` library.

*   *   `--quick`
    *   `-Q`

    Equivalent to ‘`-q --no-site-file --no-splash`’.

Next: [Init File](Init-File.html), Up: [Starting Up](Starting-Up.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
