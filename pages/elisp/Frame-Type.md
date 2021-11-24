

#### 2.5.4 Frame Type

A *frame* is a screen area that contains one or more Emacs windows; we also use the term “frame” to refer to the Lisp object that Emacs uses to refer to the screen area.

Frames have no read syntax. They print in hash notation, giving the frame’s title, plus its address in core (useful to identify the frame uniquely).

```lisp
(selected-frame)
     ⇒ #<frame emacs@psilocin.gnu.org 0xdac80>
```

See [Frames](Frames.html), for a description of the functions that work on frames.
