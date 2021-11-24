

### 17.1 Performance of Byte-Compiled Code

A byte-compiled function is not as efficient as a primitive function written in C, but runs much faster than the version written in Lisp. Here is an example:

```lisp
(defun silly-loop (n)
  "Return the time, in seconds, to run N iterations of a loop."
  (let ((t1 (float-time)))
    (while (> (setq n (1- n)) 0))
    (- (float-time) t1)))
⇒ silly-loop
```

```lisp
```

```lisp
(silly-loop 50000000)
⇒ 10.235304117202759
```

```lisp
```

```lisp
(byte-compile 'silly-loop)
⇒ [Compiled code not shown]
```

```lisp
```

```lisp
(silly-loop 50000000)
⇒ 3.705854892730713
```

In this example, the interpreted code required 10 seconds to run, whereas the byte-compiled code required less than 4 seconds. These results are representative, but actual results may vary.
