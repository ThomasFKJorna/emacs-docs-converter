

Next: [Stream Type](Stream-Type.html), Previous: [Mutex Type](Mutex-Type.html), Up: [Editing Types](Editing-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.5.11 Condition Variable Type

A *condition variable* is a device for a more complex thread synchronization than the one supported by a mutex. A thread can wait on a condition variable, to be woken up when some other thread notifies the condition.

Condition variable objects have no read syntax. They print in hash notation, giving the name of the condition variable (if it has been given a name) or its address in core:

```lisp
(make-condition-variable (make-mutex))
    ⇒ #<condvar 01c45ae8>
```
