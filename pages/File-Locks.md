

Next: [Information about Files](Information-about-Files.html), Previous: [Writing to Files](Writing-to-Files.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 25.5 File Locks

When two users edit the same file at the same time, they are likely to interfere with each other. Emacs tries to prevent this situation from arising by recording a *file lock* when a file is being modified. Emacs can then detect the first attempt to modify a buffer visiting a file that is locked by another Emacs job, and ask the user what to do. The file lock is really a file, a symbolic link with a special name, stored in the same directory as the file you are editing. The name is constructed by prepending `.#` to the filename of the buffer. The target of the symbolic link will be of the form `user@host.pid:boot`, where `user` is replaced with the current username (from `user-login-name`), `host` with the name of the host where Emacs is running (from `system-name`), `pid` with Emacs’s process id, and `boot` with the time since the last reboot. `:boot` is omitted if the boot time is unavailable. (On file systems that do not support symbolic links, a regular file is used instead, with contents of the form `user@host.pid:boot`.)

When you access files using NFS, there may be a small probability that you and another user will both lock the same file simultaneously. If this happens, it is possible for the two users to make changes simultaneously, but Emacs will still warn the user who saves second. Also, the detection of modification of a buffer visiting a file changed on disk catches some cases of simultaneous editing; see [Modification Time](Modification-Time.html).

*   Function: **file-locked-p** *filename*

    This function returns `nil` if the file `filename` is not locked. It returns `t` if it is locked by this Emacs process, and it returns the name of the user who has locked it if it is locked by some other job.

    ```lisp
    (file-locked-p "foo")
         ⇒ nil
    ```

<!---->

*   Function: **lock-buffer** *\&optional filename*

    This function locks the file `filename`, if the current buffer is modified. The argument `filename` defaults to the current buffer’s visited file. Nothing is done if the current buffer is not visiting a file, or is not modified, or if the option `create-lockfiles` is `nil`.

<!---->

*   Function: **unlock-buffer**

    This function unlocks the file being visited in the current buffer, if the buffer is modified. If the buffer is not modified, then the file should not be locked, so this function does nothing. It also does nothing if the current buffer is not visiting a file, or is not locked.

<!---->

*   User Option: **create-lockfiles**

    If this variable is `nil`, Emacs does not lock files.

<!---->

*   Function: **ask-user-about-lock** *file other-user*

    This function is called when the user tries to modify `file`, but it is locked by another user named `other-user`. The default definition of this function asks the user to say what to do. The value this function returns determines what Emacs does next:

    *   A value of `t` says to grab the lock on the file. Then this user may edit the file and `other-user` loses the lock.

    *   A value of `nil` says to ignore the lock and let this user edit the file anyway.

    *   This function may instead signal a `file-locked` error, in which case the change that the user was about to make does not take place.

        The error message for this error looks like this:

        ```lisp
        error→ File is locked: file other-user
        ```

        where `file` is the name of the file and `other-user` is the name of the user who has locked the file.

    If you wish, you can replace the `ask-user-about-lock` function with your own version that makes the decision in another way.

Next: [Information about Files](Information-about-Files.html), Previous: [Writing to Files](Writing-to-Files.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
