

### 23.8 Desktop Save Mode

*Desktop Save Mode* is a feature to save the state of Emacs from one session to another. The user-level commands for using Desktop Save Mode are described in the GNU Emacs Manual (see [Saving Emacs Sessions](https://www.gnu.org/software/emacs/manual/html_node/emacs/Saving-Emacs-Sessions.html#Saving-Emacs-Sessions) in the GNU Emacs Manual). Modes whose buffers visit a file, don’t have to do anything to use this feature.

For buffers not visiting a file to have their state saved, the major mode must bind the buffer local variable `desktop-save-buffer` to a non-`nil` value.

### Variable: **desktop-save-buffer**

If this buffer-local variable is non-`nil`, the buffer will have its state saved in the desktop file at desktop save. If the value is a function, it is called at desktop save with argument `desktop-dirname`, and its value is saved in the desktop file along with the state of the buffer for which it was called. When file names are returned as part of the auxiliary information, they should be formatted using the call

```lisp
(desktop-file-name file-name desktop-dirname)
```

For buffers not visiting a file to be restored, the major mode must define a function to do the job, and that function must be listed in the alist `desktop-buffer-mode-handlers`.

### Variable: **desktop-buffer-mode-handlers**

Alist with elements

```lisp
(major-mode . restore-buffer-function)
```

The function `restore-buffer-function` will be called with argument list

```lisp
(buffer-file-name buffer-name desktop-buffer-misc)
```

and it should return the restored buffer. Here `desktop-buffer-misc` is the value returned by the function optionally bound to `desktop-save-buffer`.
