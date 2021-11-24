

#### 2.5.2 Marker Type

A *marker* denotes a position in a specific buffer. Markers therefore have two components: one for the buffer, and one for the position. Changes in the buffer’s text automatically relocate the position value as necessary to ensure that the marker always points between the same two characters in the buffer.

Markers have no read syntax. They print in hash notation, giving the current character position and the name of the buffer.

```lisp
(point-marker)
     ⇒ #<marker at 10779 in objects.texi>
```

See [Markers](Markers.html), for information on how to test, create, copy, and move markers.
