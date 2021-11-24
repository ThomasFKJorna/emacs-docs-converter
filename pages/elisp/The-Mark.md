

### 31.7 The Mark

Each buffer has a special marker, which is designated *the mark*. When a buffer is newly created, this marker exists but does not point anywhere; this means that the mark doesn’t exist in that buffer yet. Subsequent commands can set the mark.

The mark specifies a position to bound a range of text for many commands, such as `kill-region` and `indent-rigidly`. These commands typically act on the text between point and the mark, which is called the *region*. If you are writing a command that operates on the region, don’t examine the mark directly; instead, use `interactive` with the ‘`r`’ specification. This provides the values of point and the mark as arguments to the command in an interactive call, but permits other Lisp programs to specify arguments explicitly. See [Interactive Codes](Interactive-Codes.html).

Some commands set the mark as a side-effect. Commands should do this only if it has a potential use to the user, and never for their own internal purposes. For example, the `replace-regexp` command sets the mark to the value of point before doing any replacements, because this enables the user to move back there conveniently after the replace is finished.

Once the mark exists in a buffer, it normally never ceases to exist. However, it may become *inactive*, if Transient Mark mode is enabled. The buffer-local variable `mark-active`, if non-`nil`, means that the mark is active. A command can call the function `deactivate-mark` to deactivate the mark directly, or it can request deactivation of the mark upon return to the editor command loop by setting the variable `deactivate-mark` to a non-`nil` value.

If Transient Mark mode is enabled, certain editing commands that normally apply to text near point, apply instead to the region when the mark is active. This is the main motivation for using Transient Mark mode. (Another is that this enables highlighting of the region when the mark is active. See [Display](Display.html).)

In addition to the mark, each buffer has a *mark ring* which is a list of markers containing previous values of the mark. When editing commands change the mark, they should normally save the old value of the mark on the mark ring. The variable `mark-ring-max` specifies the maximum number of entries in the mark ring; once the list becomes this long, adding a new element deletes the last element.

There is also a separate global mark ring, but that is used only in a few particular user-level commands, and is not relevant to Lisp programming. So we do not describe it here.

### Function: **mark** *\&optional force*

This function returns the current buffer’s mark position as an integer, or `nil` if no mark has ever been set in this buffer.

If Transient Mark mode is enabled, and `mark-even-if-inactive` is `nil`, `mark` signals an error if the mark is inactive. However, if `force` is non-`nil`, then `mark` disregards inactivity of the mark, and returns the mark position (or `nil`) anyway.

### Function: **mark-marker**

This function returns the marker that represents the current buffer’s mark. It is not a copy, it is the marker used internally. Therefore, changing this marker’s position will directly affect the buffer’s mark. Don’t do that unless that is the effect you want.

```lisp
(setq m (mark-marker))
     ⇒ #<marker at 3420 in markers.texi>
```

```lisp
(set-marker m 100)
     ⇒ #<marker at 100 in markers.texi>
```

```lisp
(mark-marker)
     ⇒ #<marker at 100 in markers.texi>
```

Like any marker, this marker can be set to point at any buffer you like. If you make it point at any buffer other than the one of which it is the mark, it will yield perfectly consistent, but rather odd, results. We recommend that you not do it!

### Function: **set-mark** *position*

This function sets the mark to `position`, and activates the mark. The old value of the mark is *not* pushed onto the mark ring.

**Please note:** Use this function only if you want the user to see that the mark has moved, and you want the previous mark position to be lost. Normally, when a new mark is set, the old one should go on the `mark-ring`. For this reason, most applications should use `push-mark` and `pop-mark`, not `set-mark`.

Novice Emacs Lisp programmers often try to use the mark for the wrong purposes. The mark saves a location for the user’s convenience. An editing command should not alter the mark unless altering the mark is part of the user-level functionality of the command. (And, in that case, this effect should be documented.) To remember a location for internal use in the Lisp program, store it in a Lisp variable. For example:

```lisp
(let ((beg (point)))
  (forward-line 1)
  (delete-region beg (point))).
```

### Function: **push-mark** *\&optional position nomsg activate*

This function sets the current buffer’s mark to `position`, and pushes a copy of the previous mark onto `mark-ring`. If `position` is `nil`, then the value of point is used.

The function `push-mark` normally *does not* activate the mark. To do that, specify `t` for the argument `activate`.

A ‘`Mark set`’ message is displayed unless `nomsg` is non-`nil`.

### Function: **pop-mark**

This function pops off the top element of `mark-ring` and makes that mark become the buffer’s actual mark. This does not move point in the buffer, and it does nothing if `mark-ring` is empty. It deactivates the mark.

