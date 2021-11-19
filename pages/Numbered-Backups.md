

Next: [Backup Names](Backup-Names.html), Previous: [Rename or Copy](Rename-or-Copy.html), Up: [Backup Files](Backup-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 26.1.3 Making and Deleting Numbered Backup Files

If a file’s name is `foo`, the names of its numbered backup versions are `foo.~v~`, for various integers `v`, like this: `foo.~1~`, `foo.~2~`, `foo.~3~`, …, `foo.~259~`, and so on.

*   User Option: **version-control**

    This variable controls whether to make a single non-numbered backup file or multiple numbered backups.

    *   `nil`

        Make numbered backups if the visited file already has numbered backups; otherwise, do not. This is the default.

    *   `never`

        Do not make numbered backups.

    *   `anything else`

        Make numbered backups.

The use of numbered backups ultimately leads to a large number of backup versions, which must then be deleted. Emacs can do this automatically or it can ask the user whether to delete them.

*   User Option: **kept-new-versions**

    The value of this variable is the number of newest versions to keep when a new numbered backup is made. The newly made backup is included in the count. The default value is 2.

<!---->

*   User Option: **kept-old-versions**

    The value of this variable is the number of oldest versions to keep when a new numbered backup is made. The default value is 2.

If there are backups numbered 1, 2, 3, 5, and 7, and both of these variables have the value 2, then the backups numbered 1 and 2 are kept as old versions and those numbered 5 and 7 are kept as new versions; backup version 3 is excess. The function `find-backup-file-name` (see [Backup Names](Backup-Names.html)) is responsible for determining which backup versions to delete, but does not delete them itself.

*   User Option: **delete-old-versions**

    If this variable is `t`, then saving a file deletes excess backup versions silently. If it is `nil`, that means to ask for confirmation before deleting excess backups. Otherwise, they are not deleted at all.

<!---->

*   User Option: **dired-kept-versions**

    This variable specifies how many of the newest backup versions to keep in the Dired command `.` (`dired-clean-directory`). That’s the same thing `kept-new-versions` specifies when you make a new backup file. The default is 2.

Next: [Backup Names](Backup-Names.html), Previous: [Rename or Copy](Rename-or-Copy.html), Up: [Backup Files](Backup-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
