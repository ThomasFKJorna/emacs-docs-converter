

### 21.1 Command Loop Overview

The first thing the command loop must do is read a key sequence, which is a sequence of input events that translates into a command. It does this by calling the function `read-key-sequence`. Lisp programs can also call this function (see [Key Sequence Input](Key-Sequence-Input.html)). They can also read input at a lower level with `read-key` or `read-event` (see [Reading One Event](Reading-One-Event.html)), or discard pending input with `discard-input` (see [Event Input Misc](Event-Input-Misc.html)).

The key sequence is translated into a command through the currently active keymaps. See [Key Lookup](Key-Lookup.html), for information on how this is done. The result should be a keyboard macro or an interactively callable function. If the key is `M-x`, then it reads the name of another command, which it then calls. This is done by the command `execute-extended-command` (see [Interactive Call](Interactive-Call.html)).

Prior to executing the command, Emacs runs `undo-boundary` to create an undo boundary. See [Maintaining Undo](Maintaining-Undo.html).

To execute a command, Emacs first reads its arguments by calling `command-execute` (see [Interactive Call](Interactive-Call.html)). For commands written in Lisp, the `interactive` specification says how to read the arguments. This may use the prefix argument (see [Prefix Command Arguments](Prefix-Command-Arguments.html)) or may read with prompting in the minibuffer (see [Minibuffers](Minibuffers.html)). For example, the command `find-file` has an `interactive` specification which says to read a file name using the minibuffer. The function body of `find-file` does not use the minibuffer, so if you call `find-file` as a function from Lisp code, you must supply the file name string as an ordinary Lisp function argument.

If the command is a keyboard macro (i.e., a string or vector), Emacs executes it using `execute-kbd-macro` (see [Keyboard Macros](Keyboard-Macros.html)).

### Variable: **pre-command-hook**

This normal hook is run by the editor command loop before it executes each command. At that time, `this-command` contains the command that is about to run, and `last-command` describes the previous command. See [Command Loop Info](Command-Loop-Info.html).

### Variable: **post-command-hook**

This normal hook is run by the editor command loop after it executes each command (including commands terminated prematurely by quitting or by errors). At that time, `this-command` refers to the command that just ran, and `last-command` refers to the command before that.

This hook is also run when Emacs first enters the command loop (at which point `this-command` and `last-command` are both `nil`).

Quitting is suppressed while running `pre-command-hook` and `post-command-hook`. If an error happens while executing one of these hooks, it does not terminate execution of the hook; instead the error is silenced and the function in which the error occurred is removed from the hook.

A request coming into the Emacs server (see [Emacs Server](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Server.html#Emacs-Server) in The GNU Emacs Manual) runs these two hooks just as a keyboard command does.
