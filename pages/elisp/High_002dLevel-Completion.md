

#### 20.6.4 High-Level Completion Functions

This section describes the higher-level convenience functions for reading certain sorts of names with completion.

In most cases, you should not call these functions in the middle of a Lisp function. When possible, do all minibuffer input as part of reading the arguments for a command, in the `interactive` specification. See [Defining Commands](Defining-Commands.html).

### Function: **read-buffer** *prompt \&optional default require-match predicate*

This function reads the name of a buffer and returns it as a string. It prompts with `prompt`. The argument `default` is the default name to use, the value to return if the user exits with an empty minibuffer. If non-`nil`, it should be a string, a list of strings, or a buffer. If it is a list, the default value is the first element of this list. It is mentioned in the prompt, but is not inserted in the minibuffer as initial input.

The argument `prompt` should be a string ending with a colon and a space. If `default` is non-`nil`, the function inserts it in `prompt` before the colon to follow the convention for reading from the minibuffer with a default value (see [Programming Tips](Programming-Tips.html)).

The optional argument `require-match` has the same meaning as in `completing-read`. See [Minibuffer Completion](Minibuffer-Completion.html).

The optional argument `predicate`, if non-`nil`, specifies a function to filter the buffers that should be considered: the function will be called with every potential candidate as its argument, and should return `nil` to reject the candidate, non-`nil` to accept it.

In the following example, the user enters ‘`minibuffer.t`’, and then types `RET`. The argument `require-match` is `t`, and the only buffer name starting with the given input is ‘`minibuffer.texi`’, so that name is the value.

```lisp
(read-buffer "Buffer name: " "foo" t)
```

```lisp
;; After evaluation of the preceding expression,
;;   the following prompt appears,
;;   with an empty minibuffer:
```

```lisp
```

```lisp
---------- Buffer: Minibuffer ----------
Buffer name (default foo): ∗
---------- Buffer: Minibuffer ----------
```

```lisp
```

```lisp
;; The user types minibuffer.t RET.
     ⇒ "minibuffer.texi"
```

### User Option: **read-buffer-function**

This variable, if non-`nil`, specifies a function for reading buffer names. `read-buffer` calls this function instead of doing its usual work, with the same arguments passed to `read-buffer`.

### User Option: **read-buffer-completion-ignore-case**

If this variable is non-`nil`, `read-buffer` ignores case when performing completion while reading the buffer name.

### Function: **read-command** *prompt \&optional default*

This function reads the name of a command and returns it as a Lisp symbol. The argument `prompt` is used as in `read-from-minibuffer`. Recall that a command is anything for which `commandp` returns `t`, and a command name is a symbol for which `commandp` returns `t`. See [Interactive Call](Interactive-Call.html).

The argument `default` specifies what to return if the user enters null input. It can be a symbol, a string or a list of strings. If it is a string, `read-command` interns it before returning it. If it is a list, `read-command` interns the first element of this list. If `default` is `nil`, that means no default has been specified; then if the user enters null input, the return value is `(intern "")`, that is, a symbol whose name is an empty string, and whose printed representation is `##` (see [Symbol Type](Symbol-Type.html)).

```lisp
(read-command "Command name? ")
```

```lisp
;; After evaluation of the preceding expression,
;;   the following prompt appears with an empty minibuffer:
```

```lisp
```

```lisp
---------- Buffer: Minibuffer ----------
Command name?
---------- Buffer: Minibuffer ----------
```

If the user types `forward-c RET`, then this function returns `forward-char`.

The `read-command` function is a simplified interface to `completing-read`. It uses the variable `obarray` so as to complete in the set of extant Lisp symbols, and it uses the `commandp` predicate so as to accept only command names:

```lisp
(read-command prompt)
≡
(intern (completing-read prompt obarray
                         'commandp t nil))
```

### Function: **read-variable** *prompt \&optional default*

This function reads the name of a customizable variable and returns it as a symbol. Its arguments have the same form as those of `read-command`. It behaves just like `read-command`, except that it uses the predicate `custom-variable-p` instead of `commandp`.

### Command: **read-color** *\&optional prompt convert allow-empty display*

This function reads a string that is a color specification, either the color’s name or an RGB hex value such as `#RRRGGGBBB`. It prompts with `prompt` (default: `"Color (name or #RGB triplet):"`) and provides completion for color names, but not for hex RGB values. In addition to names of standard colors, completion candidates include the foreground and background colors at point.

Valid RGB values are described in [Color Names](Color-Names.html).

The function’s return value is the string typed by the user in the minibuffer. However, when called interactively or if the optional argument `convert` is non-`nil`, it converts any input color name into the corresponding RGB value string and instead returns that. This function requires a valid color specification to be input. Empty color names are allowed when `allow-empty` is non-`nil` and the user enters null input.

Interactively, or when `display` is non-`nil`, the return value is also displayed in the echo area.

See also the functions `read-coding-system` and `read-non-nil-coding-system`, in [User-Chosen Coding Systems](User_002dChosen-Coding-Systems.html), and `read-input-method-name`, in [Input Methods](Input-Methods.html).
