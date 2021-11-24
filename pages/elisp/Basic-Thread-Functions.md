

### 37.1 Basic Thread Functions

Threads can be created and waited for. A thread cannot be exited directly, but the current thread can be exited implicitly, and other threads can be signaled.

### Function: **make-thread** *function \&optional name*

Create a new thread of execution which invokes `function`. When `function` returns, the thread exits.

The new thread is created with no local variable bindings in effect. The new thread’s current buffer is inherited from the current thread.

`name` can be supplied to give a name to the thread. The name is used for debugging and informational purposes only; it has no meaning to Emacs. If `name` is provided, it must be a string.

This function returns the new thread.

### Function: **threadp** *object*

This function returns `t` if `object` represents an Emacs thread, `nil` otherwise.

### Function: **thread-join** *thread*

Block until `thread` exits, or until the current thread is signaled. It returns the result of the `thread` function. If `thread` has already exited, this returns immediately.

### Function: **thread-signal** *thread error-symbol data*

Like `signal` (see [Signaling Errors](Signaling-Errors.html)), but the signal is delivered in the thread `thread`. If `thread` is the current thread, then this just calls `signal` immediately. Otherwise, `thread` will receive the signal as soon as it becomes current. If `thread` was blocked by a call to `mutex-lock`, `condition-wait`, or `thread-join`; `thread-signal` will unblock it.

If `thread` is the main thread, the signal is not propagated there. Instead, it is shown as message in the main thread.

### Function: **thread-yield**

Yield execution to the next runnable thread.

### Function: **thread-name** *thread*

Return the name of `thread`, as specified to `make-thread`.

### Function: **thread-live-p** *thread*

Return `t` if `thread` is alive, or `nil` if it is not. A thread is alive as long as its function is still executing.

### Function: **thread--blocker** *thread*

Return the object that `thread` is waiting on. This function is primarily intended for debugging, and is given a “double hyphen” name to indicate that.

If `thread` is blocked in `thread-join`, this returns the thread for which it is waiting.

If `thread` is blocked in `mutex-lock`, this returns the mutex.

If `thread` is blocked in `condition-wait`, this returns the condition variable.

Otherwise, this returns `nil`.

### Function: **current-thread**

Return the current thread.

### Function: **all-threads**

Return a list of all the live thread objects. A new list is returned by each invocation.

### Variable: **main-thread**

This variable keeps the main thread Emacs is running, or `nil` if Emacs is compiled without thread support.

When code run by a thread signals an error that is unhandled, the thread exits. Other threads can access the error form which caused the thread to exit using the following function.

### Function: **thread-last-error** *\&optional cleanup*

This function returns the last error form recorded when a thread exited due to an error. Each thread that exits abnormally overwrites the form stored by the previous thread’s error with a new value, so only the last one can be accessed. If `cleanup` is non-`nil`, the stored form is reset to `nil`.
