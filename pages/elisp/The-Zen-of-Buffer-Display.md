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

Previous: [Precedence of Action Functions](Precedence-of-Action-Functions.html), Up: [Displaying Buffers](Displaying-Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 28.13.6 The Zen of Buffer Display

In its most simplistic form, a frame accommodates always one single window that can be used for displaying a buffer. As a consequence, it is always the latest call of `display-buffer` that will have succeeded in placing its buffer there.

Since working with such a frame is not very practical, Emacs by default allows for more complex frame layouts controlled by the default values of the frame size and the `split-height-threshold` and `split-width-threshold` options. Displaying a buffer not yet shown on a frame then either splits the single window on that frame or (re-)uses one of its two windows.

The default behavior is abandoned as soon as the user customizes one of these thresholds or manually changes the frame’s layout. The default behavior is also abandoned when calling `display-buffer` with a non-`nil` `action` argument or the user customizes one of the options mentioned in the previous subsections. Mastering `display-buffer` soon may become a frustrating experience due to the plethora of applicable display actions and the resulting frame layouts.

However, refraining from using buffer display functions and falling back on a split & delete windows metaphor is not a good idea either. Buffer display functions give Lisp programs and users a framework to reconcile their different needs; no comparable framework exists for splitting and deleting windows. Buffer display functions also allow to at least partially restore the layout of a frame when removing a buffer from it later (see [Quitting Windows](Quitting-Windows.html)).

Below we will give a number of guidelines to redeem the frustration mentioned above and thus to avoid literally losing buffers in-between the windows of a frame.

*   Write display actions without stress

    Writing display actions can be a pain because one has to lump together action functions and action alists in one huge list. (Historical reasons prevented us from having `display-buffer` support separate arguments for these.) It might help to memorize some basic forms like the ones listed below:

        '(nil (inhibit-same-window . t))

    specifies an action alist entry only and no action function. Its sole purpose is to inhibit a `display-buffer-same-window` function specified elsewhere from showing the buffer in the same window, see also the last example of the preceding subsection.

        '(display-buffer-below-selected)

    on the other hand, specifies one action function and an empty action alist. To combine the effects of the above two specifications one would write the form

        '(display-buffer-below-selected (inhibit-same-window . t))

    to add another action function one would write

        '((display-buffer-below-selected display-buffer-at-bottom)
          (inhibit-same-window . t))

    and to add another alist entry one would write

        '((display-buffer-below-selected display-buffer-at-bottom)
          (inhibit-same-window . t)
          (window-height . fit-window-to-buffer))

    That last form can be used as `action` argument of `display-buffer` in the following way:

        (display-buffer
         (get-buffer-create "*foo*")
         '((display-buffer-below-selected display-buffer-at-bottom)
           (inhibit-same-window . t)
           (window-height . fit-window-to-buffer)))

    In a customization of `display-buffer-alist` it would be used as follows:

        (customize-set-variable
         'display-buffer-alist
         '(("\\*foo\\*"
            (display-buffer-below-selected display-buffer-at-bottom)
            (inhibit-same-window . t)
            (window-height . fit-window-to-buffer))))

    To add a customization for a second buffer one would then write:

        (customize-set-variable
         'display-buffer-alist
         '(("\\*foo\\*"
            (display-buffer-below-selected display-buffer-at-bottom)
            (inhibit-same-window . t)
            (window-height . fit-window-to-buffer))
           ("\\*bar\\*"
            (display-buffer-reuse-window display-buffer-pop-up-frame)
            (reusable-frames . visible))))

