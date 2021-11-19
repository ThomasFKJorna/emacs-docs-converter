

Next: [Lisp and Coding Systems](Lisp-and-Coding-Systems.html), Previous: [Coding System Basics](Coding-System-Basics.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 33.10.2 Encoding and I/O

The principal purpose of coding systems is for use in reading and writing files. The function `insert-file-contents` uses a coding system to decode the file data, and `write-region` uses one to encode the buffer contents.

You can specify the coding system to use either explicitly (see [Specifying Coding Systems](Specifying-Coding-Systems.html)), or implicitly using a default mechanism (see [Default Coding Systems](Default-Coding-Systems.html)). But these methods may not completely specify what to do. For example, they may choose a coding system such as `undecided` which leaves the character code conversion to be determined from the data. In these cases, the I/O operation finishes the job of choosing a coding system. Very often you will want to find out afterwards which coding system was chosen.

*   Variable: **buffer-file-coding-system**

    This buffer-local variable records the coding system used for saving the buffer and for writing part of the buffer with `write-region`. If the text to be written cannot be safely encoded using the coding system specified by this variable, these operations select an alternative encoding by calling the function `select-safe-coding-system` (see [User-Chosen Coding Systems](User_002dChosen-Coding-Systems.html)). If selecting a different encoding requires to ask the user to specify a coding system, `buffer-file-coding-system` is updated to the newly selected coding system.

    `buffer-file-coding-system` does *not* affect sending text to a subprocess.

<!---->

*   Variable: **save-buffer-coding-system**

    This variable specifies the coding system for saving the buffer (by overriding `buffer-file-coding-system`). Note that it is not used for `write-region`.

    When a command to save the buffer starts out to use `buffer-file-coding-system` (or `save-buffer-coding-system`), and that coding system cannot handle the actual text in the buffer, the command asks the user to choose another coding system (by calling `select-safe-coding-system`). After that happens, the command also updates `buffer-file-coding-system` to represent the coding system that the user specified.

<!---->

*   Variable: **last-coding-system-used**

    I/O operations for files and subprocesses set this variable to the coding system name that was used. The explicit encoding and decoding functions (see [Explicit Encoding](Explicit-Encoding.html)) set it too.

    **Warning:** Since receiving subprocess output sets this variable, it can change whenever Emacs waits; therefore, you should copy the value shortly after the function call that stores the value you are interested in.

The variable `selection-coding-system` specifies how to encode selections for the window system. See [Window System Selections](Window-System-Selections.html).

*   Variable: **file-name-coding-system**

    The variable `file-name-coding-system` specifies the coding system to use for encoding file names. Emacs encodes file names using that coding system for all file operations. If `file-name-coding-system` is `nil`, Emacs uses a default coding system determined by the selected language environment. In the default language environment, any non-ASCII characters in file names are not encoded specially; they appear in the file system using the internal Emacs representation.

**Warning:** if you change `file-name-coding-system` (or the language environment) in the middle of an Emacs session, problems can result if you have already visited files whose names were encoded using the earlier coding system and are handled differently under the new coding system. If you try to save one of these buffers under the visited file name, saving may use the wrong file name, or it may get an error. If such a problem happens, use `C-x C-w` to specify a new file name for that buffer.

On Windows 2000 and later, Emacs by default uses Unicode APIs to pass file names to the OS, so the value of `file-name-coding-system` is largely ignored. Lisp applications that need to encode or decode file names on the Lisp level should use `utf-8` coding-system when `system-type` is `windows-nt`; the conversion of UTF-8 encoded file names to the encoding appropriate for communicating with the OS is performed internally by Emacs.

Next: [Lisp and Coding Systems](Lisp-and-Coding-Systems.html), Previous: [Coding System Basics](Coding-System-Basics.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
