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

Next: [Visibility of Frames](Visibility-of-Frames.html), Previous: [Minibuffers and Frames](Minibuffers-and-Frames.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.10 Input Focus

At any time, one frame in Emacs is the *selected frame*. The selected window always resides on the selected frame.

When Emacs displays its frames on several terminals (see [Multiple Terminals](Multiple-Terminals.html)), each terminal has its own selected frame. But only one of these is *the* selected frame: it’s the frame that belongs to the terminal from which the most recent input came. That is, when Emacs runs a command that came from a certain terminal, the selected frame is the one of that terminal. Since Emacs runs only a single command at any given time, it needs to consider only one selected frame at a time; this frame is what we call *the selected frame* in this manual. The display on which the selected frame is shown is the *selected frame’s display*.

*   Function: **selected-frame**

    This function returns the selected frame.

Some window systems and window managers direct keyboard input to the window object that the mouse is in; others require explicit clicks or commands to *shift the focus* to various window objects. Either way, Emacs automatically keeps track of which frames have focus. To explicitly switch to a different frame from a Lisp function, call `select-frame-set-input-focus`.

The plural “frames” in the previous paragraph is deliberate: while Emacs itself has only one selected frame, Emacs can have frames on many different terminals (recall that a connection to a window system counts as a terminal), and each terminal has its own idea of which frame has input focus. When you set the input focus to a frame, you set the focus for that frame’s terminal, but frames on other terminals may still remain focused.

Lisp programs can switch frames temporarily by calling the function `select-frame`. This does not alter the window system’s concept of focus; rather, it escapes from the window manager’s control until that control is somehow reasserted.

When using a text terminal, only one frame can be displayed at a time on the terminal, so after a call to `select-frame`, the next redisplay actually displays the newly selected frame. This frame remains selected until a subsequent call to `select-frame`. Each frame on a text terminal has a number which appears in the mode line before the buffer name (see [Mode Line Variables](Mode-Line-Variables.html)).

*   Function: **select-frame-set-input-focus** *frame \&optional norecord*

    This function selects `frame`, raises it (should it happen to be obscured by other frames) and tries to give it the window system’s focus. On a text terminal, the next redisplay displays the new frame on the entire terminal screen. The optional argument `norecord` has the same meaning as for `select-frame` (see below). The return value of this function is not significant.

Ideally, the function described next should focus a frame without also raising it above other frames. Unfortunately, many window-systems or window managers may refuse to comply.

*   Function: **x-focus-frame** *frame \&optional noactivate*

    This function gives `frame` the focus of the X server without necessarily raising it. `frame` `nil` means use the selected frame. Under X, the optional argument `noactivate`, if non-`nil`, means to avoid making `frame`’s window-system window the “active” window which should insist a bit more on avoiding to raise `frame` above other frames.

    On MS-Windows the `noactivate` argument has no effect. However, if `frame` is a child frame (see [Child Frames](Child-Frames.html)), this function usually focuses `frame` without raising it above other child frames.

    If there is no window system support, this function does nothing.

<!---->

*   Command: **select-frame** *frame \&optional norecord*

    This function selects frame `frame`, temporarily disregarding the focus of the X server if any. The selection of `frame` lasts until the next time the user does something to select a different frame, or until the next time this function is called. (If you are using a window system, the previously selected frame may be restored as the selected frame after return to the command loop, because it still may have the window system’s input focus.)

    The specified `frame` becomes the selected frame, and its terminal becomes the selected terminal. This function then calls `select-window` as a subroutine, passing the window selected within `frame` as its first argument and `norecord` as its second argument (hence, if `norecord` is non-`nil`, this avoids changing the order of recently selected windows and the buffer list). See [Selecting Windows](Selecting-Windows.html).

    This function returns `frame`, or `nil` if `frame` has been deleted.

    In general, you should never use `select-frame` in a way that could switch to a different terminal without switching back when you’re done.

Emacs cooperates with the window system by arranging to select frames as the server and window manager request. When a window system informs Emacs that one of its frames has been selected, Emacs internally generates a *focus-in* event. When an Emacs frame is displayed on a text-terminal emulator, such as `xterm`, which supports reporting of focus-change notification, the focus-in and focus-out events are available even for text-mode frames. Focus events are normally handled by `handle-focus-in`.

*   Command: **handle-focus-in** *event*

    This function handles focus-in events from window systems and terminals that support explicit focus notifications. It updates the per-frame focus flags that `frame-focus-state` queries and calls `after-focus-change-function`. In addition, it generates a `switch-frame` event in order to switch the Emacs notion of the selected frame to the frame most recently focused in some terminal. It’s important to note that this switching of the Emacs selected frame to the most recently focused frame does not mean that other frames do not continue to have the focus in their respective terminals. Do not invoke this function yourself: instead, attach logic to `after-focus-change-function`.

<!---->

*   Command: **handle-switch-frame** *frame*

    This function handles a switch-frame event, which Emacs generates for itself upon focus notification or under various other circumstances involving an input event arriving at a different frame from the last event. Do not invoke this function yourself.

<!---->

*   Function: **redirect-frame-focus** *frame \&optional focus-frame*

    This function redirects focus from `frame` to `focus-frame`. This means that `focus-frame` will receive subsequent keystrokes and events intended for `frame`. After such an event, the value of `last-event-frame` will be `focus-frame`. Also, switch-frame events specifying `frame` will instead select `focus-frame`.

    If `focus-frame` is omitted or `nil`, that cancels any existing redirection for `frame`, which therefore once again receives its own events.

    One use of focus redirection is for frames that don’t have minibuffers. These frames use minibuffers on other frames. Activating a minibuffer on another frame redirects focus to that frame. This puts the focus on the minibuffer’s frame, where it belongs, even though the mouse remains in the frame that activated the minibuffer.

    Selecting a frame can also change focus redirections. Selecting frame `bar`, when `foo` had been selected, changes any redirections pointing to `foo` so that they point to `bar` instead. This allows focus redirection to work properly when the user switches from one frame to another using `select-window`.

    This means that a frame whose focus is redirected to itself is treated differently from a frame whose focus is not redirected. `select-frame` affects the former but not the latter.

    The redirection lasts until `redirect-frame-focus` is called to change it.

<!---->

*   Function: **frame-focus-state** *frame*

    This function retrieves the last known focus state of `frame`.

    It returns `nil` if the frame is known not to be focused, `t` if the frame is known to be focused, or `unknown` if Emacs does not know the focus state of the frame. (You may see this last state in TTY frames running on terminals that do not support explicit focus notifications.)

<!---->

*   Variable: **after-focus-change-function**

    This function is an extension point that code can use to receive a notification that focus has changed.

    This function is called with no arguments when Emacs notices that the set of focused frames may have changed. Code wanting to do something when frame focus changes should use `add-function` to add a function to this one, and in this added function, re-scan the set of focused frames, calling `frame-focus-state` to retrieve the last known focus state of each frame. Focus events are delivered asynchronously, and frame input focus according to an external system may not correspond to the notion of the Emacs selected frame. Multiple frames may appear to have input focus simultaneously due to focus event delivery differences, the presence of multiple Emacs terminals, and other factors, and code should be robust in the face of this situation.

    Depending on window system, focus events may also be delivered repeatedly and with different focus states before settling to the expected values. Code relying on focus notifications should “debounce” any user-visible updates arising from focus changes, perhaps by deferring work until redisplay.

    This function may be called in arbitrary contexts, including from inside `read-event`, so take the same care as you might when writing a process filter.

<!---->

*   User Option: **focus-follows-mouse**

    This option informs Emacs whether and how the window manager transfers focus when you move the mouse pointer into a frame. It can have three meaningful values:

    *   `nil`

        The default value `nil` should be used when your window manager follows a “click-to-focus” policy where you have to click the mouse inside of a frame in order for that frame to gain focus.

    *   `t`

        The value `t` should be used when your window manager has the focus automatically follow the position of the mouse pointer but a frame that gains focus is not raised automatically and may even remain occluded by other window-system windows.

    *   `auto-raise`

        The value `auto-raise` should be used when your window manager has the focus automatically follow the position of the mouse pointer and a frame that gains focus is raised automatically.

    If this option is non-`nil`, Emacs moves the mouse pointer to the frame selected by `select-frame-set-input-focus`. That function is used by a number of commands like, for example, `other-frame` and `pop-to-buffer`.

    The distinction between the values `t` and `auto-raise` is not needed for “normal” frames because the window manager usually takes care of raising them. It is useful to automatically raise child frames via `mouse-autoselect-window` (see [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html)).

    Note that this option does not distinguish “sloppy” focus (where the frame that previously had focus retains focus as long as the mouse pointer does not move into another window manager window) from “strict” focus (where a frame immediately loses focus when it’s left by the mouse pointer). Neither does it recognize whether your window manager supports delayed focusing or auto-raising where you can explicitly specify the time until a new frame gets focus or is auto-raised.

    You can supply a “focus follows mouse” policy for individual Emacs windows by customizing the variable `mouse-autoselect-window` (see [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html)).

Next: [Visibility of Frames](Visibility-of-Frames.html), Previous: [Minibuffers and Frames](Minibuffers-and-Frames.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
