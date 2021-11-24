

### 21.3 Interactive Call

After the command loop has translated a key sequence into a command, it invokes that command using the function `command-execute`. If the command is a function, `command-execute` calls `call-interactively`, which reads the arguments and calls the command. You can also call these functions yourself.

Note that the term “command”, in this context, refers to an interactively callable function (or function-like object), or a keyboard macro. It does not refer to the key sequence used to invoke a command (see [Keymaps](Keymaps.html)).

### Function: **commandp** *object \&optional for-call-interactively*

This function returns `t` if `object` is a command. Otherwise, it returns `nil`.

Commands include strings and vectors (which are treated as keyboard macros), lambda expressions that contain a top-level `interactive` form (see [Using Interactive](Using-Interactive.html)), byte-code function objects made from such lambda expressions, autoload objects that are declared as interactive (non-`nil` fourth argument to `autoload`), and some primitive functions. Also, a symbol is considered a command if it has a non-`nil` `interactive-form` property, or if its function definition satisfies `commandp`.

If `for-call-interactively` is non-`nil`, then `commandp` returns `t` only for objects that `call-interactively` could call—thus, not for keyboard macros.

See `documentation` in [Accessing Documentation](Accessing-Documentation.html), for a realistic example of using `commandp`.

### Function: **call-interactively** *command \&optional record-flag keys*

This function calls the interactively callable function `command`, providing arguments according to its interactive calling specifications. It returns whatever `command` returns.

If, for instance, you have a function with the following signature:

```lisp
(defun foo (begin end)
  (interactive "r")
  ...)
```

then saying

```lisp
(call-interactively 'foo)
```

will call `foo` with the region (`point` and `mark`) as the arguments.

An error is signaled if `command` is not a function or if it cannot be called interactively (i.e., is not a command). Note that keyboard macros (strings and vectors) are not accepted, even though they are considered commands, because they are not functions. If `command` is a symbol, then `call-interactively` uses its function definition.

If `record-flag` is non-`nil`, then this command and its arguments are unconditionally added to the list `command-history`. Otherwise, the command is added only if it uses the minibuffer to read an argument. See [Command History](Command-History.html).

The argument `keys`, if given, should be a vector which specifies the sequence of events to supply if the command inquires which events were used to invoke it. If `keys` is omitted or `nil`, the default is the return value of `this-command-keys-vector`. See [Definition of this-command-keys-vector](Command-Loop-Info.html#Definition-of-this_002dcommand_002dkeys_002dvector).

### Function: **funcall-interactively** *function \&rest arguments*

This function works like `funcall` (see [Calling Functions](Calling-Functions.html)), but it makes the call look like an interactive invocation: a call to `called-interactively-p` inside `function` will return `t`. If `function` is not a command, it is called without signaling an error.

### Function: **command-execute** *command \&optional record-flag keys special*

This function executes `command`. The argument `command` must satisfy the `commandp` predicate; i.e., it must be an interactively callable function or a keyboard macro.

A string or vector as `command` is executed with `execute-kbd-macro`. A function is passed to `call-interactively` (see above), along with the `record-flag` and `keys` arguments.

If `command` is a symbol, its function definition is used in its place. A symbol with an `autoload` definition counts as a command if it was declared to stand for an interactively callable function. Such a definition is handled by loading the specified library and then rechecking the definition of the symbol.

The argument `special`, if given, means to ignore the prefix argument and not clear it. This is used for executing special events (see [Special Events](Special-Events.html)).

### Command: **execute-extended-command** *prefix-argument*

This function reads a command name from the minibuffer using `completing-read` (see [Completion](Completion.html)). Then it uses `command-execute` to call the specified command. Whatever that command returns becomes the value of `execute-extended-command`.

If the command asks for a prefix argument, it receives the value `prefix-argument`. If `execute-extended-command` is called interactively, the current raw prefix argument is used for `prefix-argument`, and thus passed on to whatever command is run.

`execute-extended-command` is the normal definition of `M-x`, so it uses the string ‘`M-x `’ as a prompt. (It would be better to take the prompt from the events used to invoke `execute-extended-command`, but that is painful to implement.) A description of the value of the prefix argument, if any, also becomes part of the prompt.

```lisp
(execute-extended-command 3)
---------- Buffer: Minibuffer ----------
3 M-x forward-word RET
---------- Buffer: Minibuffer ----------
     ⇒ t
```
