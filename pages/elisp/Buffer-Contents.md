

### 32.2 Examining Buffer Contents

This section describes functions that allow a Lisp program to convert any portion of the text in the buffer into a string.

### Function: **buffer-substring** *start end*

This function returns a string containing a copy of the text of the region defined by positions `start` and `end` in the current buffer. If the arguments are not positions in the accessible portion of the buffer, `buffer-substring` signals an `args-out-of-range` error.

Here’s an example which assumes Font-Lock mode is not enabled:

```lisp
---------- Buffer: foo ----------
This is the contents of buffer foo

---------- Buffer: foo ----------
```

```lisp
```

```lisp
(buffer-substring 1 10)
     ⇒ "This is t"
```

```lisp
(buffer-substring (point-max) 10)
     ⇒ "he contents of buffer foo\n"
```

If the text being copied has any text properties, these are copied into the string along with the characters they belong to. See [Text Properties](Text-Properties.html). However, overlays (see [Overlays](Overlays.html)) in the buffer and their properties are ignored, not copied.

For example, if Font-Lock mode is enabled, you might get results like these:

```lisp
(buffer-substring 1 10)
     ⇒ #("This is t" 0 1 (fontified t) 1 9 (fontified t))
```

### Function: **buffer-substring-no-properties** *start end*

This is like `buffer-substring`, except that it does not copy text properties, just the characters themselves. See [Text Properties](Text-Properties.html).

### Function: **buffer-string**

This function returns the contents of the entire accessible portion of the current buffer, as a string. If the text being copied has any text properties, these are copied into the string along with the characters they belong to.

If you need to make sure the resulting string, when copied to a different location, will not change its visual appearance due to reordering of bidirectional text, use the `buffer-substring-with-bidi-context` function (see [buffer-substring-with-bidi-context](Bidirectional-Display.html)).

### Function: **filter-buffer-substring** *start end \&optional delete*

This function filters the buffer text between `start` and `end` using a function specified by the variable `filter-buffer-substring-function`, and returns the result.

The default filter function consults the obsolete wrapper hook `filter-buffer-substring-functions` (see the documentation string of the macro `with-wrapper-hook` for the details about this obsolete facility), and the obsolete variable `buffer-substring-filters`. If both of these are `nil`, it returns the unaltered text from the buffer, i.e., what `buffer-substring` would return.

If `delete` is non-`nil`, the function deletes the text between `start` and `end` after copying it, like `delete-and-extract-region`.

Lisp code should use this function instead of `buffer-substring`, `buffer-substring-no-properties`, or `delete-and-extract-region` when copying into user-accessible data structures such as the kill-ring, X clipboard, and registers. Major and minor modes can modify `filter-buffer-substring-function` to alter such text as it is copied out of the buffer.

### Variable: **filter-buffer-substring-function**

The value of this variable is a function that `filter-buffer-substring` will call to do the actual work. The function receives three arguments, the same as those of `filter-buffer-substring`, which it should treat as per the documentation of that function. It should return the filtered text (and optionally delete the source text).

The following two variables are obsoleted by `filter-buffer-substring-function`, but are still supported for backward compatibility.

### Variable: **filter-buffer-substring-functions**

This obsolete variable is a wrapper hook, whose members should be functions that accept four arguments: `fun`, `start`, `end`, and `delete`. `fun` is a function that takes three arguments (`start`, `end`, and `delete`), and returns a string. In both cases, the `start`, `end`, and `delete` arguments are the same as those of `filter-buffer-substring`.

The first hook function is passed a `fun` that is equivalent to the default operation of `filter-buffer-substring`, i.e., it returns the buffer-substring between `start` and `end` (processed by any `buffer-substring-filters`) and optionally deletes the original text from the buffer. In most cases, the hook function will call `fun` once, and then do its own processing of the result. The next hook function receives a `fun` equivalent to this, and so on. The actual return value is the result of all the hook functions acting in sequence.

### Variable: **buffer-substring-filters**

The value of this obsolete variable should be a list of functions that accept a single string argument and return another string. The default `filter-buffer-substring` function passes the buffer substring to the first function in this list, and the return value of each function is passed to the next function. The return value of the last function is passed to `filter-buffer-substring-functions`.

### Function: **current-word** *\&optional strict really-word*

This function returns the symbol (or word) at or near point, as a string. The return value includes no text properties.

If the optional argument `really-word` is non-`nil`, it finds a word; otherwise, it finds a symbol (which includes both word characters and symbol constituent characters).

If the optional argument `strict` is non-`nil`, then point must be in or next to the symbol or word—if no symbol or word is there, the function returns `nil`. Otherwise, a nearby symbol or word on the same line is acceptable.

### Function: **thing-at-point** *thing \&optional no-properties*

Return the `thing` around or next to point, as a string.

The argument `thing` is a symbol which specifies a kind of syntactic entity. Possibilities include `symbol`, `list`, `sexp`, `defun`, `filename`, `url`, `word`, `sentence`, `whitespace`, `line`, `page`, and others.

When the optional argument `no-properties` is non-`nil`, this function strips text properties from the return value.

```lisp
---------- Buffer: foo ----------
Gentlemen may cry ``Pea∗ce! Peace!,''
but there is no peace.
---------- Buffer: foo ----------

(thing-at-point 'word)
     ⇒ "Peace"
(thing-at-point 'line)
     ⇒ "Gentlemen may cry ``Peace! Peace!,''\n"
(thing-at-point 'whitespace)
     ⇒ nil
```
