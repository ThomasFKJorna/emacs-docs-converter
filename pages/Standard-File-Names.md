

Previous: [File Name Completion](File-Name-Completion.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.9.7 Standard File Names

Sometimes, an Emacs Lisp program needs to specify a standard file name for a particular use—typically, to hold configuration data specified by the current user. Usually, such files should be located in the directory specified by `user-emacs-directory`, which is typically `~/.config/emacs/` or `~/.emacs.d/` by default (see [How Emacs Finds Your Init File](https://www.gnu.org/software/emacs/manual/html_node/emacs/Find-Init.html#Find-Init) in The GNU Emacs Manual). For example, abbrev definitions are stored by default in `~/.config/emacs/abbrev_defs` or `~/.emacs.d/abbrev_defs`. The easiest way to specify such a file name is to use the function `locate-user-emacs-file`.

*   Function: **locate-user-emacs-file** *base-name \&optional old-name*

    This function returns an absolute file name for an Emacs-specific configuration or data file. The argument `base-name` should be a relative file name. The return value is the absolute name of a file in the directory specified by `user-emacs-directory`; if that directory does not exist, this function creates it.

    If the optional argument `old-name` is non-`nil`, it specifies a file in the user’s home directory, `~/old-name`. If such a file exists, the return value is the absolute name of that file, instead of the file specified by `base-name`. This argument is intended to be used by Emacs packages to provide backward compatibility. For instance, prior to the introduction of `user-emacs-directory`, the abbrev file was located in `~/.abbrev_defs`. Here is the definition of `abbrev-file-name`:

    ```lisp
    (defcustom abbrev-file-name
      (locate-user-emacs-file "abbrev_defs" ".abbrev_defs")
      "Default name of file from which to read abbrevs."
      …
      :type 'file)
    ```

A lower-level function for standardizing file names, which `locate-user-emacs-file` uses as a subroutine, is `convert-standard-filename`.

*   Function: **convert-standard-filename** *filename*

    This function returns a file name based on `filename`, which fits the conventions of the current operating system.

    On GNU and other POSIX-like systems, this simply returns `filename`. On other operating systems, it may enforce system-specific file name conventions; for example, on MS-DOS this function performs a variety of changes to enforce MS-DOS file name limitations, including converting any leading ‘`.`’ to ‘`_`’ and truncating to three characters after the ‘`.`’.

    The recommended way to use this function is to specify a name which fits the conventions of GNU and Unix systems, and pass it to `convert-standard-filename`.

Previous: [File Name Completion](File-Name-Completion.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
