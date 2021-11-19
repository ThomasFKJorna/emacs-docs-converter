

Next: [Dynamic Libraries](Dynamic-Libraries.html), Previous: [Desktop Notifications](Desktop-Notifications.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.20 Notifications on File Changes

Several operating systems support watching of filesystems for changes of files. If configured properly, Emacs links a respective library like `inotify`, `kqueue`, `gfilenotify`, or `w32notify` statically. These libraries enable watching of filesystems on the local machine.

It is also possible to watch filesystems on remote machines, see [Remote Files](https://www.gnu.org/software/emacs/manual/html_node/emacs/Remote-Files.html#Remote-Files) in The GNU Emacs Manual This does not depend on one of the libraries linked to Emacs.

Since all these libraries emit different events on notified file changes, there is the Emacs library `filenotify` which provides a unified interface. Lisp programs that want to receive file notifications should always use this library in preference to the native ones.

*   Function: **file-notify-add-watch** *file flags callback*

    Add a watch for filesystem events pertaining to `file`. This arranges for filesystem events pertaining to `file` to be reported to Emacs.

    The returned value is a descriptor for the added watch. Its type depends on the underlying library, it cannot be assumed to be an integer as in the example below. It should be used for comparison by `equal` only.

    If the `file` cannot be watched for some reason, this function signals a `file-notify-error` error.

    Sometimes, mounted filesystems cannot be watched for file changes. This is not detected by this function, a non-`nil` return value does not guarantee that changes on `file` will be notified.

    `flags` is a list of conditions to set what will be watched for. It can include the following symbols:

    *   `change`

        watch for file changes

    *   `attribute-change`

        watch for file attribute changes, like permissions or modification time

    If `file` is a directory, changes for all files in that directory will be notified. This does not work recursively.

    When any event happens, Emacs will call the `callback` function passing it a single argument `event`, which is of the form

    ```lisp
    (descriptor action file [file1])
    ```

    `descriptor` is the same object as the one returned by this function. `action` is the description of the event. It could be any one of the following symbols:

    *   `created`

        `file` was created

    *   `deleted`

        `file` was deleted

    *   `changed`

        `file`’s contents has changed; with `w32notify` library, reports attribute changes as well

    *   `renamed`

        `file` has been renamed to `file1`

    *   `attribute-changed`

        a `file` attribute was changed

    *   `stopped`

        watching `file` has been stopped

    Note that the `w32notify` library does not report `attribute-changed` events. When some file’s attribute, like permissions or modification time, has changed, this library reports a `changed` event. Likewise, the `kqueue` library does not report reliably file attribute changes when watching a directory.

    The `stopped` event reports, that watching the file has been stopped. This could be because `file-notify-rm-watch` was called (see below), or because the file being watched was deleted, or due to another error reported from the underlying library.

    `file` and `file1` are the name of the file(s) whose event is being reported. For example:

    ```lisp
    (require 'filenotify)
         ⇒ filenotify
    ```

    ```lisp
    ```

    ```lisp
    (defun my-notify-callback (event)
      (message "Event %S" event))
         ⇒ my-notify-callback
    ```

    ```lisp
    ```

    ```lisp
    (file-notify-add-watch
      "/tmp" '(change attribute-change) 'my-notify-callback)
         ⇒ 35025468
    ```

    ```lisp
    ```

    ```lisp
    (write-region "foo" nil "/tmp/foo")
         ⇒ Event (35025468 created "/tmp/.#foo")
            Event (35025468 created "/tmp/foo")
            Event (35025468 changed "/tmp/foo")
            Event (35025468 deleted "/tmp/.#foo")
    ```

    ```lisp
    ```

    ```lisp
    (write-region "bla" nil "/tmp/foo")
         ⇒ Event (35025468 created "/tmp/.#foo")
            Event (35025468 changed "/tmp/foo")
            Event (35025468 deleted "/tmp/.#foo")
    ```

    ```lisp
    ```

    ```lisp
    (set-file-modes "/tmp/foo" (default-file-modes))
         ⇒ Event (35025468 attribute-changed "/tmp/foo")
    ```

    Whether the action `renamed` is returned, depends on the used watch library. Otherwise, the actions `deleted` and `created` could be returned in a random order.

    ```lisp
    (rename-file "/tmp/foo" "/tmp/bla")
         ⇒ Event (35025468 renamed "/tmp/foo" "/tmp/bla")
    ```

    ```lisp
    ```

    ```lisp
    (delete-file "/tmp/bla")
         ⇒ Event (35025468 deleted "/tmp/bla")
    ```

<!---->

*   Function: **file-notify-rm-watch** *descriptor*

    Removes an existing file watch specified by its `descriptor`. `descriptor` should be an object returned by `file-notify-add-watch`.

<!---->

*   Function: **file-notify-valid-p** *descriptor*

    Checks a watch specified by its `descriptor` for validity. `descriptor` should be an object returned by `file-notify-add-watch`.

    A watch can become invalid if the file or directory it watches is deleted, or if the watcher thread exits abnormally for any other reason. Removing the watch by calling `file-notify-rm-watch` also makes it invalid.

    ```lisp
    (make-directory "/tmp/foo")
         ⇒ Event (35025468 created "/tmp/foo")
    ```

    ```lisp
    ```

    ```lisp
    (setq desc
          (file-notify-add-watch
            "/tmp/foo" '(change) 'my-notify-callback))
         ⇒ 11359632
    ```

    ```lisp
    ```

    ```lisp
    (file-notify-valid-p desc)
         ⇒ t
    ```

    ```lisp
    ```

    ```lisp
    (write-region "bla" nil "/tmp/foo/bla")
         ⇒ Event (11359632 created "/tmp/foo/.#bla")
            Event (11359632 created "/tmp/foo/bla")
            Event (11359632 changed "/tmp/foo/bla")
            Event (11359632 deleted "/tmp/foo/.#bla")
    ```

    ```lisp
    ```

    ```lisp
    ;; Deleting a file in the directory doesn't invalidate the watch.
    (delete-file "/tmp/foo/bla")
         ⇒ Event (11359632 deleted "/tmp/foo/bla")
    ```

    ```lisp
    ```

    ```lisp
    (write-region "bla" nil "/tmp/foo/bla")
         ⇒ Event (11359632 created "/tmp/foo/.#bla")
            Event (11359632 created "/tmp/foo/bla")
            Event (11359632 changed "/tmp/foo/bla")
            Event (11359632 deleted "/tmp/foo/.#bla")
    ```

    ```lisp
    ```

    ```lisp
    ;; Deleting the directory invalidates the watch.
    ;; Events arrive for different watch descriptors.
    (delete-directory "/tmp/foo" 'recursive)
         ⇒ Event (35025468 deleted "/tmp/foo")
            Event (11359632 deleted "/tmp/foo/bla")
            Event (11359632 deleted "/tmp/foo")
            Event (11359632 stopped "/tmp/foo")
    ```

    ```lisp
    ```

    ```lisp
    (file-notify-valid-p desc)
         ⇒ nil
    ```

Next: [Dynamic Libraries](Dynamic-Libraries.html), Previous: [Desktop Notifications](Desktop-Notifications.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
