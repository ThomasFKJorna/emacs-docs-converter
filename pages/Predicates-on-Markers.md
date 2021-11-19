

Next: [Creating Markers](Creating-Markers.html), Previous: [Overview of Markers](Overview-of-Markers.html), Up: [Markers](Markers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 31.2 Predicates on Markers

You can test an object to see whether it is a marker, or whether it is either an integer or a marker. The latter test is useful in connection with the arithmetic functions that work with both markers and integers.

*   Function: **markerp** *object*

    This function returns `t` if `object` is a marker, `nil` otherwise. Note that integers are not markers, even though many functions will accept either a marker or an integer.

<!---->

*   Function: **integer-or-marker-p** *object*

    This function returns `t` if `object` is an integer or a marker, `nil` otherwise.

<!---->

*   Function: **number-or-marker-p** *object*

    This function returns `t` if `object` is a number (either integer or floating point) or a marker, `nil` otherwise.