### User Option: **transient-mark-mode**

This variable, if non-`nil`, enables Transient Mark mode. In Transient Mark mode, every buffer-modifying primitive sets `deactivate-mark`. As a consequence, most commands that modify the buffer also deactivate the mark.

When Transient Mark mode is enabled and the mark is active, many commands that normally apply to the text near point instead apply to the region. Such commands should use the function `use-region-p` to test whether they should operate on the region. See [The Region](The-Region.html).

Lisp programs can set `transient-mark-mode` to non-`nil`, non-`t` values to enable Transient Mark mode temporarily. If the value is `lambda`, Transient Mark mode is automatically turned off after any action, such as buffer modification, that would normally deactivate the mark. If the value is `(only . oldval)`, then `transient-mark-mode` is set to the value `oldval` after any subsequent command that moves point and is not shift-translated (see [shift-translation](Key-Sequence-Input.html)), or after any other action that would normally deactivate the mark.

### User Option: **mark-even-if-inactive**

If this is non-`nil`, Lisp programs and the Emacs user can use the mark even when it is inactive. This option affects the behavior of Transient Mark mode. When the option is non-`nil`, deactivation of the mark turns off region highlighting, but commands that use the mark behave as if the mark were still active.

### Variable: **deactivate-mark**

If an editor command sets this variable non-`nil`, then the editor command loop deactivates the mark after the command returns (if Transient Mark mode is enabled). All the primitives that change the buffer set `deactivate-mark`, to deactivate the mark when the command is finished. Setting this variable makes it buffer-local.

To write Lisp code that modifies the buffer without causing deactivation of the mark at the end of the command, bind `deactivate-mark` to `nil` around the code that does the modification. For example:

```lisp
(let (deactivate-mark)
  (insert " "))
```

### Function: **deactivate-mark** *\&optional force*

If Transient Mark mode is enabled or `force` is non-`nil`, this function deactivates the mark and runs the normal hook `deactivate-mark-hook`. Otherwise, it does nothing.

### Variable: **mark-active**

The mark is active when this variable is non-`nil`. This variable is always buffer-local in each buffer. Do *not* use the value of this variable to decide whether a command that normally operates on text near point should operate on the region instead. Use the function `use-region-p` for that (see [The Region](The-Region.html)).

*   Variable: **activate-mark-hook**
*   Variable: **deactivate-mark-hook**

These normal hooks are run, respectively, when the mark becomes active and when it becomes inactive. The hook `activate-mark-hook` is also run when the region is reactivated, for instance after using a command that switches back to a buffer that has an active mark.

### Function: **handle-shift-selection**

This function implements the shift-selection behavior of point-motion commands. See [Shift Selection](https://www.gnu.org/software/emacs/manual/html_node/emacs/Shift-Selection.html#Shift-Selection) in The GNU Emacs Manual. It is called automatically by the Emacs command loop whenever a command with a ‘`^`’ character in its `interactive` spec is invoked, before the command itself is executed (see [^](Interactive-Codes.html)).

If `shift-select-mode` is non-`nil` and the current command was invoked via shift translation (see [shift-translation](Key-Sequence-Input.html)), this function sets the mark and temporarily activates the region, unless the region was already temporarily activated in this way. Otherwise, if the region has been activated temporarily, it deactivates the mark and restores the variable `transient-mark-mode` to its earlier value.

### Variable: **mark-ring**

The value of this buffer-local variable is the list of saved former marks of the current buffer, most recent first.

```lisp
mark-ring
⇒ (#<marker at 11050 in markers.texi>
    #<marker at 10832 in markers.texi>
    …)
```

### User Option: **mark-ring-max**

The value of this variable is the maximum size of `mark-ring`. If more marks than this are pushed onto the `mark-ring`, `push-mark` discards an old mark when it adds a new one.

When Delete Selection mode (see [Delete Selection](https://www.gnu.org/software/emacs/manual/html_node/emacs/Using-Region.html#Using-Region) in The GNU Emacs Manual) is enabled, commands that operate on the active region (a.k.a. “selection”) behave slightly differently. This works by adding the function `delete-selection-pre-hook` to the `pre-command-hook` (see [Command Overview](Command-Overview.html)). That function calls `delete-selection-helper` to delete the selection as appropriate for the command. If you want to adapt a command to Delete Selection mode, put the `delete-selection` property on the function’s symbol (see [Symbol Plists](Symbol-Plists.html)); commands that don’t have this property on their symbol won’t delete the selection. This property can have one of several values to tailor the behavior to what the command is supposed to do; see the doc strings of `delete-selection-pre-hook` and `delete-selection-helper` for the details.
