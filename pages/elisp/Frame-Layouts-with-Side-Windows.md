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

Previous: [Side Window Options and Functions](Side-Window-Options-and-Functions.html), Up: [Side Windows](Side-Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 28.17.3 Frame Layouts with Side Windows

Side windows can be used to create more complex frame layouts like those provided by integrated development environments (IDEs). In such layouts, the area of the main window is where the normal editing activities take place. Side windows are not conceived for editing in the usual sense. Rather, they are supposed to display information complementary to the current editing activity, like lists of files, tags or buffers, help information, search or grep results or shell output.

The layout of such a frame might appear as follows:

         ___________________________________
        |          *Buffer List*            |
        |___________________________________|
        |     |                       |     |
        |  *  |                       |  *  |
        |  d  |                       |  T  |
        |  i  |                       |  a  |
        |  r  |   Main Window Area    |  g  |
        |  e  |                       |  s  |
        |  d  |                       |  *  |
        |  *  |                       |     |
        |_____|_______________________|_____|
        | *help*/*grep*/  |  *shell*/       |
        | *Completions*   |  *compilation*  |
        |_________________|_________________|
        |             Echo Area             |
        |___________________________________|

The following example illustrates how window parameters (see [Window Parameters](Window-Parameters.html)) can be used with `display-buffer-in-side-window` (see [Displaying Buffers in Side Windows](Displaying-Buffers-in-Side-Windows.html)) to set up code for producing the frame layout sketched above.

    (defvar parameters
      '(window-parameters . ((no-other-window . t)
                             (no-delete-other-windows . t))))

    (setq fit-window-to-buffer-horizontally t)
    (setq window-resize-pixelwise t)

    (setq
     display-buffer-alist
     `(("\\*Buffer List\\*" display-buffer-in-side-window
        (side . top) (slot . 0) (window-height . fit-window-to-buffer)
        (preserve-size . (nil . t)) ,parameters)
       ("\\*Tags List\\*" display-buffer-in-side-window
        (side . right) (slot . 0) (window-width . fit-window-to-buffer)
        (preserve-size . (t . nil)) ,parameters)
       ("\\*\\(?:help\\|grep\\|Completions\\)\\*"
        display-buffer-in-side-window
        (side . bottom) (slot . -1) (preserve-size . (nil . t))
        ,parameters)
       ("\\*\\(?:shell\\|compilation\\)\\*" display-buffer-in-side-window
        (side . bottom) (slot . 1) (preserve-size . (nil . t))
        ,parameters)))

This specifies `display-buffer-alist` entries (see [Choosing Window](Choosing-Window.html)) for buffers with fixed names. In particular, it asks for showing `*Buffer List*` with adjustable height at the top of the frame and `*Tags List*` with adjustable width on the frame’s right. It also asks for having the `*help*`, `*grep*` and `*Completions*` buffers share a window on the bottom left side of the frame and the `*shell*` and `*compilation*` buffers appear in a window on the bottom right side of the frame.

Note that the option `fit-window-to-buffer-horizontally` must have a non-`nil` value in order to allow horizontal adjustment of windows. Entries are also added that ask for preserving the height of side windows at the top and bottom of the frame and the width of side windows at the left or right of the frame. To assure that side windows retain their respective sizes when maximizing the frame, the variable `window-resize-pixelwise` is set to a non-`nil` value. See [Resizing Windows](Resizing-Windows.html).

The last form also makes sure that none of the created side windows are accessible via `C-x o`<!-- /@w --> by installing the `no-other-window` parameter for each of these windows. In addition, it makes sure that side windows are not deleted via `C-x 1`<!-- /@w --> by installing the `no-delete-other-windows` parameter for each of these windows.

Since `dired` buffers have no fixed names, we use a special function `dired-default-directory-on-left` in order to display a lean directory buffer on the left side of the frame.

    (defun dired-default-directory-on-left ()
      "Display `default-directory' in side window on left, hiding details."
      (interactive)
      (let ((buffer (dired-noselect default-directory)))
        (with-current-buffer buffer (dired-hide-details-mode t))
        (display-buffer-in-side-window
         buffer `((side . left) (slot . 0)
                  (window-width . fit-window-to-buffer)
                  (preserve-size . (t . nil)) ,parameters))))

Evaluating the preceding forms and typing, in any order, `M-x list-buffers`<!-- /@w -->, `C-h f`, `M-x shell`, `M-x list-tags`<!-- /@w -->, and `M-x dired-default-directory-on-left` should now reproduce the frame layout sketched above.

Previous: [Side Window Options and Functions](Side-Window-Options-and-Functions.html), Up: [Side Windows](Side-Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
