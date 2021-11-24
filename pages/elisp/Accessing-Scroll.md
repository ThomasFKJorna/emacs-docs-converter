

#### 21.7.14 Accessing Scroll Bar Events

These functions are useful for decoding scroll bar events.

### Function: **scroll-bar-event-ratio** *event*

This function returns the fractional vertical position of a scroll bar event within the scroll bar. The value is a cons cell `(portion . whole)` containing two integers whose ratio is the fractional position.

### Function: **scroll-bar-scale** *ratio total*

This function multiplies (in effect) `ratio` by `total`, rounding the result to an integer. The argument `ratio` is not a number, but rather a pair `(num . denom)`—typically a value returned by `scroll-bar-event-ratio`.

This function is handy for scaling a position on a scroll bar into a buffer position. Here’s how to do that:

```lisp
(+ (point-min)
   (scroll-bar-scale
      (posn-x-y (event-start event))
      (- (point-max) (point-min))))
```

Recall that scroll bar events have two integers forming a ratio, in place of a pair of x and y coordinates.
