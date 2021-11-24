

### 37.3 Condition Variables

A *condition variable* is a way for a thread to block until some event occurs. A thread can wait on a condition variable, to be woken up when some other thread notifies the condition.

A condition variable is associated with a mutex and, conceptually, with some condition. For proper operation, the mutex must be acquired, and then a waiting thread must loop, testing the condition and waiting on the condition variable. For example:

```lisp
(with-mutex mutex
  (while (not global-variable)
    (condition-wait cond-var)))
```

The mutex ensures atomicity, and the loop is for robustness—there may be spurious notifications.

Similarly, the mutex must be held before notifying the condition. The typical, and best, approach is to acquire the mutex, make the changes associated with this condition, and then notify it:

```lisp
(with-mutex mutex
  (setq global-variable (some-computation))
  (condition-notify cond-var))
```

### Function: **make-condition-variable** *mutex \&optional name*

Make a new condition variable associated with `mutex`. If `name` is specified, it is a name given to the condition variable. It must be a string. The name is for debugging purposes only; it has no meaning to Emacs.

### Function: **condition-variable-p** *object*

This function returns `t` if `object` represents a condition variable, `nil` otherwise.

### Function: **condition-wait** *cond*

Wait for another thread to notify `cond`, a condition variable. This function will block until the condition is notified, or until a signal is delivered to this thread using `thread-signal`.

It is an error to call `condition-wait` without holding the condition’s associated mutex.

`condition-wait` releases the associated mutex while waiting. This allows other threads to acquire the mutex in order to notify the condition.

### Function: **condition-notify** *cond \&optional all*

Notify `cond`. The mutex with `cond` must be held before calling this. Ordinarily a single waiting thread is woken by `condition-notify`; but if `all` is not `nil`, then all threads waiting on `cond` are notified.

`condition-notify` releases the associated mutex while waiting. This allows other threads to acquire the mutex in order to wait on the condition.

### Function: **condition-name** *cond*

Return the name of `cond`, as passed to `make-condition-variable`.

### Function: **condition-mutex** *cond*

Return the mutex associated with `cond`. Note that the associated mutex cannot be changed.
