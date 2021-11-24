

#### 21.8.6 Miscellaneous Event Input Features

This section describes how to peek ahead at events without using them up, how to check for pending input, and how to discard pending input. See also the function `read-passwd` (see [Reading a Password](Reading-a-Password.html)).

### Variable: **unread-command-events**

This variable holds a list of events waiting to be read as command input. The events are used in the order they appear in the list, and removed one by one as they are used.

The variable is needed because in some cases a function reads an event and then decides not to use it. Storing the event in this variable causes it to be processed normally, by the command loop or by the functions to read command input.

For example, the function that implements numeric prefix arguments reads any number of digits. When it finds a non-digit event, it must unread the event so that it can be read normally by the command loop. Likewise, incremental search uses this feature to unread events with no special meaning in a search, because these events should exit the search and then execute normally.

The reliable and easy way to extract events from a key sequence so as to put them in `unread-command-events` is to use `listify-key-sequence` (see below).

Normally you add events to the front of this list, so that the events most recently unread will be reread first.

Events read from this list are not normally added to the current command’s key sequence (as returned by, e.g., `this-command-keys`), as the events will already have been added once as they were read for the first time. An element of the form `(t . event)` forces `event` to be added to the current command’s key sequence.

Elements read from this list are normally recorded by the record-keeping features (see [Recording Input](Recording-Input.html)) and while defining a keyboard macro (see [Keyboard Macros](Keyboard-Macros.html)). However, an element of the form `(no-record . event)` causes `event` to be processed normally without recording it.

### Function: **listify-key-sequence** *key*

This function converts the string or vector `key` to a list of individual events, which you can put in `unread-command-events`.

### Function: **input-pending-p** *\&optional check-timers*

This function determines whether any command input is currently available to be read. It returns immediately, with value `t` if there is available input, `nil` otherwise. On rare occasions it may return `t` when no input is available.

If the optional argument `check-timers` is non-`nil`, then if no input is available, Emacs runs any timers which are ready. See [Timers](Timers.html).

### Variable: **last-input-event**

This variable records the last terminal input event read, whether as part of a command or explicitly by a Lisp program.

In the example below, the Lisp program reads the character `1`, ASCII code 49. It becomes the value of `last-input-event`, while `C-e` (we assume `C-x C-e` command is used to evaluate this expression) remains the value of `last-command-event`.

```lisp
(progn (print (read-char))
       (print last-command-event)
       last-input-event)
     -| 49
     -| 5
     ⇒ 49
```

### Macro: **while-no-input** *body…*

This construct runs the `body` forms and returns the value of the last one—but only if no input arrives. If any input arrives during the execution of the `body` forms, it aborts them (working much like a quit). The `while-no-input` form returns `nil` if aborted by a real quit, and returns `t` if aborted by arrival of other input.

If a part of `body` binds `inhibit-quit` to non-`nil`, arrival of input during those parts won’t cause an abort until the end of that part.

If you want to be able to distinguish all possible values computed by `body` from both kinds of abort conditions, write the code like this:

```lisp
(while-no-input
  (list
    (progn . body)))
```

### Variable: **while-no-input-ignore-events**

This variable allow setting which special events `while-no-input` should ignore. It is a list of event symbols (see [Event Examples](Event-Examples.html)).

### Function: **discard-input**

This function discards the contents of the terminal input buffer and cancels any keyboard macro that might be in the process of definition. It returns `nil`.

In the following example, the user may type a number of characters right after starting the evaluation of the form. After the `sleep-for` finishes sleeping, `discard-input` discards any characters typed during the sleep.

```lisp
(progn (sleep-for 2)
       (discard-input))
     ⇒ nil
```
