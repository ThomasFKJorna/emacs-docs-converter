

Previous: [Auto-Saving](Auto_002dSaving.html), Up: [Backups and Auto-Saving](Backups-and-Auto_002dSaving.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 26.3 Reverting

If you have made extensive changes to a file and then change your mind about them, you can get rid of them by reading in the previous version of the file with the `revert-buffer` command. See [Reverting a Buffer](https://www.gnu.org/software/emacs/manual/html_node/emacs/Reverting.html#Reverting) in The GNU Emacs Manual.

*   Command: **revert-buffer** *\&optional ignore-auto noconfirm preserve-modes*

    This command replaces the buffer text with the text of the visited file on disk. This action undoes all changes since the file was visited or saved.

    By default, if the latest auto-save file is more recent than the visited file, and the argument `ignore-auto` is `nil`, `revert-buffer` asks the user whether to use that auto-save instead. When you invoke this command interactively, `ignore-auto` is `t` if there is no numeric prefix argument; thus, the interactive default is not to check the auto-save file.

    Normally, `revert-buffer` asks for confirmation before it changes the buffer; but if the argument `noconfirm` is non-`nil`, `revert-buffer` does not ask for confirmation.

    Normally, this command reinitializes the buffer’s major and minor modes using `normal-mode`. But if `preserve-modes` is non-`nil`, the modes remain unchanged.

    Reverting tries to preserve marker positions in the buffer by using the replacement feature of `insert-file-contents`. If the buffer contents and the file contents are identical before the revert operation, reverting preserves all the markers. If they are not identical, reverting does change the buffer; in that case, it preserves the markers in the unchanged text (if any) at the beginning and end of the buffer. Preserving any additional markers would be problematical.

<!---->

*   Variable: **revert-buffer-in-progress-p**

    `revert-buffer` binds this variable to a non-`nil` value while it is working.

You can customize how `revert-buffer` does its work by setting the variables described in the rest of this section.

*   User Option: **revert-without-query**

    This variable holds a list of files that should be reverted without query. The value is a list of regular expressions. If the visited file name matches one of these regular expressions, and the file has changed on disk but the buffer is not modified, then `revert-buffer` reverts the file without asking the user for confirmation.

Some major modes customize `revert-buffer` by making buffer-local bindings for these variables:

*   Variable: **revert-buffer-function**

    The value of this variable is the function to use to revert this buffer. It should be a function with two optional arguments to do the work of reverting. The two optional arguments, `ignore-auto` and `noconfirm`, are the arguments that `revert-buffer` received.

    Modes such as Dired mode, in which the text being edited does not consist of a file’s contents but can be regenerated in some other fashion, can give this variable a buffer-local value that is a special function to regenerate the contents.

<!---->

*   Variable: **revert-buffer-insert-file-contents-function**

    The value of this variable specifies the function to use to insert the updated contents when reverting this buffer. The function receives two arguments: first the file name to use; second, `t` if the user has asked to read the auto-save file.

    The reason for a mode to change this variable instead of `revert-buffer-function` is to avoid duplicating or replacing the rest of what `revert-buffer` does: asking for confirmation, clearing the undo list, deciding the proper major mode, and running the hooks listed below.

<!---->

*   Variable: **before-revert-hook**

    This normal hook is run by the default `revert-buffer-function` before inserting the modified contents. A custom `revert-buffer-function` may or may not run this hook.

<!---->

*   Variable: **after-revert-hook**

    This normal hook is run by the default `revert-buffer-function` after inserting the modified contents. A custom `revert-buffer-function` may or may not run this hook.

Emacs can revert buffers automatically. It does that by default for buffers visiting files. The following describes how to add support for auto-reverting new types of buffers.

First, such buffers must have a suitable `revert-buffer-function` and `buffer-stale-function` defined.

*   Variable: **buffer-stale-function**

    The value of this variable specifies a function to call to check whether a buffer needs reverting. The default value only handles buffers that are visiting files, by checking their modification time. Buffers that are not visiting files require a custom function of one optional argument `noconfirm`. The function should return non-`nil` if the buffer should be reverted. The buffer is current when this function is called.

    While this function is mainly intended for use in auto-reverting, it could be used for other purposes as well. For instance, if auto-reverting is not enabled, it could be used to warn the user that the buffer needs reverting. The idea behind the `noconfirm` argument is that it should be `t` if the buffer is going to be reverted without asking the user and `nil` if the function is just going to be used to warn the user that the buffer is out of date. In particular, for use in auto-reverting, `noconfirm` is `t`. If the function is only going to be used for auto-reverting, you can ignore the `noconfirm` argument.

    If you just want to automatically auto-revert every `auto-revert-interval` seconds (like the Buffer Menu), use:

    ```lisp
    (setq-local buffer-stale-function
         (lambda (&optional noconfirm) 'fast))
    ```

    in the buffer’s mode function.

    The special return value ‘`fast`’ tells the caller that the need for reverting was not checked, but that reverting the buffer is fast. It also tells Auto Revert not to print any revert messages, even if `auto-revert-verbose` is non-`nil`. This is important, as getting revert messages every `auto-revert-interval` seconds can be very annoying. The information provided by this return value could also be useful if the function is consulted for purposes other than auto-reverting.

Once the buffer has a suitable `revert-buffer-function` and `buffer-stale-function`, several problems usually remain.

The buffer will only auto-revert if it is marked unmodified. Hence, you will have to make sure that various functions mark the buffer modified if and only if either the buffer contains information that might be lost by reverting, or there is reason to believe that the user might be inconvenienced by auto-reverting, because he is actively working on the buffer. The user can always override this by manually adjusting the modified status of the buffer. To support this, calling the `revert-buffer-function` on a buffer that is marked unmodified should always keep the buffer marked unmodified.

It is important to assure that point does not continuously jump around as a consequence of auto-reverting. Of course, moving point might be inevitable if the buffer radically changes.

You should make sure that the `revert-buffer-function` does not print messages that unnecessarily duplicate Auto Revert’s own messages, displayed if `auto-revert-verbose` is `t`, and effectively override a `nil` value for `auto-revert-verbose`. Hence, adapting a mode for auto-reverting often involves getting rid of such messages. This is especially important for buffers that automatically revert every `auto-revert-interval` seconds.

If the new auto-reverting is part of Emacs, you should mention it in the documentation string of `global-auto-revert-non-file-buffers`.

Similarly, you should document the additions in the Emacs manual.

Previous: [Auto-Saving](Auto_002dSaving.html), Up: [Backups and Auto-Saving](Backups-and-Auto_002dSaving.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
