

#### 1.3.5 Error Messages

Some examples signal errors. This normally displays an error message in the echo area. We show the error message on a line starting with ‘`error→`’. Note that ‘`error→`’ itself does not appear in the echo area.

```lisp
(+ 23 'x)
error→ Wrong type argument: number-or-marker-p, x
```
