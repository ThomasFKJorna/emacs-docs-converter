

#### 2.5.10 Mutex Type

A *mutex* is an exclusive lock that threads can own and disown, in order to synchronize between them. See [Mutexes](Mutexes.html).

Mutex objects have no read syntax. They print in hash notation, giving the name of the mutex (if it has been given a name) or its address in core:

```lisp
(make-mutex "my-mutex")
    ⇒ #<mutex my-mutex>
(make-mutex)
    ⇒ #<mutex 01c7e4e0>
```