*   Treat each other with respect

    `display-buffer-alist` and `display-buffer-base-action` are user options—Lisp programs must never set or rebind them. `display-buffer-overriding-action`, on the other hand, is reserved for applications—who seldom use that option and if they use it, then with utmost care.

    Older implementations of `display-buffer` frequently caused users and applications to fight over the settings of user options like `pop-up-frames` and `pop-up-windows` (see [Choosing Window Options](Choosing-Window-Options.html)). This was one major reason for redesigning `display-buffer`—to provide a clear framework specifying what users and applications should be allowed to do.

    Lisp programs must be prepared that user customizations may cause buffers to get displayed in an unexpected way. They should never assume in their subsequent behavior, that the buffer has been shown precisely the way they asked for in the `action` argument of `display-buffer`.

    Users should not pose too many and too severe restrictions on how arbitrary buffers get displayed. Otherwise, they will risk to lose the characteristics of showing a buffer for a certain purpose. Suppose a Lisp program has been written to compare different versions of a buffer in two windows side-by-side. If the customization of `display-buffer-alist` prescribes that any such buffer should be always shown in or below the selected window, the program will have a hard time to set up the desired window configuration via `display-buffer`.

    To specify a preference for showing an arbitrary buffer, users should customize `display-buffer-base-action`. An example of how users who prefer working with multiple frames would do that was given in the previous subsection. `display-buffer-alist` should be reserved for displaying specific buffers in a specific way.

*   Consider reusing a window that already shows the buffer

    Generally, it’s always a good idea for users and Lisp programmers to be prepared for the case that a window already shows the buffer in question and to reuse that window. In the preceding subsection we have shown that failing to do so properly may cause `display-buffer` to continuously pop up a new frame although a frame showing that buffer existed already. In a few cases only, it might be undesirable to reuse a window, for example, when a different portion of the buffer should be shown in that window.

    Hence, `display-buffer-reuse-window` is one action function that should be used as often as possible, both in `action` arguments and customizations. An `inhibit-same-window` entry in the `action` argument usually takes care of the most common case where reusing a window showing the buffer should be avoided—that where the window in question is the selected one.

*   Attract focus to the window chosen

    This is a no-brainer for people working with multiple frames—the frame showing the buffer will automatically raise and get focus unless an `inhibit-switch-frame` entry forbids it. For single frame users this task can be considerably more difficult. In particular, `display-buffer-pop-up-window` and `display-buffer-use-some-window` can become obtrusive in this regard. They split or use a seemingly arbitrary (often the largest or least recently used) window, distracting the user’s attention.

    Some Lisp programs therefore try to choose a window at the bottom of the frame, for example, in order to display the buffer in vicinity of the minibuffer window where the user is expected to answer a question related to the new window. For non-input related actions `display-buffer-below-selected` might be preferable because the selected window usually already has the user’s attention.

*   Handle subsequent invocations of `display-buffer`

    `display-buffer` is not overly well suited for displaying several buffers in sequence and making sure that all these buffers are shown orderly in the resulting window configuration. Again, the standard action functions `display-buffer-pop-up-window` and `display-buffer-use-some-window` are not very suited for this purpose due to their somewhat chaotic nature in more complex configurations.

    To produce a window configuration displaying multiple buffers (or different views of one and the same buffer) in one and the same display cycle, Lisp programmers will unavoidably have to write their own action functions. A few tricks listed below might help in this regard.

    *   Making windows atomic (see [Atomic Windows](Atomic-Windows.html)) avoids breaking an existing window composition when popping up a new window. The new window will pop up outside the composition instead.
    *   Temporarily dedicating windows to their buffers (see [Dedicated Windows](Dedicated-Windows.html)) avoids using a window for displaying a different buffer. A non-dedicated window will be used instead.
    *   Calling `window-preserve-size` (see [Preserving Window Sizes](Preserving-Window-Sizes.html)) will try to keep the size of the argument window unchanged when popping up a new window. You have to make sure that another window in the same combination can be shrunk instead, though.
    *   Side windows (see [Side Windows](Side-Windows.html)) can be used for displaying specific buffers always in a window at the same position of a frame. This permits grouping buffers that do not compete for being shown at the same time on a frame and showing any such buffer in the same window without disrupting the display of other buffers.
    *   Child frames (see [Child Frames](Child-Frames.html)) can be used to display a buffer within the screen estate of the selected frame without disrupting that frame’s window configuration and without the overhead associated with full-fledged frames as inflicted by `display-buffer-pop-up-frame`.

Previous: [Precedence of Action Functions](Precedence-of-Action-Functions.html), Up: [Displaying Buffers](Displaying-Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
