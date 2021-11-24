

### 15.3 Defining Customization Variables

*Customizable variables*, also called *user options*, are global Lisp variables whose values can be set through the Customize interface. Unlike other global variables, which are defined with `defvar` (see [Defining Variables](Defining-Variables.html)), customizable variables are defined using the `defcustom` macro. In addition to calling `defvar` as a subroutine, `defcustom` states how the variable should be displayed in the Customize interface, the values it is allowed to take, etc.

### Macro: **defcustom** *option standard doc \[keyword value]…*

This macro declares `option` as a user option (i.e., a customizable variable). You should not quote `option`.

The argument `standard` is an expression that specifies the standard value for `option`. Evaluating the `defcustom` form evaluates `standard`, but does not necessarily bind the option to that value. If `option` already has a default value, it is left unchanged. If the user has already saved a customization for `option`, the user’s customized value is installed as the default value. Otherwise, the result of evaluating `standard` is installed as the default value.

Like `defvar`, this macro marks `option` as a special variable, meaning that it should always be dynamically bound. If `option` is already lexically bound, that lexical binding remains in effect until the binding construct exits. See [Variable Scoping](Variable-Scoping.html).

The expression `standard` can be evaluated at various other times, too—whenever the customization facility needs to know `option`’s standard value. So be sure to use an expression which is harmless to evaluate at any time.

The argument `doc` specifies the documentation string for the variable.

If a `defcustom` does not specify any `:group`, the last group defined with `defgroup` in the same file will be used. This way, most `defcustom` do not need an explicit `:group`.

When you evaluate a `defcustom` form with `C-M-x` in Emacs Lisp mode (`eval-defun`), a special feature of `eval-defun` arranges to set the variable unconditionally, without testing whether its value is void. (The same feature applies to `defvar`, see [Defining Variables](Defining-Variables.html).) Using `eval-defun` on a defcustom that is already defined calls the `:set` function (see below), if there is one.

If you put a `defcustom` in a pre-loaded Emacs Lisp file (see [Building Emacs](Building-Emacs.html)), the standard value installed at dump time might be incorrect, e.g., because another variable that it depends on has not been assigned the right value yet. In that case, use `custom-reevaluate-setting`, described below, to re-evaluate the standard value after Emacs starts up.

In addition to the keywords listed in [Common Keywords](Common-Keywords.html), this macro accepts the following keywords:

### `:type type`

Use `type` as the data type for this option. It specifies which values are legitimate, and how to display the value (see [Customization Types](Customization-Types.html)). Every `defcustom` should specify a value for this keyword.

### `:options value-list`

Specify the list of reasonable values for use in this option. The user is not restricted to using only these values, but they are offered as convenient alternatives.

This is meaningful only for certain types, currently including `hook`, `plist` and `alist`. See the definition of the individual types for a description of how to use `:options`.

### `:set setfunction`

Specify `setfunction` as the way to change the value of this option when using the Customize interface. The function `setfunction` should take two arguments, a symbol (the option name) and the new value, and should do whatever is necessary to update the value properly for this option (which may not mean simply setting the option as a Lisp variable); preferably, though, it should not modify its value argument destructively. The default for `setfunction` is `set-default`.

If you specify this keyword, the variable’s documentation string should describe how to do the same job in hand-written Lisp code.

### `:get getfunction`

Specify `getfunction` as the way to extract the value of this option. The function `getfunction` should take one argument, a symbol, and should return whatever customize should use as the current value for that symbol (which need not be the symbol’s Lisp value). The default is `default-value`.

You have to really understand the workings of Custom to use `:get` correctly. It is meant for values that are treated in Custom as variables but are not actually stored in Lisp variables. It is almost surely a mistake to specify `getfunction` for a value that really is stored in a Lisp variable.

### `:initialize function`

`function` should be a function used to initialize the variable when the `defcustom` is evaluated. It should take two arguments, the option name (a symbol) and the value. Here are some predefined functions meant for use in this way:

*   `custom-initialize-set`

    Use the variable’s `:set` function to initialize the variable, but do not reinitialize it if it is already non-void.

