

#### 9.4.2 Standard Symbol Properties

Here, we list the symbol properties which are used for special purposes in Emacs. In the following table, whenever we say “the named function”, that means the function whose name is the relevant symbol; similarly for “the named variable” etc.

### `:advertised-binding`

This property value specifies the preferred key binding, when showing documentation, for the named function. See [Keys in Documentation](Keys-in-Documentation.html).

`char-table-extra-slots`

The value, if non-`nil`, specifies the number of extra slots in the named char-table type. See [Char-Tables](Char_002dTables.html).

*   `customized-face`
*   `face-defface-spec`
*   `saved-face`
*   `theme-face`

These properties are used to record a face’s standard, saved, customized, and themed face specs. Do not set them directly; they are managed by `defface` and related functions. See [Defining Faces](Defining-Faces.html).

*   `customized-value`
*   `saved-value`
*   `standard-value`
*   `theme-value`

These properties are used to record a customizable variable’s standard value, saved value, customized-but-unsaved value, and themed values. Do not set them directly; they are managed by `defcustom` and related functions. See [Variable Definitions](Variable-Definitions.html).

`disabled`

If the value is non-`nil`, the named function is disabled as a command. See [Disabling Commands](Disabling-Commands.html).

`face-documentation`

The value stores the documentation string of the named face. This is set automatically by `defface`. See [Defining Faces](Defining-Faces.html).

`history-length`

The value, if non-`nil`, specifies the maximum minibuffer history length for the named history list variable. See [Minibuffer History](Minibuffer-History.html).

`interactive-form`

The value is an interactive form for the named function. Normally, you should not set this directly; use the `interactive` special form instead. See [Interactive Call](Interactive-Call.html).

`menu-enable`

The value is an expression for determining whether the named menu item should be enabled in menus. See [Simple Menu Items](Simple-Menu-Items.html).

`mode-class`

If the value is `special`, the named major mode is special. See [Major Mode Conventions](Major-Mode-Conventions.html).

`permanent-local`

If the value is non-`nil`, the named variable is a buffer-local variable whose value should not be reset when changing major modes. See [Creating Buffer-Local](Creating-Buffer_002dLocal.html).

`permanent-local-hook`

If the value is non-`nil`, the named function should not be deleted from the local value of a hook variable when changing major modes. See [Setting Hooks](Setting-Hooks.html).

`pure`

If the value is non-`nil`, the named function is considered to be pure (see [What Is a Function](What-Is-a-Function.html)). Calls with constant arguments can be evaluated at compile time. This may shift run time errors to compile time. Not to be confused with pure storage (see [Pure Storage](Pure-Storage.html)).

`risky-local-variable`

If the value is non-`nil`, the named variable is considered risky as a file-local variable. See [File Local Variables](File-Local-Variables.html).

`safe-function`

If the value is non-`nil`, the named function is considered generally safe for evaluation. See [Function Safety](Function-Safety.html).

`safe-local-eval-function`

If the value is non-`nil`, the named function is safe to call in file-local evaluation forms. See [File Local Variables](File-Local-Variables.html).

`safe-local-variable`

The value specifies a function for determining safe file-local values for the named variable. See [File Local Variables](File-Local-Variables.html).

`side-effect-free`

A non-`nil` value indicates that the named function is free of side effects (see [What Is a Function](What-Is-a-Function.html)), so the byte compiler may ignore a call whose value is unused. If the property’s value is `error-free`, the byte compiler may even delete such unused calls. In addition to byte compiler optimizations, this property is also used for determining function safety (see [Function Safety](Function-Safety.html)).

`undo-inhibit-region`

If non-`nil`, the named function prevents the `undo` operation from being restricted to the active region, if `undo` is invoked immediately after the function. See [Undo](Undo.html).

`variable-documentation`

If non-`nil`, this specifies the named variable’s documentation string. This is set automatically by `defvar` and related functions. See [Defining Faces](Defining-Faces.html).
