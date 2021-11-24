

### 20.7 Yes-or-No Queries

This section describes functions used to ask the user a yes-or-no question. The function `y-or-n-p` can be answered with a single character; it is useful for questions where an inadvertent wrong answer will not have serious consequences. `yes-or-no-p` is suitable for more momentous questions, since it requires three or four characters to answer.

If either of these functions is called in a command that was invoked using the mouse—more precisely, if `last-nonmenu-event` (see [Command Loop Info](Command-Loop-Info.html)) is either `nil` or a list—then it uses a dialog box or pop-up menu to ask the question. Otherwise, it uses keyboard input. You can force use either of the mouse or of keyboard input by binding `last-nonmenu-event` to a suitable value around the call.

Both `yes-or-no-p` and `y-or-n-p` use the minibuffer.

### Function: **y-or-n-p** *prompt*

This function asks the user a question, expecting input in the minibuffer. It returns `t` if the user types `y`, `nil` if the user types `n`. This function also accepts `SPC` to mean yes and `DEL` to mean no. It accepts `C-]` and `C-g` to quit, because the question uses the minibuffer and for that reason the user might try to use `C-]` to get out. The answer is a single character, with no `RET` needed to terminate it. Upper and lower case are equivalent.

“Asking the question” means printing `prompt` in the minibuffer, followed by the string ‘`(y or n) `’. If the input is not one of the expected answers (`y`, `n`, `SPC`, `DEL`, or something that quits), the function responds ‘`Please answer y or n.`’, and repeats the request.

This function actually uses the minibuffer, but does not allow editing of the answer. The cursor moves to the minibuffer while the question is being asked.

The answers and their meanings, even ‘`y`’ and ‘`n`’, are not hardwired, and are specified by the keymap `query-replace-map` (see [Search and Replace](Search-and-Replace.html)). In particular, if the user enters the special responses `recenter`, `scroll-up`, `scroll-down`, `scroll-other-window`, or `scroll-other-window-down` (respectively bound to `C-l`, `C-v`, `M-v`, `C-M-v` and `C-M-S-v` in `query-replace-map`), this function performs the specified window recentering or scrolling operation, and poses the question again.

### Function: **y-or-n-p-with-timeout** *prompt seconds default*

Like `y-or-n-p`, except that if the user fails to answer within `seconds` seconds, this function stops waiting and returns `default`. It works by setting up a timer; see [Timers](Timers.html). The argument `seconds` should be a number.

### Function: **yes-or-no-p** *prompt*

This function asks the user a question, expecting input in the minibuffer. It returns `t` if the user enters ‘`yes`’, `nil` if the user types ‘`no`’. The user must type `RET` to finalize the response. Upper and lower case are equivalent.

`yes-or-no-p` starts by displaying `prompt` in the minibuffer, followed by ‘`(yes or no) `’. The user must type one of the expected responses; otherwise, the function responds ‘`Please answer yes or no.`’, waits about two seconds and repeats the request.

`yes-or-no-p` requires more work from the user than `y-or-n-p` and is appropriate for more crucial decisions.

Here is an example:

```lisp
(yes-or-no-p "Do you really want to remove everything? ")

;; After evaluation of the preceding expression,
;;   the following prompt appears,
;;   with an empty minibuffer:
```

```lisp
```

```lisp
---------- Buffer: minibuffer ----------
Do you really want to remove everything? (yes or no)
---------- Buffer: minibuffer ----------
```

If the user first types `y RET`, which is invalid because this function demands the entire word ‘`yes`’, it responds by displaying these prompts, with a brief pause between them:

```lisp
---------- Buffer: minibuffer ----------
Please answer yes or no.
Do you really want to remove everything? (yes or no)
---------- Buffer: minibuffer ----------
```
