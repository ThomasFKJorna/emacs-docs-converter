

### 27.10 Killing Buffers

*Killing a buffer* makes its name unknown to Emacs and makes the memory space it occupied available for other use.

The buffer object for the buffer that has been killed remains in existence as long as anything refers to it, but it is specially marked so that you cannot make it current or display it. Killed buffers retain their identity, however; if you kill two distinct buffers, they remain distinct according to `eq` although both are dead.

If you kill a buffer that is current or displayed in a window, Emacs automatically selects or displays some other buffer instead. This means that killing a buffer can change the current buffer. Therefore, when you kill a buffer, you should also take the precautions associated with changing the current buffer (unless you happen to know that the buffer being killed isn’t current). See [Current Buffer](Current-Buffer.html).

If you kill a buffer that is the base buffer of one or more indirect buffers (see [Indirect Buffers](Indirect-Buffers.html)), the indirect buffers are automatically killed as well.

The `buffer-name` of a buffer is `nil` if, and only if, the buffer is killed. A buffer that has not been killed is called a *live* buffer. To test whether a buffer is live or killed, use the function `buffer-live-p` (see below).

### Command: **kill-buffer** *\&optional buffer-or-name*

This function kills the buffer `buffer-or-name`, freeing all its memory for other uses or to be returned to the operating system. If `buffer-or-name` is `nil` or omitted, it kills the current buffer.

Any processes that have this buffer as the `process-buffer` are sent the `SIGHUP` (hangup) signal, which normally causes them to terminate. See [Signals to Processes](Signals-to-Processes.html).

If the buffer is visiting a file and contains unsaved changes, `kill-buffer` asks the user to confirm before the buffer is killed. It does this even if not called interactively. To prevent the request for confirmation, clear the modified flag before calling `kill-buffer`. See [Buffer Modification](Buffer-Modification.html).

This function calls `replace-buffer-in-windows` for cleaning up all windows currently displaying the buffer to be killed.

Killing a buffer that is already dead has no effect.

This function returns `t` if it actually killed the buffer. It returns `nil` if the user refuses to confirm or if `buffer-or-name` was already dead.

```lisp
(kill-buffer "foo.unchanged")
     ⇒ t
(kill-buffer "foo.changed")

---------- Buffer: Minibuffer ----------
Buffer foo.changed modified; kill anyway? (yes or no) yes
---------- Buffer: Minibuffer ----------

     ⇒ t
```

### Variable: **kill-buffer-query-functions**

Before confirming unsaved changes, `kill-buffer` calls the functions in the list `kill-buffer-query-functions`, in order of appearance, with no arguments. The buffer being killed is the current buffer when they are called. The idea of this feature is that these functions will ask for confirmation from the user. If any of them returns `nil`, `kill-buffer` spares the buffer’s life.

### Variable: **kill-buffer-hook**

This is a normal hook run by `kill-buffer` after asking all the questions it is going to ask, just before actually killing the buffer. The buffer to be killed is current when the hook functions run. See [Hooks](Hooks.html). This variable is a permanent local, so its local binding is not cleared by changing major modes.

### User Option: **buffer-offer-save**

This variable, if non-`nil` in a particular buffer, tells `save-buffers-kill-emacs` to offer to save that buffer, just as it offers to save file-visiting buffers. If `save-some-buffers` is called with the second optional argument set to `t`, it will also offer to save the buffer. Lastly, if this variable is set to the symbol `always`, both `save-buffers-kill-emacs` and `save-some-buffers` will always offer to save. See [Definition of save-some-buffers](Saving-Buffers.html#Definition-of-save_002dsome_002dbuffers). The variable `buffer-offer-save` automatically becomes buffer-local when set for any reason. See [Buffer-Local Variables](Buffer_002dLocal-Variables.html).

### Variable: **buffer-save-without-query**

This variable, if non-`nil` in a particular buffer, tells `save-buffers-kill-emacs` and `save-some-buffers` to save this buffer (if it’s modified) without asking the user. The variable automatically becomes buffer-local when set for any reason.

### Function: **buffer-live-p** *object*

This function returns `t` if `object` is a live buffer (a buffer which has not been killed), `nil` otherwise.
