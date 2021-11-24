

#### 18.1.4 Entering the debugger when a variable is modified

Sometimes a problem with a function is due to a wrong setting of a variable. Setting up the debugger to trigger whenever the variable is changed is a quick way to find the origin of the setting.

### Command: **debug-on-variable-change** *variable*

This function arranges for the debugger to be called whenever `variable` is modified.

It is implemented using the watchpoint mechanism, so it inherits the same characteristics and limitations: all aliases of `variable` will be watched together, only dynamic variables can be watched, and changes to the objects referenced by variables are not detected. For details, see [Watching Variables](Watching-Variables.html).

### Command: **cancel-debug-on-variable-change** *\&optional variable*

This function undoes the effect of `debug-on-variable-change` on `variable`. When called interactively, it prompts for `variable` in the minibuffer. If `variable` is omitted or `nil`, it cancels break-on-change for all variables. Calling `cancel-debug-on-variable-change` does nothing to a variable which is not currently set up to break on change.
