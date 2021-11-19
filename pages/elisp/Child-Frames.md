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

Next: [Mouse Tracking](Mouse-Tracking.html), Previous: [Frame Configurations](Frame-Configurations.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.14 Child Frames

Child frames are objects halfway between windows (see [Windows](Windows.html)) and “normal” frames. Like windows, they are attached to an owning frame. Unlike windows, they may overlap each other—changing the size or position of one child frame does not change the size or position of any of its sibling child frames.

By design, operations to make or modify child frames are implemented with the help of frame parameters (see [Frame Parameters](Frame-Parameters.html)) without any specialized functions or customizable variables. Note that child frames are meaningful on graphical terminals only.

To create a new child frame or to convert a normal frame into a child frame, set that frame’s `parent-frame` parameter (see [Frame Interaction Parameters](Frame-Interaction-Parameters.html)) to that of an already existing frame. The frame specified by that parameter will then be the frame’s parent frame as long as the parameter is not changed or reset. Technically, this makes the child frame’s window-system window a child window of the parent frame’s window-system window.

The `parent-frame` parameter can be changed at any time. Setting it to another frame *reparents* the child frame. Setting it to another child frame makes the frame a *nested* child frame. Setting it to `nil` restores the frame’s status as a top-level frame—a frame whose window-system window is a child of its display’s root window.

Since child frames can be arbitrarily nested, a frame can be both a child and a parent frame. Also, the relative roles of child and parent frame may be reversed at any time (though it’s usually a good idea to keep the size of a child frame sufficiently smaller than that of its parent). An error will be signaled for the attempt to make a frame an ancestor of itself.

Most window-systems clip a child frame at the native edges (see [Frame Geometry](Frame-Geometry.html)) of its parent frame—everything outside these edges is usually invisible. A child frame’s `left` and `top` parameters specify a position relative to the top-left corner of its parent’s native frame. When the parent frame is resized, this position remains conceptually unaltered.

NS builds do not clip child frames at the parent frame’s edges, allowing them to be positioned so they do not obscure the parent frame while still being visible themselves.

Usually, moving a parent frame moves along all its child frames and their descendants as well, keeping their relative positions unaltered. Note that the hook `move-frame-functions` (see [Frame Position](Frame-Position.html)) is run for a child frame only when the position of the child frame relative to its parent frame changes.

When a parent frame is resized, its child frames conceptually retain their previous sizes and their positions relative to the left upper corner of the parent. This means that a child frame may become (partially) invisible when its parent frame shrinks. The parameter `keep-ratio` (see [Frame Interaction Parameters](Frame-Interaction-Parameters.html)) can be used to resize and reposition a child frame proportionally whenever its parent frame is resized. This may avoid obscuring parts of a frame when its parent frame is shrunk.

A visible child frame always appears on top of its parent frame thus obscuring parts of it, except on NS builds where it may be positioned beneath the parent. This is comparable to the window-system window of a top-level frame which also always appears on top of its parent window—the desktop’s root window. When a parent frame is iconified or made invisible (see [Visibility of Frames](Visibility-of-Frames.html)), its child frames are made invisible. When a parent frame is deiconified or made visible, its child frames are made visible.

When a parent frame is about to be deleted (see [Deleting Frames](Deleting-Frames.html)), its child frames are recursively deleted before it. There is one exception to this rule: When the child frame serves as a surrogate minibuffer frame (see [Minibuffers and Frames](Minibuffers-and-Frames.html)) for another frame, it is retained until the parent frame has been deleted. If, at this time, no remaining frame uses the child frame as its minibuffer frame, Emacs will try to delete the child frame too. If that deletion fails for whatever reason, the child frame is made a top-level frame.

Whether a child frame can have a menu or tool bar is window-system or window manager dependent. Most window-systems explicitly disallow menu bars for child frames. It seems advisable to disable both, menu and tool bars, via the frame’s initial parameters settings.

Usually, child frames do not exhibit window manager decorations like a title bar or external borders (see [Frame Geometry](Frame-Geometry.html)). When the child frame does not show a menu or tool bar, any other of the frame’s borders (see [Layout Parameters](Layout-Parameters.html)) can be used instead of the external borders.

In particular, under X (but not when building with GTK+), the frame’s outer border can be used. On MS-Windows, specifying a non-zero outer border width will show a one-pixel wide external border. Under all window-systems, the internal border can be used. In either case, it’s advisable to disable a child frame’s window manager decorations with the `undecorated` frame parameter (see [Management Parameters](Management-Parameters.html)).

To resize or move an undecorated child frame with the mouse, special frame parameters (see [Mouse Dragging Parameters](Mouse-Dragging-Parameters.html)) have to be used. The internal border of a child frame, if present, can be used to resize the frame with the mouse, provided that frame has a non-`nil` `drag-internal-border` parameter. If set, the `snap-width` parameter indicates the number of pixels where the frame *snaps* at the respective edge or corner of its parent frame.

There are two ways to drag an entire child frame with the mouse: The `drag-with-mode-line` parameter, if non-`nil`, allows to drag a frame without minibuffer window (see [Minibuffer Windows](Minibuffer-Windows.html)) via the mode line area of its bottommost window. The `drag-with-header-line` parameter, if non-`nil`, allows to drag the frame via the header line area of its topmost window.

In order to give a child frame a draggable header or mode line, the window parameters `mode-line-format` and `header-line-format` are handy (see [Window Parameters](Window-Parameters.html)). These allow to remove an unwanted mode line (when `drag-with-header-line` is chosen) and to remove mouse-sensitive areas which might interfere with frame dragging.

To avoid that dragging moves a frame completely out of its parent’s native frame, something which might happen when the mouse cursor overshoots and makes the frame difficult to retrieve once the mouse button has been released, it is advisable to set the frame’s `top-visible` or `bottom-visible` parameter correspondingly.

The `top-visible` parameter specifies the number of pixels at the top of the frame that always remain visible within the parent’s native frame during dragging and should be set when specifying a non-`nil` `drag-with-header-line` parameter. The `bottom-visible` parameter specifies the number of pixels at the bottom of the frame that always remain visible within the parent’s native frame during dragging and should be preferred when specifying a non-`nil` `drag-with-mode-line` parameter.

When a child frame is used for displaying a buffer via `display-buffer-in-child-frame` (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)), the frame’s `auto-hide-function` parameter (see [Frame Interaction Parameters](Frame-Interaction-Parameters.html)) can be set to a function, in order to appropriately deal with the frame when the window displaying the buffer shall be quit.

