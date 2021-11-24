

#### 23.2.6 Mode Hooks

Every major mode command should finish by running the mode-independent normal hook `change-major-mode-after-body-hook`, its mode hook, and the normal hook `after-change-major-mode-hook`. It does this by calling `run-mode-hooks`. If the major mode is a derived mode, that is if it calls another major mode (the parent mode) in its body, it should do this inside `delay-mode-hooks` so that the parent won’t run these hooks itself. Instead, the derived mode’s call to `run-mode-hooks` runs the parent’s mode hook too. See [Major Mode Conventions](Major-Mode-Conventions.html).

Emacs versions before Emacs 22 did not have `delay-mode-hooks`. Versions before 24 did not have `change-major-mode-after-body-hook`. When user-implemented major modes do not use `run-mode-hooks` and have not been updated to use these newer features, they won’t entirely follow these conventions: they may run the parent’s mode hook too early, or fail to run `after-change-major-mode-hook`. If you encounter such a major mode, please correct it to follow these conventions.

When you define a major mode using `define-derived-mode`, it automatically makes sure these conventions are followed. If you define a major mode “by hand”, not using `define-derived-mode`, use the following functions to handle these conventions automatically.

### Function: **run-mode-hooks** *\&rest hookvars*

Major modes should run their mode hook using this function. It is similar to `run-hooks` (see [Hooks](Hooks.html)), but it also runs `change-major-mode-after-body-hook`, `hack-local-variables` (when the buffer is visiting a file) (see [File Local Variables](File-Local-Variables.html)), and `after-change-major-mode-hook`. The last thing it does is to evaluate any `:after-hook` forms declared by parent modes (see [Derived Modes](Derived-Modes.html)).

When this function is called during the execution of a `delay-mode-hooks` form, it does not run the hooks or `hack-local-variables` or evaluate the forms immediately. Instead, it arranges for the next call to `run-mode-hooks` to run them.

### Macro: **delay-mode-hooks** *body…*

When one major mode command calls another, it should do so inside of `delay-mode-hooks`.

This macro executes `body`, but tells all `run-mode-hooks` calls during the execution of `body` to delay running their hooks. The hooks will actually run during the next call to `run-mode-hooks` after the end of the `delay-mode-hooks` construct.

### Variable: **change-major-mode-after-body-hook**

This is a normal hook run by `run-mode-hooks`. It is run before the mode hooks.

### Variable: **after-change-major-mode-hook**

This is a normal hook run by `run-mode-hooks`. It is run at the very end of every properly-written major mode command.
