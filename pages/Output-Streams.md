<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Output Functions](Output-Functions.html), Previous: [Input Functions](Input-Functions.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 19.4 Output Streams

An output stream specifies what to do with the characters produced by printing. Most print functions accept an output stream as an optional argument. Here are the possible types of output stream:

*   `buffer`

    The output characters are inserted into `buffer` at point. Point advances as characters are inserted.

*   `marker`

    The output characters are inserted into the buffer that `marker` points into, at the marker position. The marker position advances as characters are inserted. The value of point in the buffer has no effect on printing when the stream is a marker, and this kind of printing does not move point (except that if the marker points at or before the position of point, point advances with the surrounding text, as usual).

*   `function`

    The output characters are passed to `function`, which is responsible for storing them away. It is called with a single character as argument, as many times as there are characters to be output, and is responsible for storing the characters wherever you want to put them.

*   `t`

    The output characters are displayed in the echo area. If Emacs is running in batch mode (see [Batch Mode](Batch-Mode.html)), the output is written to the standard output descriptor instead.

*   `nil`

    `nil` specified as an output stream means to use the value of the `standard-output` variable instead; that value is the *default output stream*, and must not be `nil`.

*   `symbol`

    A symbol as output stream is equivalent to the symbol’s function definition (if any).

Many of the valid output streams are also valid as input streams. The difference between input and output streams is therefore more a matter of how you use a Lisp object, than of different types of object.

Here is an example of a buffer used as an output stream. Point is initially located as shown immediately before the ‘`h`’ in ‘`the`’. At the end, point is located directly before that same ‘`h`’.

    ---------- Buffer: foo ----------
    This is t∗he contents of foo.
    ---------- Buffer: foo ----------

```

(print "This is the output" (get-buffer "foo"))
     ⇒ "This is the output"
```

    ---------- Buffer: foo ----------
    This is t
    "This is the output"
    ∗he contents of foo.
    ---------- Buffer: foo ----------

Now we show a use of a marker as an output stream. Initially, the marker is in buffer `foo`, between the ‘`t`’ and the ‘`h`’ in the word ‘`the`’. At the end, the marker has advanced over the inserted text so that it remains positioned before the same ‘`h`’. Note that the location of point, shown in the usual fashion, has no effect.

    ---------- Buffer: foo ----------
    This is the ∗output
    ---------- Buffer: foo ----------

```
```

    (setq m (copy-marker 10))
         ⇒ #<marker at 10 in foo>

```
```

    (print "More output for foo." m)
         ⇒ "More output for foo."

```
```

    ---------- Buffer: foo ----------
    This is t
    "More output for foo."
    he ∗output
    ---------- Buffer: foo ----------

```
```

    m
         ⇒ #<marker at 34 in foo>

The following example shows output to the echo area:

    (print "Echo Area output" t)
         ⇒ "Echo Area output"
    ---------- Echo Area ----------
    "Echo Area output"
    ---------- Echo Area ----------

Finally, we show the use of a function as an output stream. The function `eat-output` takes each character that it is given and conses it onto the front of the list `last-output` (see [Building Lists](Building-Lists.html)). At the end, the list contains all the characters output, but in reverse order.

    (setq last-output nil)
         ⇒ nil

```
```

    (defun eat-output (c)
      (setq last-output (cons c last-output)))
         ⇒ eat-output

```
```

    (print "This is the output" #'eat-output)
         ⇒ "This is the output"

```
```

    last-output
         ⇒ (10 34 116 117 112 116 117 111 32 101 104
        116 32 115 105 32 115 105 104 84 34 10)

Now we can put the output in the proper order by reversing the list:

    (concat (nreverse last-output))
         ⇒ "
    \"This is the output\"
    "

Calling `concat` converts the list to a string so you can see its contents more clearly.

*   Function: **external-debugging-output** *character*

    This function can be useful as an output stream when debugging. It writes `character` to the standard error stream.

    For example

        (print "This is the output" #'external-debugging-output)
        -| This is the output
        ⇒ "This is the output"

Next: [Output Functions](Output-Functions.html), Previous: [Input Functions](Input-Functions.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
