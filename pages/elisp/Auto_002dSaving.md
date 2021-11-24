

### 26.2 Auto-Saving

Emacs periodically saves all files that you are visiting; this is called *auto-saving*. Auto-saving prevents you from losing more than a limited amount of work if the system crashes. By default, auto-saves happen every 300 keystrokes, or after around 30 seconds of idle time. See [Auto-Saving: Protection Against Disasters](https://www.gnu.org/software/emacs/manual/html_node/emacs/Auto-Save.html#Auto-Save) in The GNU Emacs Manual, for information on auto-save for users. Here we describe the functions used to implement auto-saving and the variables that control them.

### Variable: **buffer-auto-save-file-name**

This buffer-local variable is the name of the file used for auto-saving the current buffer. It is `nil` if the buffer should not be auto-saved.

```lisp
buffer-auto-save-file-name
     ⇒ "/xcssun/users/rms/lewis/#backups.texi#"
```

### Command: **auto-save-mode** *arg*

This is the mode command for Auto Save mode, a buffer-local minor mode. When Auto Save mode is enabled, auto-saving is enabled in the buffer. The calling convention is the same as for other minor mode commands (see [Minor Mode Conventions](Minor-Mode-Conventions.html)).

Unlike most minor modes, there is no `auto-save-mode` variable. Auto Save mode is enabled if `buffer-auto-save-file-name` is non-`nil` and `buffer-saved-size` (see below) is non-zero.

### Function: **auto-save-file-name-p** *filename*

This function returns a non-`nil` value if `filename` is a string that could be the name of an auto-save file. It assumes the usual naming convention for auto-save files: a name that begins and ends with hash marks (‘`#`’) is a possible auto-save file name. The argument `filename` should not contain a directory part.

```lisp
(make-auto-save-file-name)
     ⇒ "/xcssun/users/rms/lewis/#backups.texi#"
```

```lisp
(auto-save-file-name-p "#backups.texi#")
     ⇒ 0
```

```lisp
(auto-save-file-name-p "backups.texi")
     ⇒ nil
```

The standard definition of this function is as follows:

```lisp
(defun auto-save-file-name-p (filename)
  "Return non-nil if FILENAME can be yielded by..."
  (string-match "^#.*#$" filename))
```

This function exists so that you can customize it if you wish to change the naming convention for auto-save files. If you redefine it, be sure to redefine the function `make-auto-save-file-name` correspondingly.

### Function: **make-auto-save-file-name**

This function returns the file name to use for auto-saving the current buffer. This is just the file name with hash marks (‘`#`’) prepended and appended to it. This function does not look at the variable `auto-save-visited-file-name` (described below); callers of this function should check that variable first.

```lisp
(make-auto-save-file-name)
     ⇒ "/xcssun/users/rms/lewis/#backups.texi#"
```

Here is a simplified version of the standard definition of this function:

```lisp
(defun make-auto-save-file-name ()
  "Return file name to use for auto-saves \
of current buffer.."
  (if buffer-file-name
```

```lisp
      (concat
       (file-name-directory buffer-file-name)
       "#"
       (file-name-nondirectory buffer-file-name)
       "#")
    (expand-file-name
     (concat "#%" (buffer-name) "#"))))
```

This exists as a separate function so that you can redefine it to customize the naming convention for auto-save files. Be sure to change `auto-save-file-name-p` in a corresponding way.

### User Option: **auto-save-visited-file-name**

If this variable is non-`nil`, Emacs auto-saves buffers in the files they are visiting. That is, the auto-save is done in the same file that you are editing. Normally, this variable is `nil`, so auto-save files have distinct names that are created by `make-auto-save-file-name`.

When you change the value of this variable, the new value does not take effect in an existing buffer until the next time auto-save mode is reenabled in it. If auto-save mode is already enabled, auto-saves continue to go in the same file name until `auto-save-mode` is called again.

Note that setting this variable to a non-`nil` value does not change the fact that auto-saving is different from saving the buffer; e.g., the hooks described in [Saving Buffers](Saving-Buffers.html) are *not* run when a buffer is auto-saved.

### Function: **recent-auto-save-p**

This function returns `t` if the current buffer has been auto-saved since the last time it was read in or saved.

### Function: **set-buffer-auto-saved**

This function marks the current buffer as auto-saved. The buffer will not be auto-saved again until the buffer text is changed again. The function returns `nil`.

### User Option: **auto-save-interval**

The value of this variable specifies how often to do auto-saving, in terms of number of input events. Each time this many additional input events are read, Emacs does auto-saving for all buffers in which that is enabled. Setting this to zero disables autosaving based on the number of characters typed.

### User Option: **auto-save-timeout**

The value of this variable is the number of seconds of idle time that should cause auto-saving. Each time the user pauses for this long, Emacs does auto-saving for all buffers in which that is enabled. (If the current buffer is large, the specified timeout is multiplied by a factor that increases as the size increases; for a million-byte buffer, the factor is almost 4.)

If the value is zero or `nil`, then auto-saving is not done as a result of idleness, only after a certain number of input events as specified by `auto-save-interval`.

### Variable: **auto-save-hook**

This normal hook is run whenever an auto-save is about to happen.

### User Option: **auto-save-default**

If this variable is non-`nil`, buffers that are visiting files have auto-saving enabled by default. Otherwise, they do not.

### Command: **do-auto-save** *\&optional no-message current-only*

This function auto-saves all buffers that need to be auto-saved. It saves all buffers for which auto-saving is enabled and that have been changed since the previous auto-save.

If any buffers are auto-saved, `do-auto-save` normally displays a message saying ‘`Auto-saving...`’ in the echo area while auto-saving is going on. However, if `no-message` is non-`nil`, the message is inhibited.

If `current-only` is non-`nil`, only the current buffer is auto-saved.

### Function: **delete-auto-save-file-if-necessary** *\&optional force*

This function deletes the current buffer’s auto-save file if `delete-auto-save-files` is non-`nil`. It is called every time a buffer is saved.

Unless `force` is non-`nil`, this function only deletes the file if it was written by the current Emacs session since the last true save.

### User Option: **delete-auto-save-files**

This variable is used by the function `delete-auto-save-file-if-necessary`. If it is non-`nil`, Emacs deletes auto-save files when a true save is done (in the visited file). This saves disk space and unclutters your directory.

### Function: **rename-auto-save-file**

This function adjusts the current buffer’s auto-save file name if the visited file name has changed. It also renames an existing auto-save file, if it was made in the current Emacs session. If the visited file name has not changed, this function does nothing.

### Variable: **buffer-saved-size**

The value of this buffer-local variable is the length of the current buffer, when it was last read in, saved, or auto-saved. This is used to detect a substantial decrease in size, and turn off auto-saving in response.

If it is -1, that means auto-saving is temporarily shut off in this buffer due to a substantial decrease in size. Explicitly saving the buffer stores a positive value in this variable, thus reenabling auto-saving. Turning auto-save mode off or on also updates this variable, so that the substantial decrease in size is forgotten.

If it is -2, that means this buffer should disregard changes in buffer size; in particular, it should not shut off auto-saving temporarily due to changes in buffer size.

### Variable: **auto-save-list-file-name**

This variable (if non-`nil`) specifies a file for recording the names of all the auto-save files. Each time Emacs does auto-saving, it writes two lines into this file for each buffer that has auto-saving enabled. The first line gives the name of the visited file (it’s empty if the buffer has none), and the second gives the name of the auto-save file.

When Emacs exits normally, it deletes this file; if Emacs crashes, you can look in the file to find all the auto-save files that might contain work that was otherwise lost. The `recover-session` command uses this file to find them.

The default name for this file specifies your home directory and starts with ‘`.saves-`’. It also contains the Emacs process ID and the host name.

### User Option: **auto-save-list-file-prefix**

After Emacs reads your init file, it initializes `auto-save-list-file-name` (if you have not already set it non-`nil`) based on this prefix, adding the host name and process ID. If you set this to `nil` in your init file, then Emacs does not initialize `auto-save-list-file-name`.
