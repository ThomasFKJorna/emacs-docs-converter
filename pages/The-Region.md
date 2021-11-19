

Previous: [The Mark](The-Mark.html), Up: [Markers](Markers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 31.8 The Region

The text between point and the mark is known as *the region*. Various functions operate on text delimited by point and the mark, but only those functions specifically related to the region itself are described here.

The next two functions signal an error if the mark does not point anywhere. If Transient Mark mode is enabled and `mark-even-if-inactive` is `nil`, they also signal an error if the mark is inactive.

*   Function: **region-beginning**

    This function returns the position of the beginning of the region (as an integer). This is the position of either point or the mark, whichever is smaller.

<!---->

*   Function: **region-end**

    This function returns the position of the end of the region (as an integer). This is the position of either point or the mark, whichever is larger.

Instead of using `region-beginning` and `region-end`, a command designed to operate on a region should normally use `interactive` with the ‘`r`’ specification to find the beginning and end of the region. This lets other Lisp programs specify the bounds explicitly as arguments. See [Interactive Codes](Interactive-Codes.html).

*   Function: **use-region-p**

    This function returns `t` if Transient Mark mode is enabled, the mark is active, and there is a valid region in the buffer. This function is intended to be used by commands that operate on the region, instead of on text near point, when the mark is active.

    A region is valid if it has a non-zero size, or if the user option `use-empty-active-region` is non-`nil` (by default, it is `nil`). The function `region-active-p` is similar to `use-region-p`, but considers all regions as valid. In most cases, you should not use `region-active-p`, since if the region is empty it is often more appropriate to operate on point.

Previous: [The Mark](The-Mark.html), Up: [Markers](Markers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