*   `custom-initialize-default`

    Like `custom-initialize-set`, but use the function `set-default` to set the variable, instead of the variable’s `:set` function. This is the usual choice for a variable whose `:set` function enables or disables a minor mode; with this choice, defining the variable will not call the minor mode function, but customizing the variable will do so.

*   `custom-initialize-reset`

    Always use the `:set` function to initialize the variable. If the variable is already non-void, reset it by calling the `:set` function using the current value (returned by the `:get` method). This is the default `:initialize` function.

*   `custom-initialize-changed`

    Use the `:set` function to initialize the variable, if it is already set or has been customized; otherwise, just use `set-default`.

*   `custom-initialize-delay`

    This function behaves like `custom-initialize-set`, but it delays the actual initialization to the next Emacs start. This should be used in files that are preloaded (or for autoloaded variables), so that the initialization is done in the run-time context rather than the build-time context. This also has the side-effect that the (delayed) initialization is performed with the `:set` function. See [Building Emacs](Building-Emacs.html).

### `:local value`

If the `value` is `t`, mark `option` as automatically buffer-local; if the value is `permanent`, also set `option`s `permanent-local` property to `t`. See [Creating Buffer-Local](Creating-Buffer_002dLocal.html).

### `:risky value`

Set the variable’s `risky-local-variable` property to `value` (see [File Local Variables](File-Local-Variables.html)).

### `:safe function`

Set the variable’s `safe-local-variable` property to `function` (see [File Local Variables](File-Local-Variables.html)).

### `:set-after variables`

When setting variables according to saved customizations, make sure to set the variables `variables` before this one; i.e., delay setting this variable until after those others have been handled. Use `:set-after` if setting this variable won’t work properly unless those other variables already have their intended values.

It is useful to specify the `:require` keyword for an option that turns on a certain feature. This causes Emacs to load the feature, if it is not already loaded, whenever the option is set. See [Common Keywords](Common-Keywords.html). Here is an example:

```lisp
(defcustom frobnicate-automatically nil
  "Non-nil means automatically frobnicate all buffers."
  :type 'boolean
  :require 'frobnicate-mode
  :group 'frobnicate)
```

If a customization item has a type such as `hook` or `alist`, which supports `:options`, you can add additional values to the list from outside the `defcustom` declaration by calling `custom-add-frequent-value`. For example, if you define a function `my-lisp-mode-initialization` intended to be called from `emacs-lisp-mode-hook`, you might want to add that to the list of reasonable values for `emacs-lisp-mode-hook`, but not by editing its definition. You can do it thus:

```lisp
(custom-add-frequent-value 'emacs-lisp-mode-hook
   'my-lisp-mode-initialization)
```

### Function: **custom-add-frequent-value** *symbol value*

For the customization option `symbol`, add `value` to the list of reasonable values.

The precise effect of adding a value depends on the customization type of `symbol`.

Internally, `defcustom` uses the symbol property `standard-value` to record the expression for the standard value, `saved-value` to record the value saved by the user with the customization buffer, and `customized-value` to record the value set by the user with the customization buffer, but not saved. See [Symbol Properties](Symbol-Properties.html). In addition, there’s `themed-value`, which is used to record the value set by a theme (see [Custom Themes](Custom-Themes.html)). These properties are lists, the car of which is an expression that evaluates to the value.

### Function: **custom-reevaluate-setting** *symbol*

This function re-evaluates the standard value of `symbol`, which should be a user option declared via `defcustom`. If the variable was customized, this function re-evaluates the saved value instead. Then it sets the user option to that value (using the option’s `:set` property if that is defined).

This is useful for customizable options that are defined before their value could be computed correctly. For example, during startup Emacs calls this function for some user options that were defined in pre-loaded Emacs Lisp files, but whose initial values depend on information available only at run-time.

### Function: **custom-variable-p** *arg*

This function returns non-`nil` if `arg` is a customizable variable. A customizable variable is either a variable that has a `standard-value` or `custom-autoload` property (usually meaning it was declared with `defcustom`), or an alias for another customizable variable.
