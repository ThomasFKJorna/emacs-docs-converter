

#### 11.7.4 Cleaning Up from Nonlocal Exits

The `unwind-protect` construct is essential whenever you temporarily put a data structure in an inconsistent state; it permits you to make the data consistent again in the event of an error or throw. (Another more specific cleanup construct that is used only for changes in buffer contents is the atomic change group; [Atomic Changes](Atomic-Changes.html).)

### Special Form: **unwind-protect** *body-form cleanup-forms…*

`unwind-protect` executes `body-form` with a guarantee that the `cleanup-forms` will be evaluated if control leaves `body-form`, no matter how that happens. `body-form` may complete normally, or execute a `throw` out of the `unwind-protect`, or cause an error; in all cases, the `cleanup-forms` will be evaluated.

If `body-form` finishes normally, `unwind-protect` returns the value of `body-form`, after it evaluates the `cleanup-forms`. If `body-form` does not finish, `unwind-protect` does not return any value in the normal sense.

Only `body-form` is protected by the `unwind-protect`. If any of the `cleanup-forms` themselves exits nonlocally (via a `throw` or an error), `unwind-protect` is *not* guaranteed to evaluate the rest of them. If the failure of one of the `cleanup-forms` has the potential to cause trouble, then protect it with another `unwind-protect` around that form.

The number of currently active `unwind-protect` forms counts, together with the number of local variable bindings, against the limit `max-specpdl-size` (see [Local Variables](Local-Variables.html#Definition-of-max_002dspecpdl_002dsize)).

For example, here we make an invisible buffer for temporary use, and make sure to kill it before finishing:

```lisp
(let ((buffer (get-buffer-create " *temp*")))
  (with-current-buffer buffer
    (unwind-protect
        body-form
      (kill-buffer buffer))))
```

You might think that we could just as well write `(kill-buffer (current-buffer))` and dispense with the variable `buffer`. However, the way shown above is safer, if `body-form` happens to get an error after switching to a different buffer! (Alternatively, you could write a `save-current-buffer` around `body-form`, to ensure that the temporary buffer becomes current again in time to kill it.)

Emacs includes a standard macro called `with-temp-buffer` which expands into more or less the code shown above (see [Current Buffer](Current-Buffer.html#Definition-of-with_002dtemp_002dbuffer)). Several of the macros defined in this manual use `unwind-protect` in this way.

Here is an actual example derived from an FTP package. It creates a process (see [Processes](Processes.html)) to try to establish a connection to a remote machine. As the function `ftp-login` is highly susceptible to numerous problems that the writer of the function cannot anticipate, it is protected with a form that guarantees deletion of the process in the event of failure. Otherwise, Emacs might fill up with useless subprocesses.

```lisp
(let ((win nil))
  (unwind-protect
      (progn
        (setq process (ftp-setup-buffer host file))
        (if (setq win (ftp-login process host user password))
            (message "Logged in")
          (error "Ftp login failed")))
    (or win (and process (delete-process process)))))
```

This example has a small bug: if the user types `C-g` to quit, and the quit happens immediately after the function `ftp-setup-buffer` returns but before the variable `process` is set, the process will not be killed. There is no easy way to fix this bug, but at least it is very unlikely.

***
