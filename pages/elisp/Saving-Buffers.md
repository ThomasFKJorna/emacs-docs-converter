

### 25.2 Saving Buffers

When you edit a file in Emacs, you are actually working on a buffer that is visiting that file—that is, the contents of the file are copied into the buffer and the copy is what you edit. Changes to the buffer do not change the file until you *save* the buffer, which means copying the contents of the buffer into the file. Buffers which are not visiting a file can still be “saved”, in a sense, using functions in the buffer-local `write-contents-functions` hook.

### Command: **save-buffer** *\&optional backup-option*

This function saves the contents of the current buffer in its visited file if the buffer has been modified since it was last visited or saved. Otherwise it does nothing.

`save-buffer` is responsible for making backup files. Normally, `backup-option` is `nil`, and `save-buffer` makes a backup file only if this is the first save since visiting the file. Other values for `backup-option` request the making of backup files in other circumstances:

*   With an argument of 4 or 64, reflecting 1 or 3 `C-u`’s, the `save-buffer` function marks this version of the file to be backed up when the buffer is next saved.
*   With an argument of 16 or 64, reflecting 2 or 3 `C-u`’s, the `save-buffer` function unconditionally backs up the previous version of the file before saving it.
*   With an argument of 0, unconditionally do *not* make any backup file.

### Command: **save-some-buffers** *\&optional save-silently-p pred*

This command saves some modified file-visiting buffers. Normally it asks the user about each buffer. But if `save-silently-p` is non-`nil`, it saves all the file-visiting buffers without querying the user.

The optional `pred` argument provides a predicate that controls which buffers to ask about (or to save silently if `save-silently-p` is non-`nil`). If `pred` is `nil`, that means to use the value of `save-some-buffers-default-predicate` instead of `pred`. If the result is `nil`, it means ask only about file-visiting buffers. If it is `t`, that means also offer to save certain other non-file buffers—those that have a non-`nil` buffer-local value of `buffer-offer-save` (see [Killing Buffers](Killing-Buffers.html)). A user who says ‘`yes`’ to saving a non-file buffer is asked to specify the file name to use. The `save-buffers-kill-emacs` function passes the value `t` for `pred`.

If the predicate is neither `t` nor `nil`, then it should be a function of no arguments. It will be called in each buffer to decide whether to offer to save that buffer. If it returns a non-`nil` value in a certain buffer, that means do offer to save that buffer.

### Command: **write-file** *filename \&optional confirm*

This function writes the current buffer into file `filename`, makes the buffer visit that file, and marks it not modified. Then it renames the buffer based on `filename`, appending a string like ‘`<2>`’ if necessary to make a unique buffer name. It does most of this work by calling `set-visited-file-name` (see [Buffer File Name](Buffer-File-Name.html)) and `save-buffer`.

If `confirm` is non-`nil`, that means to ask for confirmation before overwriting an existing file. Interactively, confirmation is required, unless the user supplies a prefix argument.

If `filename` is a directory name (see [Directory Names](Directory-Names.html)), `write-file` uses the name of the visited file, in directory `filename`. If the buffer is not visiting a file, it uses the buffer name instead.

Saving a buffer runs several hooks. It also performs format conversion (see [Format Conversion](Format-Conversion.html)). Note that these hooks, described below, are only run by `save-buffer`, they are not run by other primitives and functions that write buffer text to files, and in particular auto-saving (see [Auto-Saving](Auto_002dSaving.html)) doesn’t run these hooks.

### Variable: **write-file-functions**

The value of this variable is a list of functions to be called before writing out a buffer to its visited file. If one of them returns non-`nil`, the file is considered already written and the rest of the functions are not called, nor is the usual code for writing the file executed.

If a function in `write-file-functions` returns non-`nil`, it is responsible for making a backup file (if that is appropriate). To do so, execute the following code:

```lisp
(or buffer-backed-up (backup-buffer))
```

You might wish to save the file modes value returned by `backup-buffer` and use that (if non-`nil`) to set the mode bits of the file that you write. This is what `save-buffer` normally does. See [Making Backup Files](Making-Backups.html).

The hook functions in `write-file-functions` are also responsible for encoding the data (if desired): they must choose a suitable coding system and end-of-line conversion (see [Lisp and Coding Systems](Lisp-and-Coding-Systems.html)), perform the encoding (see [Explicit Encoding](Explicit-Encoding.html)), and set `last-coding-system-used` to the coding system that was used (see [Encoding and I/O](Encoding-and-I_002fO.html)).

If you set this hook locally in a buffer, it is assumed to be associated with the file or the way the contents of the buffer were obtained. Thus the variable is marked as a permanent local, so that changing the major mode does not alter a buffer-local value. On the other hand, calling `set-visited-file-name` will reset it. If this is not what you want, you might like to use `write-contents-functions` instead.

Even though this is not a normal hook, you can use `add-hook` and `remove-hook` to manipulate the list. See [Hooks](Hooks.html).

### Variable: **write-contents-functions**

This works just like `write-file-functions`, but it is intended for hooks that pertain to the buffer’s contents, not to the particular visited file or its location, and can be used to create arbitrary save processes for buffers that aren’t visiting files at all. Such hooks are usually set up by major modes, as buffer-local bindings for this variable. This variable automatically becomes buffer-local whenever it is set; switching to a new major mode always resets this variable, but calling `set-visited-file-name` does not.

If any of the functions in this hook returns non-`nil`, the file is considered already written and the rest are not called and neither are the functions in `write-file-functions`.

When using this hook to save buffers that are not visiting files (for instance, special-mode buffers), keep in mind that, if the function fails to save correctly and returns a `nil` value, `save-buffer` will go on to prompt the user for a file to save the buffer in. If this is undesirable, consider having the function fail by raising an error.

### User Option: **before-save-hook**

This normal hook runs before a buffer is saved in its visited file, regardless of whether that is done normally or by one of the hooks described above. For instance, the `copyright.el` program uses this hook to make sure the file you are saving has the current year in its copyright notice.

### User Option: **after-save-hook**

This normal hook runs after a buffer has been saved in its visited file.

### User Option: **file-precious-flag**

If this variable is non-`nil`, then `save-buffer` protects against I/O errors while saving by writing the new file to a temporary name instead of the name it is supposed to have, and then renaming it to the intended name after it is clear there are no errors. This procedure prevents problems such as a lack of disk space from resulting in an invalid file.

As a side effect, backups are necessarily made by copying. See [Rename or Copy](Rename-or-Copy.html). Yet, at the same time, saving a precious file always breaks all hard links between the file you save and other file names.

Some modes give this variable a non-`nil` buffer-local value in particular buffers.

### User Option: **require-final-newline**

This variable determines whether files may be written out that do *not* end with a newline. If the value of the variable is `t`, then `save-buffer` silently adds a newline at the end of the buffer whenever it does not already end in one. If the value is `visit`, Emacs adds a missing newline just after it visits the file. If the value is `visit-save`, Emacs adds a missing newline both on visiting and on saving. For any other non-`nil` value, `save-buffer` asks the user whether to add a newline each time the case arises.

If the value of the variable is `nil`, then `save-buffer` doesn’t add newlines at all. `nil` is the default value, but a few major modes set it to `t` in particular buffers.

See also the function `set-visited-file-name` (see [Buffer File Name](Buffer-File-Name.html)).
