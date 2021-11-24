

### 32.1 Examining Text Near Point

Many functions are provided to look at the characters around point. Several simple functions are described here. See also `looking-at` in [Regexp Search](Regexp-Search.html).

In the following four functions, “beginning” or “end” of buffer refers to the beginning or end of the accessible portion.

### Function: **char-after** *\&optional position*

This function returns the character in the current buffer at (i.e., immediately after) position `position`. If `position` is out of range for this purpose, either before the beginning of the buffer, or at or beyond the end, then the value is `nil`. The default for `position` is point.

In the following example, assume that the first character in the buffer is ‘`@`’:

```lisp
(string (char-after 1))
     ⇒ "@"
```

### Function: **char-before** *\&optional position*

This function returns the character in the current buffer immediately before position `position`. If `position` is out of range for this purpose, either at or before the beginning of the buffer, or beyond the end, then the value is `nil`. The default for `position` is point.

### Function: **following-char**

This function returns the character following point in the current buffer. This is similar to `(char-after (point))`. However, if point is at the end of the buffer, then `following-char` returns 0.

Remember that point is always between characters, and the cursor normally appears over the character following point. Therefore, the character returned by `following-char` is the character the cursor is over.

In this example, point is between the ‘`a`’ and the ‘`c`’.

```lisp
---------- Buffer: foo ----------
Gentlemen may cry ``Pea∗ce! Peace!,''
but there is no peace.
---------- Buffer: foo ----------
```

```lisp
```

```lisp
(string (preceding-char))
     ⇒ "a"
(string (following-char))
     ⇒ "c"
```

### Function: **preceding-char**

This function returns the character preceding point in the current buffer. See above, under `following-char`, for an example. If point is at the beginning of the buffer, `preceding-char` returns 0.

### Function: **bobp**

This function returns `t` if point is at the beginning of the buffer. If narrowing is in effect, this means the beginning of the accessible portion of the text. See also `point-min` in [Point](Point.html).

### Function: **eobp**

This function returns `t` if point is at the end of the buffer. If narrowing is in effect, this means the end of accessible portion of the text. See also `point-max` in See [Point](Point.html).

### Function: **bolp**

This function returns `t` if point is at the beginning of a line. See [Text Lines](Text-Lines.html). The beginning of the buffer (or of its accessible portion) always counts as the beginning of a line.

### Function: **eolp**

This function returns `t` if point is at the end of a line. The end of the buffer (or of its accessible portion) is always considered the end of a line.
