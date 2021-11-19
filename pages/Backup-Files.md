

Next: [Auto-Saving](Auto_002dSaving.html), Up: [Backups and Auto-Saving](Backups-and-Auto_002dSaving.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 26.1 Backup Files

A *backup file* is a copy of the old contents of a file you are editing. Emacs makes a backup file the first time you save a buffer into its visited file. Thus, normally, the backup file contains the contents of the file as it was before the current editing session. The contents of the backup file normally remain unchanged once it exists.

Backups are usually made by renaming the visited file to a new name. Optionally, you can specify that backup files should be made by copying the visited file. This choice makes a difference for files with multiple names; it also can affect whether the edited file remains owned by the original owner or becomes owned by the user editing it.

By default, Emacs makes a single backup file for each file edited. You can alternatively request numbered backups; then each new backup file gets a new name. You can delete old numbered backups when you don’t want them any more, or Emacs can delete them automatically.

For performance, the operating system may not write the backup file’s contents to secondary storage immediately, or may alias the backup data with the original until one or the other is later modified. See [Files and Storage](Files-and-Storage.html).

|                                             |    |                                                        |
| :------------------------------------------ | -- | :----------------------------------------------------- |
| • [Making Backups](Making-Backups.html)     |    | How Emacs makes backup files, and when.                |
| • [Rename or Copy](Rename-or-Copy.html)     |    | Two alternatives: renaming the old file or copying it. |
| • [Numbered Backups](Numbered-Backups.html) |    | Keeping multiple backups for each source file.         |
| • [Backup Names](Backup-Names.html)         |    | How backup file names are computed; customization.     |

Next: [Auto-Saving](Auto_002dSaving.html), Up: [Backups and Auto-Saving](Backups-and-Auto_002dSaving.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
