

#### 23.7.1.9 Customizing Indentation

If you are using a mode whose indentation is provided by SMIE, you can customize the indentation to suit your preferences. You can do this on a per-mode basis (using the option `smie-config`), or a per-file basis (using the function `smie-config-local` in a file-local variable specification).

### User Option: **smie-config**

This option lets you customize indentation on a per-mode basis. It is an alist with elements of the form `(mode . rules)`. For the precise form of rules, see the variable’s documentation; but you may find it easier to use the command `smie-config-guess`.

### Command: **smie-config-guess**

This command tries to work out appropriate settings to produce your preferred style of indentation. Simply call the command while visiting a file that is indented with your style.

### Command: **smie-config-save**

Call this command after using `smie-config-guess`, to save your settings for future sessions.

### Command: **smie-config-show-indent** *\&optional move*

This command displays the rules that are used to indent the current line.

### Command: **smie-config-set-indent**

This command adds a local rule to adjust the indentation of the current line.

### Function: **smie-config-local** *rules*

This function adds `rules` as indentation rules for the current buffer. These add to any mode-specific rules defined by the `smie-config` option. To specify custom indentation rules for a specific file, add an entry to the file’s local variables of the form: `eval: (smie-config-local '(rules))`.
