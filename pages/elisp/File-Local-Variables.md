

### 12.12 File Local Variables

A file can specify local variable values; Emacs uses these to create buffer-local bindings for those variables in the buffer visiting that file. See [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_node/emacs/File-Variables.html#File-Variables) in The GNU Emacs Manual, for basic information about file-local variables. This section describes the functions and variables that affect how file-local variables are processed.

If a file-local variable could specify an arbitrary function or Lisp expression that would be called later, visiting a file could take over your Emacs. Emacs protects against this by automatically setting only those file-local variables whose specified values are known to be safe. Other file-local variables are set only if the user agrees.

For additional safety, `read-circle` is temporarily bound to `nil` when Emacs reads file-local variables (see [Input Functions](Input-Functions.html)). This prevents the Lisp reader from recognizing circular and shared Lisp structures (see [Circular Objects](Circular-Objects.html)).

### User Option: **enable-local-variables**

This variable controls whether to process file-local variables. The possible values are:

*   `t` (the default)

    Set the safe variables, and query (once) about any unsafe variables.

*   `:safe`

    Set only the safe variables and do not query.

*   `:all`

    Set all the variables and do not query.

*   `nil`

    Don’t set any variables.

*   anything else

    Query (once) about all the variables.

### Variable: **inhibit-local-variables-regexps**

This is a list of regular expressions. If a file has a name matching an element of this list, then it is not scanned for any form of file-local variable. For examples of why you might want to use this, see [Auto Major Mode](Auto-Major-Mode.html).

### Function: **hack-local-variables** *\&optional handle-mode*

This function parses, and binds or evaluates as appropriate, any local variables specified by the contents of the current buffer. The variable `enable-local-variables` has its effect here. However, this function does not look for the ‘`mode:`’ local variable in the ‘`-*-`’ line. `set-auto-mode` does that, also taking `enable-local-variables` into account (see [Auto Major Mode](Auto-Major-Mode.html)).

This function works by walking the alist stored in `file-local-variables-alist` and applying each local variable in turn. It calls `before-hack-local-variables-hook` and `hack-local-variables-hook` before and after applying the variables, respectively. It only calls the before-hook if the alist is non-`nil`; it always calls the other hook. This function ignores a ‘`mode`’ element if it specifies the same major mode as the buffer already has.

If the optional argument `handle-mode` is `t`, then all this function does is return a symbol specifying the major mode, if the ‘`-*-`’ line or the local variables list specifies one, and `nil` otherwise. It does not set the mode or any other file-local variable. If `handle-mode` has any value other than `nil` or `t`, any settings of ‘`mode`’ in the ‘`-*-`’ line or the local variables list are ignored, and the other settings are applied. If `handle-mode` is `nil`, all the file local variables are set.

### Variable: **file-local-variables-alist**

This buffer-local variable holds the alist of file-local variable settings. Each element of the alist is of the form `(var . value)`, where `var` is a symbol of the local variable and `value` is its value. When Emacs visits a file, it first collects all the file-local variables into this alist, and then the `hack-local-variables` function applies them one by one.

### Variable: **before-hack-local-variables-hook**

Emacs calls this hook immediately before applying file-local variables stored in `file-local-variables-alist`.

### Variable: **hack-local-variables-hook**

Emacs calls this hook immediately after it finishes applying file-local variables stored in `file-local-variables-alist`.

You can specify safe values for a variable with a `safe-local-variable` property. The property has to be a function of one argument; any value is safe if the function returns non-`nil` given that value. Many commonly-encountered file variables have `safe-local-variable` properties; these include `fill-column`, `fill-prefix`, and `indent-tabs-mode`. For boolean-valued variables that are safe, use `booleanp` as the property value.

If you want to define `safe-local-variable` properties for variables defined in C source code, add the names and the properties of those variables to the list in the “Safe local variables” section of `files.el`.

When defining a user option using `defcustom`, you can set its `safe-local-variable` property by adding the arguments `:safe function` to `defcustom` (see [Variable Definitions](Variable-Definitions.html)). However, a safety predicate defined using `:safe` will only be known once the package that contains the `defcustom` is loaded, which is often too late. As an alternative, you can use the autoload cookie (see [Autoload](Autoload.html)) to assign the option its safety predicate, like this:

```lisp
;;;###autoload (put 'var 'safe-local-variable 'pred)
```

The safe value definitions specified with `autoload` are copied into the package’s autoloads file (`loaddefs.el` for most packages bundled with Emacs), and are known to Emacs since the beginning of a session.

### User Option: **safe-local-variable-values**

This variable provides another way to mark some variable values as safe. It is a list of cons cells `(var . val)`, where `var` is a variable name and `val` is a value which is safe for that variable.

When Emacs asks the user whether or not to obey a set of file-local variable specifications, the user can choose to mark them as safe. Doing so adds those variable/value pairs to `safe-local-variable-values`, and saves it to the user’s custom file.

### Function: **safe-local-variable-p** *sym val*

This function returns non-`nil` if it is safe to give `sym` the value `val`, based on the above criteria.

Some variables are considered *risky*. If a variable is risky, it is never entered automatically into `safe-local-variable-values`; Emacs always queries before setting a risky variable, unless the user explicitly allows a value by customizing `safe-local-variable-values` directly.

Any variable whose name has a non-`nil` `risky-local-variable` property is considered risky. When you define a user option using `defcustom`, you can set its `risky-local-variable` property by adding the arguments `:risky value` to `defcustom` (see [Variable Definitions](Variable-Definitions.html)). In addition, any variable whose name ends in any of ‘`-command`’, ‘`-frame-alist`’, ‘`-function`’, ‘`-functions`’, ‘`-hook`’, ‘`-hooks`’, ‘`-form`’, ‘`-forms`’, ‘`-map`’, ‘`-map-alist`’, ‘`-mode-alist`’, ‘`-program`’, or ‘`-predicate`’ is automatically considered risky. The variables ‘`font-lock-keywords`’, ‘`font-lock-keywords`’ followed by a digit, and ‘`font-lock-syntactic-keywords`’ are also considered risky.

### Function: **risky-local-variable-p** *sym*

This function returns non-`nil` if `sym` is a risky variable, based on the above criteria.

### Variable: **ignored-local-variables**

This variable holds a list of variables that should not be given local values by files. Any value specified for one of these variables is completely ignored.

The ‘`Eval:`’ “variable” is also a potential loophole, so Emacs normally asks for confirmation before handling it.

### User Option: **enable-local-eval**

This variable controls processing of ‘`Eval:`’ in ‘`-*-`’ lines or local variables lists in files being visited. A value of `t` means process them unconditionally; `nil` means ignore them; anything else means ask the user what to do for each file. The default value is `maybe`.

### User Option: **safe-local-eval-forms**

This variable holds a list of expressions that are safe to evaluate when found in the ‘`Eval:`’ “variable” in a file local variables list.

If the expression is a function call and the function has a `safe-local-eval-function` property, the property value determines whether the expression is safe to evaluate. The property value can be a predicate to call to test the expression, a list of such predicates (it’s safe if any predicate succeeds), or `t` (always safe provided the arguments are constant).

Text properties are also potential loopholes, since their values could include functions to call. So Emacs discards all text properties from string values specified for file-local variables.
