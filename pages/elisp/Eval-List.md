

#### 18.2.10 Evaluation List Buffer

You can use the *evaluation list buffer*, called `*edebug*`, to evaluate expressions interactively. You can also set up the *evaluation list* of expressions to be evaluated automatically each time Edebug updates the display.

`E`

Switch to the evaluation list buffer `*edebug*` (`edebug-visit-eval-list`).

In the `*edebug*` buffer you can use the commands of Lisp Interaction mode (see [Lisp Interaction](https://www.gnu.org/software/emacs/manual/html_node/emacs/Lisp-Interaction.html#Lisp-Interaction) in The GNU Emacs Manual) as well as these special commands:

`C-j`

Evaluate the expression before point, in the outside context, and insert the value in the buffer (`edebug-eval-print-last-sexp`). With prefix argument of zero (`C-u 0 C-j`), don’t shorten long items (like strings and lists).

`C-x C-e`

Evaluate the expression before point, in the context outside of Edebug (`edebug-eval-last-sexp`).

`C-c C-u`

Build a new evaluation list from the contents of the buffer (`edebug-update-eval-list`).

`C-c C-d`

Delete the evaluation list group that point is in (`edebug-delete-eval-item`).

`C-c C-w`

Switch back to the source code buffer at the current stop point (`edebug-where`).

You can evaluate expressions in the evaluation list window with `C-j` or `C-x C-e`, just as you would in `*scratch*`; but they are evaluated in the context outside of Edebug.

The expressions you enter interactively (and their results) are lost when you continue execution; but you can set up an *evaluation list* consisting of expressions to be evaluated each time execution stops.

To do this, write one or more *evaluation list groups* in the evaluation list buffer. An evaluation list group consists of one or more Lisp expressions. Groups are separated by comment lines.

The command `C-c C-u` (`edebug-update-eval-list`) rebuilds the evaluation list, scanning the buffer and using the first expression of each group. (The idea is that the second expression of the group is the value previously computed and displayed.)

Each entry to Edebug redisplays the evaluation list by inserting each expression in the buffer, followed by its current value. It also inserts comment lines so that each expression becomes its own group. Thus, if you type `C-c C-u` again without changing the buffer text, the evaluation list is effectively unchanged.

If an error occurs during an evaluation from the evaluation list, the error message is displayed in a string as if it were the result. Therefore, expressions using variables that are not currently valid do not interrupt your debugging.

Here is an example of what the evaluation list window looks like after several expressions have been added to it:

```lisp
(current-buffer)
#<buffer *scratch*>
;---------------------------------------------------------------
(selected-window)
#<window 16 on *scratch*>
;---------------------------------------------------------------
(point)
196
;---------------------------------------------------------------
bad-var
"Symbol's value as variable is void: bad-var"
;---------------------------------------------------------------
(recursion-depth)
0
;---------------------------------------------------------------
this-command
eval-last-sexp
;---------------------------------------------------------------
```

To delete a group, move point into it and type `C-c C-d`, or simply delete the text for the group and update the evaluation list with `C-c C-u`. To add a new expression to the evaluation list, insert the expression at a suitable place, insert a new comment line, then type `C-c C-u`. You need not insert dashes in the comment line—its contents don’t matter.

After selecting `*edebug*`, you can return to the source code buffer with `C-c C-w`. The `*edebug*` buffer is killed when you continue execution, and recreated next time it is needed.
