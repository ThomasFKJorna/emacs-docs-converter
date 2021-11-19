

Next: [Multiple Terminals](Multiple-Terminals.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.1 Creating Frames

To create a new frame, call the function `make-frame`.

*   Command: **make-frame** *\&optional parameters*

    This function creates and returns a new frame, displaying the current buffer.

    The `parameters` argument is an alist that specifies frame parameters for the new frame. See [Frame Parameters](Frame-Parameters.html). If you specify the `terminal` parameter in `parameters`, the new frame is created on that terminal. Otherwise, if you specify the `window-system` frame parameter in `parameters`, that determines whether the frame should be displayed on a text terminal or a graphical terminal. See [Window Systems](Window-Systems.html). If neither is specified, the new frame is created in the same terminal as the selected frame.

    Any parameters not mentioned in `parameters` default to the values in the alist `default-frame-alist` (see [Initial Parameters](Initial-Parameters.html)); parameters not specified there default from the X resources or its equivalent on your operating system (see [X Resources](https://www.gnu.org/software/emacs/manual/html_node/emacs/X-Resources.html#X-Resources) in The GNU Emacs Manual). After the frame is created, this function applies any parameters specified in `frame-inherited-parameters` (see below) it has no assigned yet, taking the values from the frame that was selected when `make-frame` was called.

    Note that on multi-monitor displays (see [Multiple Terminals](Multiple-Terminals.html)), the window manager might position the frame differently than specified by the positional parameters in `parameters` (see [Position Parameters](Position-Parameters.html)). For example, some window managers have a policy of displaying the frame on the monitor that contains the largest part of the window (a.k.a. the *dominating* monitor).

    This function itself does not make the new frame the selected frame. See [Input Focus](Input-Focus.html). The previously selected frame remains selected. On graphical terminals, however, the windowing system may select the new frame for its own reasons.

<!---->

*   Variable: **before-make-frame-hook**

    A normal hook run by `make-frame` before it creates the frame.

<!---->

*   Variable: **after-make-frame-functions**

    An abnormal hook run by `make-frame` after it created the frame. Each function in `after-make-frame-functions` receives one argument, the frame just created.

Note that any functions added to these hooks by your initial file are usually not run for the initial frame, since Emacs reads the initial file only after creating that frame. However, if the initial frame is specified to use a separate minibuffer frame (see [Minibuffers and Frames](Minibuffers-and-Frames.html)), the functions will be run for both, the minibuffer-less and the minibuffer frame.

*   Variable: **frame-inherited-parameters**

    This variable specifies the list of frame parameters that a newly created frame inherits from the currently selected frame. For each parameter (a symbol) that is an element in this list and has not been assigned earlier when processing `make-frame`, the function sets the value of that parameter in the created frame to its value in the selected frame.

<!---->

*   User Option: **server-after-make-frame-hook**

    A normal hook run when the Emacs server creates a client frame. When this hook is called, the created frame is the selected one. See [Emacs Server](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Server.html#Emacs-Server) in The GNU Emacs Manual.

Next: [Multiple Terminals](Multiple-Terminals.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
