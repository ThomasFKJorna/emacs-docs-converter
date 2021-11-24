

### 20.3 Reading Lisp Objects with the Minibuffer

This section describes functions for reading Lisp objects with the minibuffer.

### Function: **read-minibuffer** *prompt \&optional initial*

This function reads a Lisp object using the minibuffer, and returns it without evaluating it. The arguments `prompt` and `initial` are used as in `read-from-minibuffer`.

This is a simplified interface to the `read-from-minibuffer` function:

```lisp
(read-minibuffer prompt initial)
≡
(let (minibuffer-allow-text-properties)
  (read-from-minibuffer prompt initial nil t))
```

Here is an example in which we supply the string `"(testing)"` as initial input:

```lisp
(read-minibuffer
 "Enter an expression: " (format "%s" '(testing)))

;; Here is how the minibuffer is displayed:
```

```lisp
```

```lisp
---------- Buffer: Minibuffer ----------
Enter an expression: (testing)∗
---------- Buffer: Minibuffer ----------
```

The user can type `RET` immediately to use the initial input as a default, or can edit the input.

### Function: **eval-minibuffer** *prompt \&optional initial*

This function reads a Lisp expression using the minibuffer, evaluates it, then returns the result. The arguments `prompt` and `initial` are used as in `read-from-minibuffer`.

This function simply evaluates the result of a call to `read-minibuffer`:

```lisp
(eval-minibuffer prompt initial)
≡
(eval (read-minibuffer prompt initial))
```

### Function: **edit-and-eval-command** *prompt form*

This function reads a Lisp expression in the minibuffer, evaluates it, then returns the result. The difference between this command and `eval-minibuffer` is that here the initial `form` is not optional and it is treated as a Lisp object to be converted to printed representation rather than as a string of text. It is printed with `prin1`, so if it is a string, double-quote characters (‘`"`’) appear in the initial text. See [Output Functions](Output-Functions.html).

In the following example, we offer the user an expression with initial text that is already a valid form:

```lisp
(edit-and-eval-command "Please edit: " '(forward-word 1))

;; After evaluation of the preceding expression,
;;   the following appears in the minibuffer:
```

```lisp
```

```lisp
---------- Buffer: Minibuffer ----------
Please edit: (forward-word 1)∗
---------- Buffer: Minibuffer ----------
```

Typing `RET` right away would exit the minibuffer and evaluate the expression, thus moving point forward one word.
