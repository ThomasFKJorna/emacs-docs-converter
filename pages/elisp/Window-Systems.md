

### 39.24 Window Systems

Emacs works with several window systems, most notably the X Window System. Both Emacs and X use the term “window”, but use it differently. An Emacs frame is a single window as far as X is concerned; the individual Emacs windows are not known to X at all.

### Variable: **window-system**

This terminal-local variable tells Lisp programs what window system Emacs is using for displaying the frame. The possible values are

*   `x`

    Emacs is displaying the frame using X.

*   `w32`

    Emacs is displaying the frame using native MS-Windows GUI.

*   `ns`

    Emacs is displaying the frame using the Nextstep interface (used on GNUstep and macOS).

*   `pc`

    Emacs is displaying the frame using MS-DOS direct screen writes.

*   `nil`

    Emacs is displaying the frame on a character-based terminal.

### Variable: **initial-window-system**

This variable holds the value of `window-system` used for the first frame created by Emacs during startup. (When Emacs is invoked as a daemon, it does not create any initial frames, so `initial-window-system` is `nil`, except on MS-Windows, where it is still `w32`. See [daemon](https://www.gnu.org/software/emacs/manual/html_node/emacs/Initial-Options.html#Initial-Options) in The GNU Emacs Manual.)

### Function: **window-system** *\&optional frame*

This function returns a symbol whose name tells what window system is used for displaying `frame` (which defaults to the currently selected frame). The list of possible symbols it returns is the same one documented for the variable `window-system` above.

Do *not* use `window-system` and `initial-window-system` as predicates or boolean flag variables, if you want to write code that works differently on text terminals and graphic displays. That is because `window-system` is not a good indicator of Emacs capabilities on a given display type. Instead, use `display-graphic-p` or any of the other `display-*-p` predicates described in [Display Feature Testing](Display-Feature-Testing.html).
