

Next: [Atomic Windows](Atomic-Windows.html), Previous: [Quitting Windows](Quitting-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.17 Side Windows

Side windows are special windows positioned at any of the four sides of a frame’s root window (see [Windows and Frames](Windows-and-Frames.html)). In practice, this means that the area of the frame’s root window is subdivided into a main window and a number of side windows surrounding that main window. The main window is either a “normal” live window or specifies the area containing all the normal windows.

In their most simple form of use, side windows allow to display specific buffers always in the same area of a frame. Hence they can be regarded as a generalization of the concept provided by `display-buffer-at-bottom` (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)) to the remaining sides of a frame. With suitable customizations, however, side windows can be also used to provide frame layouts similar to those found in so-called integrated development environments (IDEs).

|                                                                                 |    |                                                            |
| :------------------------------------------------------------------------------ | -- | :--------------------------------------------------------- |
| • [Displaying Buffers in Side Windows](Displaying-Buffers-in-Side-Windows.html) |    | An action function for displaying buffers in side windows. |
| • [Side Window Options and Functions](Side-Window-Options-and-Functions.html)   |    | Further tuning of side windows.                            |
| • [Frame Layouts with Side Windows](Frame-Layouts-with-Side-Windows.html)       |    | Setting up frame layouts with side windows.                |
