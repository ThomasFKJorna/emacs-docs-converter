

### 32.31 Atomic Change Groups

In database terminology, an *atomic* change is an indivisible change—it can succeed entirely or it can fail entirely, but it cannot partly succeed. A Lisp program can make a series of changes to one or several buffers as an *atomic change group*, meaning that either the entire series of changes will be installed in their buffers or, in case of an error, none of them will be.

To do this for one buffer, the one already current, simply write a call to `atomic-change-group` around the code that makes the changes, like this:

```lisp
(atomic-change-group
  (insert foo)
  (delete-region x y))
```

If an error (or other nonlocal exit) occurs inside the body of `atomic-change-group`, it unmakes all the changes in that buffer that were during the execution of the body. This kind of change group has no effect on any other buffers—any such changes remain.

If you need something more sophisticated, such as to make changes in various buffers constitute one atomic group, you must directly call lower-level functions that `atomic-change-group` uses.

### Function: **prepare-change-group** *\&optional buffer*

This function sets up a change group for buffer `buffer`, which defaults to the current buffer. It returns a handle that represents the change group. You must use this handle to activate the change group and subsequently to finish it.

To use the change group, you must *activate* it. You must do this before making any changes in the text of `buffer`.

### Function: **activate-change-group** *handle*

This function activates the change group that `handle` designates.

After you activate the change group, any changes you make in that buffer become part of it. Once you have made all the desired changes in the buffer, you must *finish* the change group. There are two ways to do this: you can either accept (and finalize) all the changes, or cancel them all.

### Function: **accept-change-group** *handle*

This function accepts all the changes in the change group specified by `handle`, making them final.

### Function: **cancel-change-group** *handle*

This function cancels and undoes all the changes in the change group specified by `handle`.

You can cause some or all of the changes in a change group to be considered as a single unit for the purposes of the `undo` commands (see [Undo](Undo.html)) by using `undo-amalgamate-change-group`.

### Function: **undo-amalgamate-change-group**

Amalgamate all the changes made in the change-group since the state identified by `handle`. This function removes all undo boundaries between undo records of changes since the state described by `handle`. Usually, `handle` is the handle returned by `prepare-change-group`, in which case all the changes since the beginning of the change-group are amalgamated into a single undo unit.

Your code should use `unwind-protect` to make sure the group is always finished. The call to `activate-change-group` should be inside the `unwind-protect`, in case the user types `C-g` just after it runs. (This is one reason why `prepare-change-group` and `activate-change-group` are separate functions, because normally you would call `prepare-change-group` before the start of that `unwind-protect`.) Once you finish the group, don’t use the handle again—in particular, don’t try to finish the same group twice.

To make a multibuffer change group, call `prepare-change-group` once for each buffer you want to cover, then use `nconc` to combine the returned values, like this:

```lisp
(nconc (prepare-change-group buffer-1)
       (prepare-change-group buffer-2))
```

You can then activate the multibuffer change group with a single call to `activate-change-group`, and finish it with a single call to `accept-change-group` or `cancel-change-group`.

Nested use of several change groups for the same buffer works as you would expect. Non-nested use of change groups for the same buffer will get Emacs confused, so don’t let it happen; the first change group you start for any given buffer should be the last one finished.
