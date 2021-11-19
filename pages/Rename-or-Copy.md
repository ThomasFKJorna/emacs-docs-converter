

Next: [Numbered Backups](Numbered-Backups.html), Previous: [Making Backups](Making-Backups.html), Up: [Backup Files](Backup-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 26.1.2 Backup by Renaming or by Copying?

There are two ways that Emacs can make a backup file:

*   Emacs can rename the original file so that it becomes a backup file, and then write the buffer being saved into a new file. After this procedure, any other names (i.e., hard links) of the original file now refer to the backup file. The new file is owned by the user doing the editing, and its group is the default for new files written by the user in that directory.
*   Emacs can copy the original file into a backup file, and then overwrite the original file with new contents. After this procedure, any other names (i.e., hard links) of the original file continue to refer to the current (updated) version of the file. The file’s owner and group will be unchanged.

The first method, renaming, is the default.

The variable `backup-by-copying`, if non-`nil`, says to use the second method, which is to copy the original file and overwrite it with the new buffer contents. The variable `file-precious-flag`, if non-`nil`, also has this effect (as a sideline of its main significance). See [Saving Buffers](Saving-Buffers.html).

*   User Option: **backup-by-copying**

    If this variable is non-`nil`, Emacs always makes backup files by copying. The default is `nil`.

The following three variables, when non-`nil`, cause the second method to be used in certain special cases. They have no effect on the treatment of files that don’t fall into the special cases.

*   User Option: **backup-by-copying-when-linked**

    If this variable is non-`nil`, Emacs makes backups by copying for files with multiple names (hard links). The default is `nil`.

    This variable is significant only if `backup-by-copying` is `nil`, since copying is always used when that variable is non-`nil`.

<!---->

*   User Option: **backup-by-copying-when-mismatch**

    If this variable is non-`nil` (the default), Emacs makes backups by copying in cases where renaming would change either the owner or the group of the file.

    The value has no effect when renaming would not alter the owner or group of the file; that is, for files which are owned by the user and whose group matches the default for a new file created there by the user.

    This variable is significant only if `backup-by-copying` is `nil`, since copying is always used when that variable is non-`nil`.

<!---->

*   User Option: **backup-by-copying-when-privileged-mismatch**

    This variable, if non-`nil`, specifies the same behavior as `backup-by-copying-when-mismatch`, but only for certain user-id and group-id values: namely, those less than or equal to a certain number. You set this variable to that number.

    Thus, if you set `backup-by-copying-when-privileged-mismatch` to 0, backup by copying is done for the superuser and group 0 only, when necessary to prevent a change in the owner of the file.

    The default is 200.

Next: [Numbered Backups](Numbered-Backups.html), Previous: [Making Backups](Making-Backups.html), Up: [Backup Files](Backup-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