When a child frame is used during minibuffer interaction, for example, to display completions in a separate window, the `minibuffer-exit` parameter (see [Frame Interaction Parameters](Frame-Interaction-Parameters.html)) is useful in order to deal with the frame when the minibuffer is exited.

The behavior of child frames deviates from that of top-level frames in a number of other ways as well. Here we sketch a few of them:

*   The semantics of maximizing and iconifying child frames is highly window-system dependent. As a rule, applications should never invoke these operations on child frames. By default, invoking `iconify-frame` on a child frame will try to iconify the top-level frame corresponding to that child frame instead. To obtain a different behavior, users may customize the option `iconify-child-frame` described below.
*   Raising, lowering and restacking child frames (see [Raising and Lowering](Raising-and-Lowering.html)) or changing the `z-group` (see [Position Parameters](Position-Parameters.html)) of a child frame changes only the stacking order of child frames with the same parent.
*   Many window-systems are not able to change the opacity (see [Font and Color Parameters](Font-and-Color-Parameters.html)) of child frames.
*   Transferring focus from a child frame to an ancestor that is not its parent by clicking with the mouse in a visible part of that ancestor’s window may fail with some window-systems. You may have to click into the direct parent’s window-system window first.
*   Window managers might not bother to extend their focus follows mouse policy to child frames. Customizing `mouse-autoselect-window` can help in this regard (see [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html)).
*   Dropping (see [Drag and Drop](Drag-and-Drop.html)) on child frames is not guaranteed to work on all window-systems. Some will drop the object on the parent frame or on some ancestor instead.

The following two functions can be useful when working with child and parent frames:

*   Function: **frame-parent** *\&optional frame*

    This function returns the parent frame of `frame`. The parent frame of `frame` is the Emacs frame whose window-system window is the parent window of `frame`’s window-system window. If such a frame exists, `frame` is considered a child frame of that frame.

    This function returns `nil` if `frame` has no parent frame.

<!---->

*   Function: **frame-ancestor-p** *ancestor descendant*

    This functions returns non-`nil` if `ancestor` is an ancestor of `descendant`. `ancestor` is an ancestor of `descendant` when it is either `descendant`’s parent frame or it is an ancestor of `descendant`’s parent frame. Both, `ancestor` and `descendant` must specify live frames.

Note also the function `window-largest-empty-rectangle` (see [Coordinates and Windows](Coordinates-and-Windows.html)) which can be used to inscribe a child frame in the largest empty area of an existing window. This can be useful to avoid that a child frame obscures any text shown in that window.

Customizing the following option can be useful to tweak the behavior of `iconify-frame` for child frames.

*   User Option: **iconify-child-frame**

    This option tells Emacs how to proceed when it is asked to iconify a child frame. If it is `nil`, `iconify-frame` will do nothing when invoked on a child frame. If it is `iconify-top-level`, Emacs will try to iconify the top-level frame that is the ancestor of this child frame instead. If it is `make-invisible`, Emacs will try to make this child frame invisible instead of iconifying it.

    Any other value means to try iconifying the child frame. Since such an attempt may not be honored by all window managers and can even lead to making the child frame unresponsive to user actions, the default is to iconify the top level frame instead.

Next: [Mouse Tracking](Mouse-Tracking.html), Previous: [Frame Configurations](Frame-Configurations.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
