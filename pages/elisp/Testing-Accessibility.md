

#### 25.6.1 Testing Accessibility

These functions test for permission to access a file for reading, writing, or execution. Unless explicitly stated otherwise, they follow symbolic links. See [Kinds of Files](Kinds-of-Files.html).

On some operating systems, more complex sets of access permissions can be specified, via mechanisms such as Access Control Lists (ACLs). See [Extended Attributes](Extended-Attributes.html), for how to query and set those permissions.

### Function: **file-exists-p** *filename*

This function returns `t` if a file named `filename` appears to exist. This does not mean you can necessarily read the file, only that you can probably find out its attributes. (On GNU and other POSIX-like systems, this is true if the file exists and you have execute permission on the containing directories, regardless of the permissions of the file itself.)

If the file does not exist, or if there was trouble determining whether the file exists, this function returns `nil`.

Directories are files, so `file-exists-p` can return `t` when given a directory. However, because `file-exists-p` follows symbolic links, it returns `t` for a symbolic link name only if the target file exists.

### Function: **file-readable-p** *filename*

This function returns `t` if a file named `filename` exists and you can read it. It returns `nil` otherwise.

### Function: **file-executable-p** *filename*

This function returns `t` if a file named `filename` exists and you can execute it. It returns `nil` otherwise. On GNU and other POSIX-like systems, if the file is a directory, execute permission means you can check the existence and attributes of files inside the directory, and open those files if their modes permit.

### Function: **file-writable-p** *filename*

This function returns `t` if the file `filename` can be written or created by you, and `nil` otherwise. A file is writable if the file exists and you can write it. It is creatable if it does not exist, but its parent directory does exist and you can write in that directory.

In the example below, `foo` is not writable because the parent directory does not exist, even though the user could create such a directory.

```lisp
(file-writable-p "~/no-such-dir/foo")
     ⇒ nil
```

### Function: **file-accessible-directory-p** *dirname*

This function returns `t` if you have permission to open existing files in the directory whose name as a file is `dirname`; otherwise (e.g., if there is no such directory), it returns `nil`. The value of `dirname` may be either a directory name (such as `/foo/`) or the file name of a file which is a directory (such as `/foo`, without the final slash).

For example, from the following we deduce that any attempt to read a file in `/foo/` will give an error:

```lisp
(file-accessible-directory-p "/foo")
     ⇒ nil
```

### Function: **access-file** *filename string*

If you can read `filename` this function returns `nil`; otherwise it signals an error using `string` as the error message text.

### Function: **file-ownership-preserved-p** *filename \&optional group*

This function returns `t` if deleting the file `filename` and then creating it anew would keep the file’s owner unchanged. It also returns `t` for nonexistent files.

If the optional argument `group` is non-`nil`, this function also checks that the file’s group would be unchanged.

This function does not follow symbolic links.

### Function: **file-modes** *filename*

This function returns the *mode bits* of `filename`—an integer summarizing its read, write, and execution permissions. This function follows symbolic links. If the file does not exist, the return value is `nil`.

See [File permissions](https://www.gnu.org/software/coreutils/manual/html_node/File-permissions.html#File-permissions) in The GNU `Coreutils` Manual, for a description of mode bits. For example, if the low-order bit is 1, the file is executable by all users; if the second-lowest-order bit is 1, the file is writable by all users; etc. The highest possible value is 4095 (7777 octal), meaning that everyone has read, write, and execute permission, the SUID bit is set for both others and group, and the sticky bit is set.

See [Changing Files](Changing-Files.html), for the `set-file-modes` function, which can be used to set these permissions.

```lisp
(file-modes "~/junk/diffs")
     ⇒ 492               ; Decimal integer.
```

```lisp
(format "%o" 492)
     ⇒ "754"             ; Convert to octal.
```

```lisp
```

```lisp
(set-file-modes "~/junk/diffs" #o666)
     ⇒ nil
```

```lisp
```

```lisp
$ ls -l diffs
-rw-rw-rw- 1 lewis lewis 3063 Oct 30 16:00 diffs
```

**MS-DOS note:** On MS-DOS, there is no such thing as an executable file mode bit. So `file-modes` considers a file executable if its name ends in one of the standard executable extensions, such as `.com`, `.bat`, `.exe`, and some others. Files that begin with the POSIX-standard ‘`#!`’ signature, such as shell and Perl scripts, are also considered executable. Directories are also reported as executable, for compatibility with POSIX. These conventions are also followed by `file-attributes` (see [File Attributes](File-Attributes.html)).
