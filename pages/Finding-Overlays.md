

Previous: [Overlay Properties](Overlay-Properties.html), Up: [Overlays](Overlays.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.9.3 Searching for Overlays

*   Function: **overlays-at** *pos \&optional sorted*

    This function returns a list of all the overlays that cover the character at position `pos` in the current buffer. If `sorted` is non-`nil`, the list is in decreasing order of priority, otherwise it is in no particular order. An overlay contains position `pos` if it begins at or before `pos`, and ends after `pos`.

    To illustrate usage, here is a Lisp function that returns a list of the overlays that specify property `prop` for the character at point:

    ```lisp
    (defun find-overlays-specifying (prop)
      (let ((overlays (overlays-at (point)))
            found)
        (while overlays
          (let ((overlay (car overlays)))
            (if (overlay-get overlay prop)
                (setq found (cons overlay found))))
          (setq overlays (cdr overlays)))
        found))
    ```

<!---->

*   Function: **overlays-in** *beg end*

    This function returns a list of the overlays that overlap the region `beg` through `end`. An overlay overlaps with a region if it contains one or more characters in the region; empty overlays (see [empty overlay](Managing-Overlays.html)) overlap if they are at `beg`, strictly between `beg` and `end`, or at `end` when `end` denotes the position at the end of the buffer.

<!---->

*   Function: **next-overlay-change** *pos*

    This function returns the buffer position of the next beginning or end of an overlay, after `pos`. If there is none, it returns `(point-max)`.

<!---->

*   Function: **previous-overlay-change** *pos*

    This function returns the buffer position of the previous beginning or end of an overlay, before `pos`. If there is none, it returns `(point-min)`.

As an example, here’s a simplified (and inefficient) version of the primitive function `next-single-char-property-change` (see [Property Search](Property-Search.html)). It searches forward from position `pos` for the next position where the value of a given property `prop`, as obtained from either overlays or text properties, changes.

```lisp
(defun next-single-char-property-change (position prop)
  (save-excursion
    (goto-char position)
    (let ((propval (get-char-property (point) prop)))
      (while (and (not (eobp))
                  (eq (get-char-property (point) prop) propval))
        (goto-char (min (next-overlay-change (point))
                        (next-single-property-change (point) prop)))))
    (point)))
```

Previous: [Overlay Properties](Overlay-Properties.html), Up: [Overlays](Overlays.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
