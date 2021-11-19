

Next: [Specified Space](Specified-Space.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.16.1 Display Specs That Replace The Text

Some kinds of display specifications specify something to display instead of the text that has the property. These are called *replacing* display specifications. Emacs does not allow the user to interactively move point into the middle of buffer text that is replaced in this way.

If a list of display specifications includes more than one replacing display specification, the first overrides the rest. Replacing display specifications make most other display specifications irrelevant, since those don’t apply to the replacement.

For replacing display specifications, *the text that has the property* means all the consecutive characters that have the same Lisp object as their `display` property; these characters are replaced as a single unit. If two characters have different Lisp objects as their `display` properties (i.e., objects which are not `eq`), they are handled separately.

Here is an example which illustrates this point. A string serves as a replacing display specification, which replaces the text that has the property with the specified string (see [Other Display Specs](Other-Display-Specs.html)). Consider the following function:

```lisp
(defun foo ()
  (dotimes (i 5)
    (let ((string (concat "A"))
          (start (+ i i (point-min))))
      (put-text-property start (1+ start) 'display string)
      (put-text-property start (+ 2 start) 'display string))))
```

This function gives each of the first ten characters in the buffer a `display` property which is a string `"A"`, but they don’t all get the same string object. The first two characters get the same string object, so they are replaced with one ‘`A`’; the fact that the display property was assigned in two separate calls to `put-text-property` is irrelevant. Similarly, the next two characters get a second string (`concat` creates a new string object), so they are replaced with one ‘`A`’; and so on. Thus, the ten characters appear as five A’s.

Next: [Specified Space](Specified-Space.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
