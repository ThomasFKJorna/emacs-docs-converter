

Next: [Desktop Notifications](Desktop-Notifications.html), Previous: [Batch Mode](Batch-Mode.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.18 Session Management

Emacs supports the X Session Management Protocol, which is used to suspend and restart applications. In the X Window System, a program called the *session manager* is responsible for keeping track of the applications that are running. When the X server shuts down, the session manager asks applications to save their state, and delays the actual shutdown until they respond. An application can also cancel the shutdown.

When the session manager restarts a suspended session, it directs these applications to individually reload their saved state. It does this by specifying a special command-line argument that says what saved session to restore. For Emacs, this argument is ‘`--smid session`’.

*   Variable: **emacs-save-session-functions**

    Emacs supports saving state via a hook called `emacs-save-session-functions`. Emacs runs this hook when the session manager tells it that the window system is shutting down. The functions are called with no arguments, and with the current buffer set to a temporary buffer. Each function can use `insert` to add Lisp code to this buffer. At the end, Emacs saves the buffer in a file, called the *session file*.

    Subsequently, when the session manager restarts Emacs, it loads the session file automatically (see [Loading](Loading.html)). This is performed by a function named `emacs-session-restore`, which is called during startup. See [Startup Summary](Startup-Summary.html).

    If a function in `emacs-save-session-functions` returns non-`nil`, Emacs tells the session manager to cancel the shutdown.

Here is an example that just inserts some text into `*scratch*` when Emacs is restarted by the session manager.

```lisp
(add-hook 'emacs-save-session-functions 'save-yourself-test)
```

```lisp
```

```lisp
(defun save-yourself-test ()
  (insert "(save-current-buffer
  (switch-to-buffer \"*scratch*\")
  (insert \"I am restored\"))")
  nil)
```

Next: [Desktop Notifications](Desktop-Notifications.html), Previous: [Batch Mode](Batch-Mode.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
