

#### 12.11.2 Creating and Deleting Buffer-Local Bindings

### Command: **make-local-variable** *variable*

This function creates a buffer-local binding in the current buffer for `variable` (a symbol). Other buffers are not affected. The value returned is `variable`.

The buffer-local value of `variable` starts out as the same value `variable` previously had. If `variable` was void, it remains void.

```lisp
;; In buffer ‘b1’:
(setq foo 5)                ; Affects all buffers.
     ⇒ 5
```

```lisp
(make-local-variable 'foo)  ; Now it is local in ‘b1’.
     ⇒ foo
```

```lisp
foo                         ; That did not change
     ⇒ 5                   ;   the value.
```

```lisp
(setq foo 6)                ; Change the value
     ⇒ 6                   ;   in ‘b1’.
```

```lisp
foo
     ⇒ 6
```

```lisp
```

```lisp
;; In buffer ‘b2’, the value hasn’t changed.
(with-current-buffer "b2"
  foo)
     ⇒ 5
```

Making a variable buffer-local within a `let`-binding for that variable does not work reliably, unless the buffer in which you do this is not current either on entry to or exit from the `let`. This is because `let` does not distinguish between different kinds of bindings; it knows only which variable the binding was made for.

It is an error to make a constant or a read-only variable buffer-local. See [Constant Variables](Constant-Variables.html).

If the variable is terminal-local (see [Multiple Terminals](Multiple-Terminals.html)), this function signals an error. Such variables cannot have buffer-local bindings as well.

**Warning:** do not use `make-local-variable` for a hook variable. The hook variables are automatically made buffer-local as needed if you use the `local` argument to `add-hook` or `remove-hook`.

### Macro: **setq-local** *\&rest pairs*

`pairs` is a list of variable and value pairs. This macro creates a buffer-local binding in the current buffer for each of the variables, and gives them a buffer-local value. It is equivalent to calling `make-local-variable` followed by `setq` for each of the variables. The variables should be unquoted symbols.

```lisp
(setq-local var1 "value1"
            var2 "value2")
```

### Command: **make-variable-buffer-local** *variable*

This function marks `variable` (a symbol) automatically buffer-local, so that any subsequent attempt to set it will make it local to the current buffer at the time. Unlike `make-local-variable`, with which it is often confused, this cannot be undone, and affects the behavior of the variable in all buffers.

A peculiar wrinkle of this feature is that binding the variable (with `let` or other binding constructs) does not create a buffer-local binding for it. Only setting the variable (with `set` or `setq`), while the variable does not have a `let`-style binding that was made in the current buffer, does so.

If `variable` does not have a default value, then calling this command will give it a default value of `nil`. If `variable` already has a default value, that value remains unchanged. Subsequently calling `makunbound` on `variable` will result in a void buffer-local value and leave the default value unaffected.

The value returned is `variable`.

It is an error to make a constant or a read-only variable buffer-local. See [Constant Variables](Constant-Variables.html).

**Warning:** Don’t assume that you should use `make-variable-buffer-local` for user-option variables, simply because users *might* want to customize them differently in different buffers. Users can make any variable local, when they wish to. It is better to leave the choice to them.

The time to use `make-variable-buffer-local` is when it is crucial that no two buffers ever share the same binding. For example, when a variable is used for internal purposes in a Lisp program which depends on having separate values in separate buffers, then using `make-variable-buffer-local` can be the best solution.

### Macro: **defvar-local** *variable value \&optional docstring*

This macro defines `variable` as a variable with initial value `value` and `docstring`, and marks it as automatically buffer-local. It is equivalent to calling `defvar` followed by `make-variable-buffer-local`. `variable` should be an unquoted symbol.

### Function: **local-variable-p** *variable \&optional buffer*

This returns `t` if `variable` is buffer-local in buffer `buffer` (which defaults to the current buffer); otherwise, `nil`.

### Function: **local-variable-if-set-p** *variable \&optional buffer*

This returns `t` if `variable` either has a buffer-local value in buffer `buffer`, or is automatically buffer-local. Otherwise, it returns `nil`. If omitted or `nil`, `buffer` defaults to the current buffer.

### Function: **buffer-local-value** *variable buffer*

This function returns the buffer-local binding of `variable` (a symbol) in buffer `buffer`. If `variable` does not have a buffer-local binding in buffer `buffer`, it returns the default value (see [Default Value](Default-Value.html)) of `variable` instead.

### Function: **buffer-local-variables** *\&optional buffer*

This function returns a list describing the buffer-local variables in buffer `buffer`. (If `buffer` is omitted, the current buffer is used.) Normally, each list element has the form `(sym . val)`, where `sym` is a buffer-local variable (a symbol) and `val` is its buffer-local value. But when a variable’s buffer-local binding in `buffer` is void, its list element is just `sym`.

```lisp
(make-local-variable 'foobar)
(makunbound 'foobar)
(make-local-variable 'bind-me)
(setq bind-me 69)
```

```lisp
(setq lcl (buffer-local-variables))
    ;; First, built-in variables local in all buffers:
⇒ ((mark-active . nil)
    (buffer-undo-list . nil)
    (mode-name . "Fundamental")
    …
```

```lisp
    ;; Next, non-built-in buffer-local variables.
    ;; This one is buffer-local and void:
    foobar
    ;; This one is buffer-local and nonvoid:
    (bind-me . 69))
```

Note that storing new values into the CDRs of cons cells in this list does *not* change the buffer-local values of the variables.

### Command: **kill-local-variable** *variable*

This function deletes the buffer-local binding (if any) for `variable` (a symbol) in the current buffer. As a result, the default binding of `variable` becomes visible in this buffer. This typically results in a change in the value of `variable`, since the default value is usually different from the buffer-local value just eliminated.

If you kill the buffer-local binding of a variable that automatically becomes buffer-local when set, this makes the default value visible in the current buffer. However, if you set the variable again, that will once again create a buffer-local binding for it.

`kill-local-variable` returns `variable`.

This function is a command because it is sometimes useful to kill one buffer-local variable interactively, just as it is useful to create buffer-local variables interactively.

### Function: **kill-all-local-variables**

This function eliminates all the buffer-local variable bindings of the current buffer except for variables marked as permanent and local hook functions that have a non-`nil` `permanent-local-hook` property (see [Setting Hooks](Setting-Hooks.html)). As a result, the buffer will see the default values of most variables.

This function also resets certain other information pertaining to the buffer: it sets the local keymap to `nil`, the syntax table to the value of `(standard-syntax-table)`, the case table to `(standard-case-table)`, and the abbrev table to the value of `fundamental-mode-abbrev-table`.

The very first thing this function does is run the normal hook `change-major-mode-hook` (see below).

Every major mode command begins by calling this function, which has the effect of switching to Fundamental mode and erasing most of the effects of the previous major mode. To ensure that this does its job, the variables that major modes set should not be marked permanent.

`kill-all-local-variables` returns `nil`.

### Variable: **change-major-mode-hook**

The function `kill-all-local-variables` runs this normal hook before it does anything else. This gives major modes a way to arrange for something special to be done if the user switches to a different major mode. It is also useful for buffer-specific minor modes that should be forgotten if the user changes the major mode.

For best results, make this variable buffer-local, so that it will disappear after doing its job and will not interfere with the subsequent major mode. See [Hooks](Hooks.html).

A buffer-local variable is *permanent* if the variable name (a symbol) has a `permanent-local` property that is non-`nil`. Such variables are unaffected by `kill-all-local-variables`, and their local bindings are therefore not cleared by changing major modes. Permanent locals are appropriate for data pertaining to where the file came from or how to save it, rather than with how to edit the contents.
