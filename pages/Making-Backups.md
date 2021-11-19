

Next: [Rename or Copy](Rename-or-Copy.html), Up: [Backup Files](Backup-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 26.1.1 Making Backup Files

*   Function: **backup-buffer**

    This function makes a backup of the file visited by the current buffer, if appropriate. It is called by `save-buffer` before saving the buffer the first time.

    If a backup was made by renaming, the return value is a cons cell of the form (`modes` `extra-alist` `backupname`), where `modes` are the mode bits of the original file, as returned by `file-modes` (see [Testing Accessibility](Testing-Accessibility.html)), `extra-alist` is an alist describing the original file’s extended attributes, as returned by `file-extended-attributes` (see [Extended Attributes](Extended-Attributes.html)), and `backupname` is the name of the backup.

    In all other cases (i.e., if a backup was made by copying or if no backup was made), this function returns `nil`.

<!---->

*   Variable: **buffer-backed-up**

    This buffer-local variable says whether this buffer’s file has been backed up on account of this buffer. If it is non-`nil`, the backup file has been written. Otherwise, the file should be backed up when it is next saved (if backups are enabled). This is a permanent local; `kill-all-local-variables` does not alter it.

<!---->

*   User Option: **make-backup-files**

    This variable determines whether or not to make backup files. If it is non-`nil`, then Emacs creates a backup of each file when it is saved for the first time—provided that `backup-inhibited` is `nil` (see below).

    The following example shows how to change the `make-backup-files` variable only in the Rmail buffers and not elsewhere. Setting it `nil` stops Emacs from making backups of these files, which may save disk space. (You would put this code in your init file.)

    ```lisp
    (add-hook 'rmail-mode-hook
              (lambda () (setq-local make-backup-files nil)))
    ```

<!---->

*   Variable: **backup-enable-predicate**

    This variable’s value is a function to be called on certain occasions to decide whether a file should have backup files. The function receives one argument, an absolute file name to consider. If the function returns `nil`, backups are disabled for that file. Otherwise, the other variables in this section say whether and how to make backups.

    The default value is `normal-backup-enable-predicate`, which checks for files in `temporary-file-directory` and `small-temporary-file-directory`.

<!---->

*   Variable: **backup-inhibited**

    If this variable is non-`nil`, backups are inhibited. It records the result of testing `backup-enable-predicate` on the visited file name. It can also coherently be used by other mechanisms that inhibit backups based on which file is visited. For example, VC sets this variable non-`nil` to prevent making backups for files managed with a version control system.

    This is a permanent local, so that changing the major mode does not lose its value. Major modes should not set this variable—they should set `make-backup-files` instead.

<!---->

*   User Option: **backup-directory-alist**

    This variable’s value is an alist of filename patterns and backup directories. Each element looks like

    ```lisp
    (regexp . directory)
    ```

    Backups of files with names matching `regexp` will be made in `directory`. `directory` may be relative or absolute. If it is absolute, so that all matching files are backed up into the same directory, the file names in this directory will be the full name of the file backed up with all directory separators changed to ‘`!`’ to prevent clashes. This will not work correctly if your filesystem truncates the resulting name.

    For the common case of all backups going into one directory, the alist should contain a single element pairing ‘`"."`’ with the appropriate directory.

    If this variable is `nil` (the default), or it fails to match a filename, the backup is made in the original file’s directory.

    On MS-DOS filesystems without long names this variable is always ignored.

<!---->

*   User Option: **make-backup-file-name-function**

    This variable’s value is a function to use for making backup file names. The function `make-backup-file-name` calls it. See [Naming Backup Files](Backup-Names.html).

    This could be buffer-local to do something special for specific files. If you change it, you may need to change `backup-file-name-p` and `file-name-sans-versions` too.

Next: [Rename or Copy](Rename-or-Copy.html), Up: [Backup Files](Backup-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
