

#### 25.6.4 File Attributes

This section describes the functions for getting detailed information about a file, including the owner and group numbers, the number of names, the inode number, the size, and the times of access and modification.

### Function: **file-newer-than-file-p** *filename1 filename2*

This function returns `t` if the file `filename1` is newer than file `filename2`. If `filename1` does not exist, it returns `nil`. If `filename1` does exist, but `filename2` does not, it returns `t`.

In the following example, assume that the file `aug-19` was written on the 19th, `aug-20` was written on the 20th, and the file `no-file` doesn’t exist at all.

```lisp
(file-newer-than-file-p "aug-19" "aug-20")
     ⇒ nil
```

```lisp
(file-newer-than-file-p "aug-20" "aug-19")
     ⇒ t
```

```lisp
(file-newer-than-file-p "aug-19" "no-file")
     ⇒ t
```

```lisp
(file-newer-than-file-p "no-file" "aug-19")
     ⇒ nil
```

### Function: **file-attributes** *filename \&optional id-format*

This function returns a list of attributes of file `filename`. If the specified file does not exist, it returns `nil`. This function does not follow symbolic links. The optional parameter `id-format` specifies the preferred format of attributes UID and GID (see below)—the valid values are `'string` and `'integer`. The latter is the default, but we plan to change that, so you should specify a non-`nil` value for `id-format` if you use the returned UID or GID.

On GNU platforms when operating on a local file, this function is atomic: if the filesystem is simultaneously being changed by some other process, this function returns the file’s attributes either before or after the change. Otherwise this function is not atomic, and might return `nil` if it detects the race condition, or might return a hodgepodge of the previous and current file attributes.

Accessor functions are provided to access the elements in this list. The accessors are mentioned along with the descriptions of the elements below.

The elements of the list, in order, are:

0.  `t` for a directory, a string for a symbolic link (the name linked to), or `nil` for a text file (`file-attribute-type`).
1.  The number of names the file has (`file-attribute-link-number`). Alternate names, also known as hard links, can be created by using the `add-name-to-file` function (see [Changing Files](Changing-Files.html)).
2.  The file’s UID, normally as a string (`file-attribute-user-id`). However, if it does not correspond to a named user, the value is an integer.
3.  The file’s GID, likewise (`file-attribute-group-id`).
4.  The time of last access as a Lisp timestamp (`file-attribute-access-time`). The timestamp is in the style of `current-time` (see [Time of Day](Time-of-Day.html)) and is truncated to that of the filesystem’s timestamp resolution; for example, on some FAT-based filesystems, only the date of last access is recorded, so this time will always hold the midnight of the day of the last access.
5.  The time of last modification as a Lisp timestamp (`file-attribute-modification-time`). This is the last time when the file’s contents were modified.
6.  The time of last status change as a Lisp timestamp (`file-attribute-status-change-time`). This is the time of the last change to the file’s access mode bits, its owner and group, and other information recorded in the filesystem for the file, beyond the file’s contents.
7.  The size of the file in bytes (`file-attribute-size`).
8.  The file’s modes, as a string of ten letters or dashes, as in ‘`ls -l`’ (`file-attribute-modes`).
9.  An unspecified value, present for backward compatibility.
10. The file’s inode number (`file-attribute-inode-number`), a nonnegative integer.
11. The filesystem number of the device that the file is on `file-attribute-device-number`), an integer. This element and the file’s inode number together give enough information to distinguish any two files on the system—no two files can have the same values for both of these numbers.

For example, here are the file attributes for `files.texi`:

```lisp
(file-attributes "files.texi" 'string)
     ⇒  (nil 1 "lh" "users"
          (20614 64019 50040 152000)
          (20000 23 0 0)
          (20614 64555 902289 872000)
          122295 "-rw-rw-rw-"
          t 6473924464520138
          1014478468)
```

and here is how the result is interpreted:

*   `nil`

    is neither a directory nor a symbolic link.

*   `1`

    has only one name (the name `files.texi` in the current default directory).

*   `"lh"`

    is owned by the user with name ‘`lh`’.

*   `"users"`

    is in the group with name ‘`users`’.

*   `(20614 64019 50040 152000)`

    was last accessed on October 23, 2012, at 20:12:03.050040152 UTC.

*   `(20000 23 0 0)`

    was last modified on July 15, 2001, at 08:53:43 UTC.

*   `(20614 64555 902289 872000)`

    last had its status changed on October 23, 2012, at 20:20:59.902289872 UTC.

*   `122295`

    is 122295 bytes long. (It may not contain 122295 characters, though, if some of the bytes belong to multibyte sequences, and also if the end-of-line format is CR-LF.)

*   `"-rw-rw-rw-"`

    has a mode of read and write access for the owner, group, and world.

*   `t`

    is merely a placeholder; it carries no information.

*   `6473924464520138`

    has an inode number of 6473924464520138.

*   `1014478468`

    is on the file-system device whose number is 1014478468.

### Function: **file-nlinks** *filename*

This function returns the number of names (i.e., hard links) that file `filename` has. If the file does not exist, this function returns `nil`. Note that symbolic links have no effect on this function, because they are not considered to be names of the files they link to. This function does not follow symbolic links.

```lisp
$ ls -l foo*
-rw-rw-rw- 2 rms rms 4 Aug 19 01:27 foo
-rw-rw-rw- 2 rms rms 4 Aug 19 01:27 foo1
```

```lisp
```

```lisp
(file-nlinks "foo")
     ⇒ 2
```

```lisp
(file-nlinks "doesnt-exist")
     ⇒ nil
```
