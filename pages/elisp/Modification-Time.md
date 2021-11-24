

### 27.6 Buffer Modification Time

Suppose that you visit a file and make changes in its buffer, and meanwhile the file itself is changed on disk. At this point, saving the buffer would overwrite the changes in the file. Occasionally this may be what you want, but usually it would lose valuable information. Emacs therefore checks the file’s modification time using the functions described below before saving the file. (See [File Attributes](File-Attributes.html), for how to examine a file’s modification time.)

### Function: **verify-visited-file-modtime** *\&optional buffer*

This function compares what `buffer` (by default, the current-buffer) has recorded for the modification time of its visited file against the actual modification time of the file as recorded by the operating system. The two should be the same unless some other process has written the file since Emacs visited or saved it.

The function returns `t` if the last actual modification time and Emacs’s recorded modification time are the same, `nil` otherwise. It also returns `t` if the buffer has no recorded last modification time, that is if `visited-file-modtime` would return zero.

It always returns `t` for buffers that are not visiting a file, even if `visited-file-modtime` returns a non-zero value. For instance, it always returns `t` for dired buffers. It returns `t` for buffers that are visiting a file that does not exist and never existed, but `nil` for file-visiting buffers whose file has been deleted.

### Function: **clear-visited-file-modtime**

This function clears out the record of the last modification time of the file being visited by the current buffer. As a result, the next attempt to save this buffer will not complain of a discrepancy in file modification times.

This function is called in `set-visited-file-name` and other exceptional places where the usual test to avoid overwriting a changed file should not be done.

### Function: **visited-file-modtime**

This function returns the current buffer’s recorded last file modification time, as a Lisp timestamp (see [Time of Day](Time-of-Day.html)).

If the buffer has no recorded last modification time, this function returns zero. This case occurs, for instance, if the buffer is not visiting a file or if the time has been explicitly cleared by `clear-visited-file-modtime`. Note, however, that `visited-file-modtime` returns a timestamp for some non-file buffers too. For instance, in a Dired buffer listing a directory, it returns the last modification time of that directory, as recorded by Dired.

If the buffer is visiting a file that doesn’t exist, this function returns -1.

### Function: **set-visited-file-modtime** *\&optional time*

This function updates the buffer’s record of the last modification time of the visited file, to the value specified by `time` if `time` is not `nil`, and otherwise to the last modification time of the visited file.

If `time` is neither `nil` nor an integer flag returned by `visited-file-modtime`, it should be a Lisp time value (see [Time of Day](Time-of-Day.html)).

This function is useful if the buffer was not read from the file normally, or if the file itself has been changed for some known benign reason.

### Function: **ask-user-about-supersession-threat** *filename*

This function is used to ask a user how to proceed after an attempt to modify a buffer visiting file `filename` when the file is newer than the buffer text. Emacs detects this because the modification time of the file on disk is newer than the last save-time and its contents have changed. This means some other program has probably altered the file.

Depending on the user’s answer, the function may return normally, in which case the modification of the buffer proceeds, or it may signal a `file-supersession` error with data `(filename)`, in which case the proposed buffer modification is not allowed.

This function is called automatically by Emacs on the proper occasions. It exists so you can customize Emacs by redefining it. See the file `userlock.el` for the standard definition.

See also the file locking mechanism in [File Locks](File-Locks.html).
