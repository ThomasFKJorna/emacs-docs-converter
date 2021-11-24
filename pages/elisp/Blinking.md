

### 39.21 Blinking Parentheses

This section describes the mechanism by which Emacs shows a matching open parenthesis when the user inserts a close parenthesis.

### Variable: **blink-paren-function**

The value of this variable should be a function (of no arguments) to be called whenever a character with close parenthesis syntax is inserted. The value of `blink-paren-function` may be `nil`, in which case nothing is done.

### User Option: **blink-matching-paren**

If this variable is `nil`, then `blink-matching-open` does nothing.

### User Option: **blink-matching-paren-distance**

This variable specifies the maximum distance to scan for a matching parenthesis before giving up.

### User Option: **blink-matching-delay**

This variable specifies the number of seconds to keep indicating the matching parenthesis. A fraction of a second often gives good results, but the default is 1, which works on all systems.

### Command: **blink-matching-open**

This function is the default value of `blink-paren-function`. It assumes that point follows a character with close parenthesis syntax and applies the appropriate effect momentarily to the matching opening character. If that character is not already on the screen, it displays the character’s context in the echo area. To avoid long delays, this function does not search farther than `blink-matching-paren-distance` characters.

Here is an example of calling this function explicitly.

```lisp
(defun interactive-blink-matching-open ()
  "Indicate momentarily the start of parenthesized sexp before point."
  (interactive)
```

```lisp
  (let ((blink-matching-paren-distance
         (buffer-size))
        (blink-matching-paren t))
    (blink-matching-open)))
```
