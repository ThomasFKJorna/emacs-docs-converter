

#### 23.3.3 Defining Minor Modes

The macro `define-minor-mode` offers a convenient way of implementing a mode in one self-contained definition.

### Macro: **define-minor-mode** *mode doc \[init-value \[lighter \[keymap]]] keyword-args… body…*

This macro defines a new minor mode whose name is `mode` (a symbol). It defines a command named `mode` to toggle the minor mode, with `doc` as its documentation string.

The toggle command takes one optional (prefix) argument. If called interactively with no argument it toggles the mode on or off. A positive prefix argument enables the mode, any other prefix argument disables it. From Lisp, an argument of `toggle` toggles the mode, whereas an omitted or `nil` argument enables the mode. This makes it easy to enable the minor mode in a major mode hook, for example. If `doc` is `nil`, the macro supplies a default documentation string explaining the above.

By default, it also defines a variable named `mode`, which is set to `t` or `nil` by enabling or disabling the mode. The variable is initialized to `init-value`. Except in unusual circumstances (see below), this value must be `nil`.

The string `lighter` says what to display in the mode line when the mode is enabled; if it is `nil`, the mode is not displayed in the mode line.

The optional argument `keymap` specifies the keymap for the minor mode. If non-`nil`, it should be a variable name (whose value is a keymap), a keymap, or an alist of the form

```lisp
(key-sequence . definition)
```

where each `key-sequence` and `definition` are arguments suitable for passing to `define-key` (see [Changing Key Bindings](Changing-Key-Bindings.html)). If `keymap` is a keymap or an alist, this also defines the variable `mode-map`.

The above three arguments `init-value`, `lighter`, and `keymap` can be (partially) omitted when `keyword-args` are used. The `keyword-args` consist of keywords followed by corresponding values. A few keywords have special meanings:

*   `:group group`

    Custom group name to use in all generated `defcustom` forms. Defaults to `mode` without the possible trailing ‘`-mode`’. **Warning:** don’t use this default group name unless you have written a `defgroup` to define that group properly. See [Group Definitions](Group-Definitions.html).

*   `:global global`

    If non-`nil`, this specifies that the minor mode should be global rather than buffer-local. It defaults to `nil`.

    One of the effects of making a minor mode global is that the `mode` variable becomes a customization variable. Toggling it through the Customize interface turns the mode on and off, and its value can be saved for future Emacs sessions (see [Saving Customizations](https://www.gnu.org/software/emacs/manual/html_node/emacs/Saving-Customizations.html#Saving-Customizations) in The GNU Emacs Manual. For the saved variable to work, you should ensure that the `define-minor-mode` form is evaluated each time Emacs starts; for packages that are not part of Emacs, the easiest way to do this is to specify a `:require` keyword.

*   `:init-value init-value`

    This is equivalent to specifying `init-value` positionally.

*   `:lighter lighter`

    This is equivalent to specifying `lighter` positionally.

*   `:keymap keymap`

    This is equivalent to specifying `keymap` positionally.

*   `:variable place`

    This replaces the default variable `mode`, used to store the state of the mode. If you specify this, the `mode` variable is not defined, and any `init-value` argument is unused. `place` can be a different named variable (which you must define yourself), or anything that can be used with the `setf` function (see [Generalized Variables](Generalized-Variables.html)). `place` can also be a cons `(get . set)`, where `get` is an expression that returns the current state, and `set` is a function of one argument (a state) that sets it.

*   `:after-hook after-hook`

    This defines a single Lisp form which is evaluated after the mode hooks have run. It should not be quoted.

Any other keyword arguments are passed directly to the `defcustom` generated for the variable `mode`.

The command named `mode` first performs the standard actions such as setting the variable named `mode` and then executes the `body` forms, if any. It then runs the mode hook variable `mode-hook` and finishes by evaluating any form in `:after-hook`.

The initial value must be `nil` except in cases where (1) the mode is preloaded in Emacs, or (2) it is painless for loading to enable the mode even though the user did not request it. For instance, if the mode has no effect unless something else is enabled, and will always be loaded by that time, enabling it by default is harmless. But these are unusual circumstances. Normally, the initial value must be `nil`.

The name `easy-mmode-define-minor-mode` is an alias for this macro.

Here is an example of using `define-minor-mode`:

```lisp
(define-minor-mode hungry-mode
  "Toggle Hungry mode.
Interactively with no argument, this command toggles the mode.
A positive prefix argument enables the mode, any other prefix
argument disables it.  From Lisp, argument omitted or nil enables
the mode, `toggle' toggles the state.

When Hungry mode is enabled, the control delete key
gobbles all preceding whitespace except the last.
See the command \\[hungry-electric-delete]."
 ;; The initial value.
 nil
 ;; The indicator for the mode line.
 " Hungry"
 ;; The minor mode bindings.
 '(([C-backspace] . hungry-electric-delete)))
```

This defines a minor mode named “Hungry mode”, a command named `hungry-mode` to toggle it, a variable named `hungry-mode` which indicates whether the mode is enabled, and a variable named `hungry-mode-map` which holds the keymap that is active when the mode is enabled. It initializes the keymap with a key binding for `C-DEL`. There are no `body` forms—many minor modes don’t need any.

Here’s an equivalent way to write it:

```lisp
(define-minor-mode hungry-mode
  "Toggle Hungry mode.
...rest of documentation as before..."
 ;; The initial value.
 :init-value nil
 ;; The indicator for the mode line.
 :lighter " Hungry"
 ;; The minor mode bindings.
 :keymap
 '(([C-backspace] . hungry-electric-delete)
   ([C-M-backspace]
    . (lambda ()
        (interactive)
        (hungry-electric-delete t)))))
```

### Macro: **define-globalized-minor-mode** *global-mode mode turn-on keyword-args… body…*

This defines a global toggle named `global-mode` whose meaning is to enable or disable the buffer-local minor mode `mode` in all buffers. It also executes the `body` forms. To turn on the minor mode in a buffer, it uses the function `turn-on`; to turn off the minor mode, it calls `mode` with -1 as argument.

Globally enabling the mode also affects buffers subsequently created by visiting files, and buffers that use a major mode other than Fundamental mode; but it does not detect the creation of a new buffer in Fundamental mode.

This defines the customization option `global-mode` (see [Customization](Customization.html)), which can be toggled in the Customize interface to turn the minor mode on and off. As with `define-minor-mode`, you should ensure that the `define-globalized-minor-mode` form is evaluated each time Emacs starts, for example by providing a `:require` keyword.

Use `:group group` in `keyword-args` to specify the custom group for the mode variable of the global minor mode.

Generally speaking, when you define a globalized minor mode, you should also define a non-globalized version, so that people can use (or disable) it in individual buffers. This also allows them to disable a globally enabled minor mode in a specific major mode, by using that mode’s hook.
