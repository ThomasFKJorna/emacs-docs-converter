

#### 21.2.3 Examples of Using `interactive`

Here are some examples of `interactive`:

```lisp
(defun foo1 ()              ; foo1 takes no arguments,
    (interactive)           ;   just moves forward two words.
    (forward-word 2))
     ⇒ foo1
```

```lisp
```

```lisp
(defun foo2 (n)             ; foo2 takes one argument,
    (interactive "^p")      ;   which is the numeric prefix.
                            ; under shift-select-mode,
                            ;   will activate or extend region.
    (forward-word (* 2 n)))
     ⇒ foo2
```

```lisp
```

```lisp
(defun foo3 (n)             ; foo3 takes one argument,
    (interactive "nCount:") ;   which is read with the Minibuffer.
    (forward-word (* 2 n)))
     ⇒ foo3
```

```lisp
```

```lisp
(defun three-b (b1 b2 b3)
  "Select three existing buffers.
Put them into three windows, selecting the last one."
```

```lisp
    (interactive "bBuffer1:\nbBuffer2:\nbBuffer3:")
    (delete-other-windows)
    (split-window (selected-window) 8)
    (switch-to-buffer b1)
    (other-window 1)
    (split-window (selected-window) 8)
    (switch-to-buffer b2)
    (other-window 1)
    (switch-to-buffer b3))
     ⇒ three-b
```

```lisp
(three-b "*scratch*" "declarations.texi" "*mail*")
     ⇒ nil
```
