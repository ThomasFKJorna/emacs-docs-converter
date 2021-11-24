

### 21.14 Disabling Commands

*Disabling a command* marks the command as requiring user confirmation before it can be executed. Disabling is used for commands which might be confusing to beginning users, to prevent them from using the commands by accident.

The low-level mechanism for disabling a command is to put a non-`nil` `disabled` property on the Lisp symbol for the command. These properties are normally set up by the user’s init file (see [Init File](Init-File.html)) with Lisp expressions such as this:

```lisp
(put 'upcase-region 'disabled t)
```

For a few commands, these properties are present by default (you can remove them in your init file if you wish).

If the value of the `disabled` property is a string, the message saying the command is disabled includes that string. For example:

```lisp
(put 'delete-region 'disabled
     "Text deleted this way cannot be yanked back!\n")
```

See [Disabling](https://www.gnu.org/software/emacs/manual/html_node/emacs/Disabling.html#Disabling) in The GNU Emacs Manual, for the details on what happens when a disabled command is invoked interactively. Disabling a command has no effect on calling it as a function from Lisp programs.

### Command: **enable-command** *command*

Allow `command` (a symbol) to be executed without special confirmation from now on, and alter the user’s init file (see [Init File](Init-File.html)) so that this will apply to future sessions.

### Command: **disable-command** *command*

Require special confirmation to execute `command` from now on, and alter the user’s init file so that this will apply to future sessions.

### Variable: **disabled-command-function**

The value of this variable should be a function. When the user invokes a disabled command interactively, this function is called instead of the disabled command. It can use `this-command-keys` to determine what the user typed to run the command, and thus find the command itself.

The value may also be `nil`. Then all commands work normally, even disabled ones.

By default, the value is a function that asks the user whether to proceed.
