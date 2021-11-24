

#### 21.7.8 Motion Events

Emacs sometimes generates *mouse motion* events to describe motion of the mouse without any button activity. Mouse motion events are represented by lists that look like this:

```lisp
(mouse-movement POSITION)
```

`position` is a mouse position list (see [Click Events](Click-Events.html)), specifying the current position of the mouse cursor. As with the end-position of a drag event, this position list may represent a location outside the boundaries of the initially selected frame, in which case the list contains that frame in place of a window.

The special form `track-mouse` enables generation of motion events within its body. Outside of `track-mouse` forms, Emacs does not generate events for mere motion of the mouse, and these events do not appear. See [Mouse Tracking](Mouse-Tracking.html).

### Variable: **mouse-fine-grained-tracking**

When non-`nil`, mouse motion events are generated even for very small movements. Otherwise, motion events are not generated as long as the mouse cursor remains pointing to the same glyph in the text.
