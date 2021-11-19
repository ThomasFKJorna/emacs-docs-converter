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

Next: [Frame Geometry](Frame-Geometry.html), Previous: [Creating Frames](Creating-Frames.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.2 Multiple Terminals

Emacs represents each terminal as a *terminal object* data type (see [Terminal Type](Terminal-Type.html)). On GNU and Unix systems, Emacs can use multiple terminals simultaneously in each session. On other systems, it can only use a single terminal. Each terminal object has the following attributes:

*   The name of the device used by the terminal (e.g., ‘`:0.0`’ or `/dev/tty`).
*   The terminal and keyboard coding systems used on the terminal. See [Terminal I/O Encoding](Terminal-I_002fO-Encoding.html).
*   The kind of display associated with the terminal. This is the symbol returned by the function `terminal-live-p` (i.e., `x`, `t`, `w32`, `ns`, or `pc`). See [Frames](Frames.html).
*   A list of terminal parameters. See [Terminal Parameters](Terminal-Parameters.html).

There is no primitive for creating terminal objects. Emacs creates them as needed, such as when you call `make-frame-on-display` (described below).

*   Function: **terminal-name** *\&optional terminal*

    This function returns the file name of the device used by `terminal`. If `terminal` is omitted or `nil`, it defaults to the selected frame’s terminal. `terminal` can also be a frame, meaning that frame’s terminal.

<!---->

*   Function: **terminal-list**

    This function returns a list of all live terminal objects.

<!---->

*   Function: **get-device-terminal** *device*

    This function returns a terminal whose device name is given by `device`. If `device` is a string, it can be either the file name of a terminal device, or the name of an X display of the form ‘`host:server.screen`’. If `device` is a frame, this function returns that frame’s terminal; `nil` means the selected frame. Finally, if `device` is a terminal object that represents a live terminal, that terminal is returned. The function signals an error if its argument is none of the above.

<!---->

*   Function: **delete-terminal** *\&optional terminal force*

    This function deletes all frames on `terminal` and frees the resources used by it. It runs the abnormal hook `delete-terminal-functions`, passing `terminal` as the argument to each function.

    If `terminal` is omitted or `nil`, it defaults to the selected frame’s terminal. `terminal` can also be a frame, meaning that frame’s terminal.

    Normally, this function signals an error if you attempt to delete the sole active terminal, but if `force` is non-`nil`, you are allowed to do so. Emacs automatically calls this function when the last frame on a terminal is deleted (see [Deleting Frames](Deleting-Frames.html)).

<!---->

*   Variable: **delete-terminal-functions**

    An abnormal hook run by `delete-terminal`. Each function receives one argument, the `terminal` argument passed to `delete-terminal`. Due to technical details, the functions may be called either just before the terminal is deleted, or just afterwards.

A few Lisp variables are *terminal-local*; that is, they have a separate binding for each terminal. The binding in effect at any time is the one for the terminal that the currently selected frame belongs to. These variables include `default-minibuffer-frame`, `defining-kbd-macro`, `last-kbd-macro`, and `system-key-alist`. They are always terminal-local, and can never be buffer-local (see [Buffer-Local Variables](Buffer_002dLocal-Variables.html)).

On GNU and Unix systems, each X display is a separate graphical terminal. When Emacs is started from within the X window system, it uses the X display specified by the `DISPLAY` environment variable, or by the ‘`--display`’ option (see [Initial Options](https://www.gnu.org/software/emacs/manual/html_node/emacs/Initial-Options.html#Initial-Options) in The GNU Emacs Manual). Emacs can connect to other X displays via the command `make-frame-on-display`. Each X display has its own selected frame and its own minibuffer windows; however, only one of those frames is *the* selected frame at any given moment (see [Input Focus](Input-Focus.html)). Emacs can even connect to other text terminals, by interacting with the `emacsclient` program. See [Emacs Server](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Server.html#Emacs-Server) in The GNU Emacs Manual.

A single X server can handle more than one display. Each X display has a three-part name, ‘`hostname:displaynumber.screennumber`’. The first part, `hostname`, specifies the name of the machine to which the display is physically connected. The second part, `displaynumber`, is a zero-based number that identifies one or more monitors connected to that machine that share a common keyboard and pointing device (mouse, tablet, etc.). The third part, `screennumber`, identifies a zero-based screen number (a separate monitor) that is part of a single monitor collection on that X server. When you use two or more screens belonging to one server, Emacs knows by the similarity in their names that they share a single keyboard.

Systems that don’t use the X window system, such as MS-Windows, don’t support the notion of X displays, and have only one display on each host. The display name on these systems doesn’t follow the above 3-part format; for example, the display name on MS-Windows systems is a constant string ‘`w32`’, and exists for compatibility, so that you could pass it to functions that expect a display name.

*   Command: **make-frame-on-display** *display \&optional parameters*

    This function creates and returns a new frame on `display`, taking the other frame parameters from the alist `parameters`. `display` should be the name of an X display (a string).

    Before creating the frame, this function ensures that Emacs is set up to display graphics. For instance, if Emacs has not processed X resources (e.g., if it was started on a text terminal), it does so at this time. In all other respects, this function behaves like `make-frame` (see [Creating Frames](Creating-Frames.html)).

<!---->

*   Function: **x-display-list**

    This function returns a list that indicates which X displays Emacs has a connection to. The elements of the list are strings, and each one is a display name.

<!---->

*   Function: **x-open-connection** *display \&optional xrm-string must-succeed*

    This function opens a connection to the X display `display`, without creating a frame on that display. Normally, Emacs Lisp programs need not call this function, as `make-frame-on-display` calls it automatically. The only reason for calling it is to check whether communication can be established with a given X display.

    The optional argument `xrm-string`, if not `nil`, is a string of resource names and values, in the same format used in the `.Xresources` file. See [X Resources](https://www.gnu.org/software/emacs/manual/html_node/emacs/X-Resources.html#X-Resources) in The GNU Emacs Manual. These values apply to all Emacs frames created on this display, overriding the resource values recorded in the X server. Here’s an example of what this string might look like:

        "*BorderWidth: 3\n*InternalBorder: 2\n"

    If `must-succeed` is non-`nil`, failure to open the connection terminates Emacs. Otherwise, it is an ordinary Lisp error.

<!---->

*   Function: **x-close-connection** *display*

    This function closes the connection to display `display`. Before you can do this, you must first delete all the frames that were open on that display (see [Deleting Frames](Deleting-Frames.html)).

On some multi-monitor setups, a single X display outputs to more than one physical monitor. You can use the functions `display-monitor-attributes-list` and `frame-monitor-attributes` to obtain information about such setups.

*   Function: **display-monitor-attributes-list** *\&optional display*

    This function returns a list of physical monitor attributes on `display`, which can be a display name (a string), a terminal, or a frame; if omitted or `nil`, it defaults to the selected frame’s display. Each element of the list is an association list, representing the attributes of a physical monitor. The first element corresponds to the primary monitor. The attribute keys and values are:

    *   ‘`geometry`’

        Position of the top-left corner of the monitor’s screen and its size, in pixels, as ‘`(x y width height)`’. Note that, if the monitor is not the primary monitor, some of the coordinates might be negative.

    *   ‘`workarea`’

        Position of the top-left corner and size of the work area (usable space) in pixels as ‘`(x y width height)`’. This may be different from ‘`geometry`’ in that space occupied by various window manager features (docks, taskbars, etc.) may be excluded from the work area. Whether or not such features actually subtract from the work area depends on the platform and environment. Again, if the monitor is not the primary monitor, some of the coordinates might be negative.

    *   ‘`mm-size`’

        Width and height in millimeters as ‘`(width height)`’

    *   ‘`frames`’

        List of frames that this physical monitor dominates (see below).

    *   ‘`name`’

        Name of the physical monitor as `string`.

    *   ‘`source`’

        Source of the multi-monitor information as `string`; e.g., ‘`XRandr`’ or ‘`Xinerama`’.

    `x`, `y`, `width`, and `height` are integers. ‘`name`’ and ‘`source`’ may be absent.

    A frame is *dominated* by a physical monitor when either the largest area of the frame resides in that monitor, or (if the frame does not intersect any physical monitors) that monitor is the closest to the frame. Every (non-tooltip) frame (whether visible or not) in a graphical display is dominated by exactly one physical monitor at a time, though the frame can span multiple (or no) physical monitors.

    Here’s an example of the data produced by this function on a 2-monitor display:

          (display-monitor-attributes-list)
          ⇒
          (((geometry 0 0 1920 1080) ;; Left-hand, primary monitor
            (workarea 0 0 1920 1050) ;; A taskbar occupies some of the height
            (mm-size 677 381)
            (name . "DISPLAY1")
            (frames #<frame emacs@host *Messages* 0x11578c0>
                    #<frame emacs@host *scratch* 0x114b838>))
           ((geometry 1920 0 1680 1050) ;; Right-hand monitor
            (workarea 1920 0 1680 1050) ;; Whole screen can be used
            (mm-size 593 370)
            (name . "DISPLAY2")
            (frames)))

<!---->

*   Function: **frame-monitor-attributes** *\&optional frame*

    This function returns the attributes of the physical monitor dominating (see above) `frame`, which defaults to the selected frame.

On multi-monitor displays it is possible to use the command `make-frame-on-monitor` to make frames on the specified monitor.

*   Command: **make-frame-on-monitor** *monitor \&optional display parameters*

    This function creates and returns a new frame on `monitor` located on `display`, taking the other frame parameters from the alist `parameters`. `monitor` should be the name of the physical monitor, the same string as returned by the function `display-monitor-attributes-list` in the attribute `name`. `display` should be the name of an X display (a string).

Next: [Frame Geometry](Frame-Geometry.html), Previous: [Creating Frames](Creating-Frames.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
