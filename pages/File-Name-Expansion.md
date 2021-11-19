

Next: [Unique File Names](Unique-File-Names.html), Previous: [Directory Names](Directory-Names.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.9.4 Functions that Expand Filenames

*Expanding* a file name means converting a relative file name to an absolute one. Since this is done relative to a default directory, you must specify the default directory as well as the file name to be expanded. It also involves expanding abbreviations like `~/` (see [abbreviate-file-name](Directory-Names.html#abbreviate_002dfile_002dname)), and eliminating redundancies like `./` and `name/../`.

*   Function: **expand-file-name** *filename \&optional directory*

    This function converts `filename` to an absolute file name. If `directory` is supplied, it is the default directory to start with if `filename` is relative and does not start with ‘`~`’. (The value of `directory` should itself be an absolute directory name or directory file name; it may start with ‘`~`’.) Otherwise, the current buffer’s value of `default-directory` is used. For example:

    ```lisp
    (expand-file-name "foo")
         ⇒ "/xcssun/users/rms/lewis/foo"
    ```

    ```lisp
    (expand-file-name "../foo")
         ⇒ "/xcssun/users/rms/foo"
    ```

    ```lisp
    (expand-file-name "foo" "/usr/spool/")
         ⇒ "/usr/spool/foo"
    ```

    If the part of `filename` before the first slash is ‘`~`’, it expands to your home directory, which is typically specified by the value of the `HOME` environment variable (see [General Variables](https://www.gnu.org/software/emacs/manual/html_node/emacs/General-Variables.html#General-Variables) in The GNU Emacs Manual). If the part before the first slash is ‘`~user`’ and if `user` is a valid login name, it expands to `user`’s home directory. If you do not want this expansion for a relative `filename` that might begin with a literal ‘`~`’, you can use `(concat (file-name-as-directory directory) filename)` instead of `(expand-file-name filename directory)`.

    Filenames containing ‘`.`’ or ‘`..`’ are simplified to their canonical form:

    ```lisp
    (expand-file-name "bar/../foo")
         ⇒ "/xcssun/users/rms/lewis/foo"
    ```

    In some cases, a leading ‘`..`’ component can remain in the output:

    ```lisp
    (expand-file-name "../home" "/")
         ⇒ "/../home"
    ```

    This is for the sake of filesystems that have the concept of a superroot above the root directory `/`. On other filesystems, `/../` is interpreted exactly the same as `/`.

    Expanding `.` or the empty string returns the default directory:

    ```lisp
    (expand-file-name "." "/usr/spool/")
         ⇒ "/usr/spool"
    (expand-file-name "" "/usr/spool/")
         ⇒ "/usr/spool"
    ```

    Note that `expand-file-name` does *not* expand environment variables; only `substitute-in-file-name` does that:

    ```lisp
    (expand-file-name "$HOME/foo")
         ⇒ "/xcssun/users/rms/lewis/$HOME/foo"
    ```

    Note also that `expand-file-name` does not follow symbolic links at any level. This results in a difference between the way `file-truename` and `expand-file-name` treat ‘`..`’. Assuming that ‘`/tmp/bar`’ is a symbolic link to the directory ‘`/tmp/foo/bar`’ we get:

    ```lisp
    (file-truename "/tmp/bar/../myfile")
         ⇒ "/tmp/foo/myfile"
    ```

    ```lisp
    (expand-file-name "/tmp/bar/../myfile")
         ⇒ "/tmp/myfile"
    ```

    If you may need to follow symbolic links preceding ‘`..`’, you should make sure to call `file-truename` without prior direct or indirect calls to `expand-file-name`. See [Truenames](Truenames.html).

<!---->

*   Variable: **default-directory**

    The value of this buffer-local variable is the default directory for the current buffer. It should be an absolute directory name; it may start with ‘`~`’. This variable is buffer-local in every buffer.

    `expand-file-name` uses the default directory when its second argument is `nil`.

    The value is always a string ending with a slash.

    ```lisp
    default-directory
         ⇒ "/user/lewis/manual/"
    ```

<!---->

*   Function: **substitute-in-file-name** *filename*

    This function replaces environment variable references in `filename` with the environment variable values. Following standard Unix shell syntax, ‘`$`’ is the prefix to substitute an environment variable value. If the input contains ‘`$$`’, that is converted to ‘`$`’; this gives the user a way to quote a ‘`$`’.

    The environment variable name is the series of alphanumeric characters (including underscores) that follow the ‘`$`’. If the character following the ‘`$`’ is a ‘`{`’, then the variable name is everything up to the matching ‘`}`’.

    Calling `substitute-in-file-name` on output produced by `substitute-in-file-name` tends to give incorrect results. For instance, use of ‘`$$`’ to quote a single ‘`$`’ won’t work properly, and ‘`$`’ in an environment variable’s value could lead to repeated substitution. Therefore, programs that call this function and put the output where it will be passed to this function need to double all ‘`$`’ characters to prevent subsequent incorrect results.

    Here we assume that the environment variable `HOME`, which holds the user’s home directory, has value ‘`/xcssun/users/rms`’.

    ```lisp
    (substitute-in-file-name "$HOME/foo")
         ⇒ "/xcssun/users/rms/foo"
    ```

    After substitution, if a ‘`~`’ or a ‘`/`’ appears immediately after another ‘`/`’, the function discards everything before it (up through the immediately preceding ‘`/`’).

    ```lisp
    (substitute-in-file-name "bar/~/foo")
         ⇒ "~/foo"
    ```

    ```lisp
    (substitute-in-file-name "/usr/local/$HOME/foo")
         ⇒ "/xcssun/users/rms/foo"
         ;; /usr/local/ has been discarded.
    ```

Sometimes, it is not desired to expand file names. In such cases, the file name can be quoted to suppress the expansion, and to handle the file name literally. Quoting happens by prefixing the file name with ‘`/:`’.

*   Macro: **file-name-quote** *name*

    This macro adds the quotation prefix ‘`/:`’ to the file `name`. For a local file `name`, it prefixes `name` with ‘`/:`’. If `name` is a remote file name, the local part of `name` (see [Magic File Names](Magic-File-Names.html)) is quoted. If `name` is already a quoted file name, `name` is returned unchanged.

    ```lisp
    (substitute-in-file-name (file-name-quote "bar/~/foo"))
         ⇒ "/:bar/~/foo"
    ```

    ```lisp
    ```

    ```lisp
    (substitute-in-file-name (file-name-quote "/ssh:host:bar/~/foo"))
         ⇒ "/ssh:host:/:bar/~/foo"
    ```

    The macro cannot be used to suppress file name handlers from magic file names (see [Magic File Names](Magic-File-Names.html)).

<!---->

*   Macro: **file-name-unquote** *name*

    This macro removes the quotation prefix ‘`/:`’ from the file `name`, if any. If `name` is a remote file name, the local part of `name` is unquoted.

<!---->

*   Macro: **file-name-quoted-p** *name*

    This macro returns non-`nil`, when `name` is quoted with the prefix ‘`/:`’. If `name` is a remote file name, the local part of `name` is checked.

Next: [Unique File Names](Unique-File-Names.html), Previous: [Directory Names](Directory-Names.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
