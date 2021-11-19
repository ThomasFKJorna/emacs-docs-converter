

Next: [Default Coding Systems](Default-Coding-Systems.html), Previous: [Lisp and Coding Systems](Lisp-and-Coding-Systems.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 33.10.4 User-Chosen Coding Systems

*   Function: **select-safe-coding-system** *from to \&optional default-coding-system accept-default-p file*

    This function selects a coding system for encoding specified text, asking the user to choose if necessary. Normally the specified text is the text in the current buffer between `from` and `to`. If `from` is a string, the string specifies the text to encode, and `to` is ignored.

    If the specified text includes raw bytes (see [Text Representations](Text-Representations.html)), `select-safe-coding-system` suggests `raw-text` for its encoding.

    If `default-coding-system` is non-`nil`, that is the first coding system to try; if that can handle the text, `select-safe-coding-system` returns that coding system. It can also be a list of coding systems; then the function tries each of them one by one. After trying all of them, it next tries the current buffer’s value of `buffer-file-coding-system` (if it is not `undecided`), then the default value of `buffer-file-coding-system` and finally the user’s most preferred coding system, which the user can set using the command `prefer-coding-system` (see [Recognizing Coding Systems](https://www.gnu.org/software/emacs/manual/html_node/emacs/Recognize-Coding.html#Recognize-Coding) in The GNU Emacs Manual).

    If one of those coding systems can safely encode all the specified text, `select-safe-coding-system` chooses it and returns it. Otherwise, it asks the user to choose from a list of coding systems which can encode all the text, and returns the user’s choice.

    `default-coding-system` can also be a list whose first element is `t` and whose other elements are coding systems. Then, if no coding system in the list can handle the text, `select-safe-coding-system` queries the user immediately, without trying any of the three alternatives described above. This is handy for checking only the coding systems in the list.

    The optional argument `accept-default-p` determines whether a coding system selected without user interaction is acceptable. If it’s omitted or `nil`, such a silent selection is always acceptable. If it is non-`nil`, it should be a function; `select-safe-coding-system` calls this function with one argument, the base coding system of the selected coding system. If the function returns `nil`, `select-safe-coding-system` rejects the silently selected coding system, and asks the user to select a coding system from a list of possible candidates.

    If the variable `select-safe-coding-system-accept-default-p` is non-`nil`, it should be a function taking a single argument. It is used in place of `accept-default-p`, overriding any value supplied for this argument.

    As a final step, before returning the chosen coding system, `select-safe-coding-system` checks whether that coding system is consistent with what would be selected if the contents of the region were read from a file. (If not, this could lead to data corruption in a file subsequently re-visited and edited.) Normally, `select-safe-coding-system` uses `buffer-file-name` as the file for this purpose, but if `file` is non-`nil`, it uses that file instead (this can be relevant for `write-region` and similar functions). If it detects an apparent inconsistency, `select-safe-coding-system` queries the user before selecting the coding system.

<!---->

*   Variable: **select-safe-coding-system-function**

    This variable names the function to be called to request the user to select a proper coding system for encoding text when the default coding system for an output operation cannot safely encode that text. The default value of this variable is `select-safe-coding-system`. Emacs primitives that write text to files, such as `write-region`, or send text to other processes, such as `process-send-region`, normally call the value of this variable, unless `coding-system-for-write` is bound to a non-`nil` value (see [Specifying Coding Systems](Specifying-Coding-Systems.html)).

Here are two functions you can use to let the user specify a coding system, with completion. See [Completion](Completion.html).

*   Function: **read-coding-system** *prompt \&optional default*

    This function reads a coding system using the minibuffer, prompting with string `prompt`, and returns the coding system name as a symbol. If the user enters null input, `default` specifies which coding system to return. It should be a symbol or a string.

<!---->

*   Function: **read-non-nil-coding-system** *prompt*

    This function reads a coding system using the minibuffer, prompting with string `prompt`, and returns the coding system name as a symbol. If the user tries to enter null input, it asks the user to try again. See [Coding Systems](Coding-Systems.html).

Next: [Default Coding Systems](Default-Coding-Systems.html), Previous: [Lisp and Coding Systems](Lisp-and-Coding-Systems.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
