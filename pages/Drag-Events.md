

Next: [Button-Down Events](Button_002dDown-Events.html), Previous: [Click Events](Click-Events.html), Up: [Input Events](Input-Events.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 21.7.5 Drag Events

With Emacs, you can have a drag event without even changing your clothes. A *drag event* happens every time the user presses a mouse button and then moves the mouse to a different character position before releasing the button. Like all mouse events, drag events are represented in Lisp as lists. The lists record both the starting mouse position and the final position, like this:

```lisp
(event-type
 (window1 START-POSITION)
 (window2 END-POSITION))
```

For a drag event, the name of the symbol `event-type` contains the prefix ‘`drag-`’. For example, dragging the mouse with button 2 held down generates a `drag-mouse-2` event. The second and third elements of the event give the starting and ending position of the drag, as mouse position lists (see [Click Events](Click-Events.html)). You can access the second element of any mouse event in the same way. However, the drag event may end outside the boundaries of the frame that was initially selected. In that case, the third element’s position list contains that frame in place of a window.

The ‘`drag-`’ prefix follows the modifier key prefixes such as ‘`C-`’ and ‘`M-`’.

If `read-key-sequence` receives a drag event that has no key binding, and the corresponding click event does have a binding, it changes the drag event into a click event at the drag’s starting position. This means that you don’t have to distinguish between click and drag events unless you want to.
