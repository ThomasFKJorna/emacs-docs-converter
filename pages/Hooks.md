

Next: [Major Modes](Major-Modes.html), Up: [Modes](Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 23.1 Hooks

A *hook* is a variable where you can store a function or functions to be called on a particular occasion by an existing program. Emacs provides hooks for the sake of customization. Most often, hooks are set up in the init file (see [Init File](Init-File.html)), but Lisp programs can set them also. See [Standard Hooks](Standard-Hooks.html), for a list of some standard hook variables.

Most of the hooks in Emacs are *normal hooks*. These variables contain lists of functions to be called with no arguments. By convention, whenever the hook name ends in ‘`-hook`’, that tells you it is normal. We try to make all hooks normal, as much as possible, so that you can use them in a uniform way.

Every major mode command is supposed to run a normal hook called the *mode hook* as one of the last steps of initialization. This makes it easy for a user to customize the behavior of the mode, by overriding the buffer-local variable assignments already made by the mode. Most minor mode functions also run a mode hook at the end. But hooks are used in other contexts too. For example, the hook `suspend-hook` runs just before Emacs suspends itself (see [Suspending Emacs](Suspending-Emacs.html)).

The recommended way to add a hook function to a hook is by calling `add-hook` (see [Setting Hooks](Setting-Hooks.html)). The hook functions may be any of the valid kinds of functions that `funcall` accepts (see [What Is a Function](What-Is-a-Function.html)). Most normal hook variables are initially void; `add-hook` knows how to deal with this. You can add hooks either globally or buffer-locally with `add-hook`.

If the hook variable’s name does not end with ‘`-hook`’, that indicates it is probably an *abnormal hook*. That means the hook functions are called with arguments, or their return values are used in some way. The hook’s documentation says how the functions are called. You can use `add-hook` to add a function to an abnormal hook, but you must write the function to follow the hook’s calling convention. By convention, abnormal hook names end in ‘`-functions`’.

If the variable’s name ends in ‘`-function`’, then its value is just a single function, not a list of functions. `add-hook` cannot be used to modify such a *single function hook*, and you have to use `add-function` instead (see [Advising Functions](Advising-Functions.html)).

|                                       |    |                                                 |
| :------------------------------------ | -- | :---------------------------------------------- |
| • [Running Hooks](Running-Hooks.html) |    | How to run a hook.                              |
| • [Setting Hooks](Setting-Hooks.html) |    | How to put functions on a hook, or remove them. |

Next: [Major Modes](Major-Modes.html), Up: [Modes](Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
