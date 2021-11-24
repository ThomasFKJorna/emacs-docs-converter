

#### 23.1.2 Setting Hooks

Here’s an example that adds a function to a mode hook to turn on Auto Fill mode when in Lisp Interaction mode:

```lisp
(add-hook 'lisp-interaction-mode-hook 'auto-fill-mode)
```

The value of a hook variable should be a list of functions. You can manipulate that list using the normal Lisp facilities, but the modular way is to use the functions `add-hook` and `remove-hook`, defined below. They take care to handle some unusual situations and avoid problems.

It works to put a `lambda`-expression function on a hook, but we recommend avoiding this because it can lead to confusion. If you add the same `lambda`-expression a second time but write it slightly differently, you will get two equivalent but distinct functions on the hook. If you then remove one of them, the other will still be on it.

### Function: **add-hook** *hook function \&optional depth local*

This function is the handy way to add function `function` to hook variable `hook`. You can use it for abnormal hooks as well as for normal hooks. `function` can be any Lisp function that can accept the proper number of arguments for `hook`. For example,

```lisp
(add-hook 'text-mode-hook 'my-text-hook-function)
```

adds `my-text-hook-function` to the hook called `text-mode-hook`.

If `function` is already present in `hook` (comparing using `equal`), then `add-hook` does not add it a second time.

If `function` has a non-`nil` property `permanent-local-hook`, then `kill-all-local-variables` (or changing major modes) won’t delete it from the hook variable’s local value.

For a normal hook, hook functions should be designed so that the order in which they are executed does not matter. Any dependence on the order is asking for trouble. However, the order is predictable: normally, `function` goes at the front of the hook list, so it is executed first (barring another `add-hook` call).

In some cases, it is important to control the relative ordering of functions on the hook. The optional argument `depth` lets you indicate where the function should be inserted in the list: it should then be a number between -100 and 100 where the higher the value, the closer to the end of the list the function should go. The `depth` defaults to 0 and for backward compatibility when `depth` is a non-nil symbol it is interpreted as a depth of 90. Furthermore, when `depth` is strictly greater than 0 the function is added *after* rather than before functions of the same depth. One should never use a depth of 100 (or -100), because one can never be sure that no other function will ever need to come before (or after) us.

`add-hook` can handle the cases where `hook` is void or its value is a single function; it sets or changes the value to a list of functions.

If `local` is non-`nil`, that says to add `function` to the buffer-local hook list instead of to the global hook list. This makes the hook buffer-local and adds `t` to the buffer-local value. The latter acts as a flag to run the hook functions in the default value as well as in the local value.

### Function: **remove-hook** *hook function \&optional local*

This function removes `function` from the hook variable `hook`. It compares `function` with elements of `hook` using `equal`, so it works for both symbols and lambda expressions.

If `local` is non-`nil`, that says to remove `function` from the buffer-local hook list instead of from the global hook list.
