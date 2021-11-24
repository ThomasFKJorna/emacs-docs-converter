

#### 23.2.4 Defining Derived Modes

The recommended way to define a new major mode is to derive it from an existing one using `define-derived-mode`. If there is no closely related mode, you should inherit from either `text-mode`, `special-mode`, or `prog-mode`. See [Basic Major Modes](Basic-Major-Modes.html). If none of these are suitable, you can inherit from `fundamental-mode` (see [Major Modes](Major-Modes.html)).

### Macro: **define-derived-mode** *variant parent name docstring keyword-args… body…*

This macro defines `variant` as a major mode command, using `name` as the string form of the mode name. `variant` and `parent` should be unquoted symbols.

The new command `variant` is defined to call the function `parent`, then override certain aspects of that parent mode:

*   The new mode has its own sparse keymap, named `variant-map`. `define-derived-mode` makes the parent mode’s keymap the parent of the new map, unless `variant-map` is already set and already has a parent.
*   The new mode has its own syntax table, kept in the variable `variant-syntax-table`, unless you override this using the `:syntax-table` keyword (see below). `define-derived-mode` makes the parent mode’s syntax-table the parent of `variant-syntax-table`, unless the latter is already set and already has a parent different from the standard syntax table.
*   The new mode has its own abbrev table, kept in the variable `variant-abbrev-table`, unless you override this using the `:abbrev-table` keyword (see below).
*   The new mode has its own mode hook, `variant-hook`. It runs this hook, after running the hooks of its ancestor modes, with `run-mode-hooks`, as the last thing it does, apart from running any `:after-hook` form it may have. See [Mode Hooks](Mode-Hooks.html).

In addition, you can specify how to override other aspects of `parent` with `body`. The command `variant` evaluates the forms in `body` after setting up all its usual overrides, just before running the mode hooks.

If `parent` has a non-`nil` `mode-class` symbol property, then `define-derived-mode` sets the `mode-class` property of `variant` to the same value. This ensures, for example, that if `parent` is a special mode, then `variant` is also a special mode (see [Major Mode Conventions](Major-Mode-Conventions.html)).

You can also specify `nil` for `parent`. This gives the new mode no parent. Then `define-derived-mode` behaves as described above, but, of course, omits all actions connected with `parent`.

The argument `docstring` specifies the documentation string for the new mode. `define-derived-mode` adds some general information about the mode’s hook, followed by the mode’s keymap, at the end of this documentation string. If you omit `docstring`, `define-derived-mode` generates a documentation string.

The `keyword-args` are pairs of keywords and values. The values, except for `:after-hook`’s, are evaluated. The following keywords are currently supported:

*   `:syntax-table`

    You can use this to explicitly specify a syntax table for the new mode. If you specify a `nil` value, the new mode uses the same syntax table as `parent`, or the standard syntax table if `parent` is `nil`. (Note that this does *not* follow the convention used for non-keyword arguments that a `nil` value is equivalent with not specifying the argument.)

*   `:abbrev-table`

    You can use this to explicitly specify an abbrev table for the new mode. If you specify a `nil` value, the new mode uses the same abbrev table as `parent`, or `fundamental-mode-abbrev-table` if `parent` is `nil`. (Again, a `nil` value is *not* equivalent to not specifying this keyword.)

*   `:group`

    If this is specified, the value should be the customization group for this mode. (Not all major modes have one.) The command `customize-mode` uses this. `define-derived-mode` does *not* automatically define the specified customization group.

*   `:after-hook`

    This optional keyword specifies a single Lisp form to evaluate as the final act of the mode function, after the mode hooks have been run. It should not be quoted. Since the form might be evaluated after the mode function has terminated, it should not access any element of the mode function’s local state. An `:after-hook` form is useful for setting up aspects of the mode which depend on the user’s settings, which in turn may have been changed in a mode hook.

Here is a hypothetical example:

```lisp
(defvar hypertext-mode-map
  (let ((map (make-sparse-keymap)))
    (define-key map [down-mouse-3] 'do-hyper-link)
    map))

(define-derived-mode hypertext-mode
  text-mode "Hypertext"
  "Major mode for hypertext."
  (setq-local case-fold-search nil))
```

Do not write an `interactive` spec in the definition; `define-derived-mode` does that automatically.

### Function: **derived-mode-p** *\&rest modes*

This function returns non-`nil` if the current major mode is derived from any of the major modes given by the symbols `modes`.
