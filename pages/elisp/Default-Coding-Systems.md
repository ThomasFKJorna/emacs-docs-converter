

#### 33.10.5 Default Coding Systems

This section describes variables that specify the default coding system for certain files or when running certain subprograms, and the function that I/O operations use to access them.

The idea of these variables is that you set them once and for all to the defaults you want, and then do not change them again. To specify a particular coding system for a particular operation in a Lisp program, don’t change these variables; instead, override them using `coding-system-for-read` and `coding-system-for-write` (see [Specifying Coding Systems](Specifying-Coding-Systems.html)).

### User Option: **auto-coding-regexp-alist**

This variable is an alist of text patterns and corresponding coding systems. Each element has the form `(regexp . coding-system)`; a file whose first few kilobytes match `regexp` is decoded with `coding-system` when its contents are read into a buffer. The settings in this alist take priority over `coding:` tags in the files and the contents of `file-coding-system-alist` (see below). The default value is set so that Emacs automatically recognizes mail files in Babyl format and reads them with no code conversions.

### User Option: **file-coding-system-alist**

This variable is an alist that specifies the coding systems to use for reading and writing particular files. Each element has the form `(pattern . coding)`, where `pattern` is a regular expression that matches certain file names. The element applies to file names that match `pattern`.

The CDR of the element, `coding`, should be either a coding system, a cons cell containing two coding systems, or a function name (a symbol with a function definition). If `coding` is a coding system, that coding system is used for both reading the file and writing it. If `coding` is a cons cell containing two coding systems, its CAR specifies the coding system for decoding, and its CDR specifies the coding system for encoding.

If `coding` is a function name, the function should take one argument, a list of all arguments passed to `find-operation-coding-system`. It must return a coding system or a cons cell containing two coding systems. This value has the same meaning as described above.

If `coding` (or what returned by the above function) is `undecided`, the normal code-detection is performed.

### User Option: **auto-coding-alist**

This variable is an alist that specifies the coding systems to use for reading and writing particular files. Its form is like that of `file-coding-system-alist`, but, unlike the latter, this variable takes priority over any `coding:` tags in the file.

### Variable: **process-coding-system-alist**

This variable is an alist specifying which coding systems to use for a subprocess, depending on which program is running in the subprocess. It works like `file-coding-system-alist`, except that `pattern` is matched against the program name used to start the subprocess. The coding system or systems specified in this alist are used to initialize the coding systems used for I/O to the subprocess, but you can specify other coding systems later using `set-process-coding-system`.

**Warning:** Coding systems such as `undecided`, which determine the coding system from the data, do not work entirely reliably with asynchronous subprocess output. This is because Emacs handles asynchronous subprocess output in batches, as it arrives. If the coding system leaves the character code conversion unspecified, or leaves the end-of-line conversion unspecified, Emacs must try to detect the proper conversion from one batch at a time, and this does not always work.

Therefore, with an asynchronous subprocess, if at all possible, use a coding system which determines both the character code conversion and the end of line conversion—that is, one like `latin-1-unix`, rather than `undecided` or `latin-1`.

### Variable: **network-coding-system-alist**

This variable is an alist that specifies the coding system to use for network streams. It works much like `file-coding-system-alist`, with the difference that the `pattern` in an element may be either a port number or a regular expression. If it is a regular expression, it is matched against the network service name used to open the network stream.

### Variable: **default-process-coding-system**

This variable specifies the coding systems to use for subprocess (and network stream) input and output, when nothing else specifies what to do.

The value should be a cons cell of the form `(input-coding . output-coding)`. Here `input-coding` applies to input from the subprocess, and `output-coding` applies to output to it.

### User Option: **auto-coding-functions**

This variable holds a list of functions that try to determine a coding system for a file based on its undecoded contents.

Each function in this list should be written to look at text in the current buffer, but should not modify it in any way. The buffer will contain the text of parts of the file. Each function should take one argument, `size`, which tells it how many characters to look at, starting from point. If the function succeeds in determining a coding system for the file, it should return that coding system. Otherwise, it should return `nil`.

The functions in this list could be called either when the file is visited and Emacs wants to decode its contents, and/or when the file’s buffer is about to be saved and Emacs wants to determine how to encode its contents.

If a file has a ‘`coding:`’ tag, that takes precedence, so these functions won’t be called.

### Function: **find-auto-coding** *filename size*

This function tries to determine a suitable coding system for `filename`. It examines the buffer visiting the named file, using the variables documented above in sequence, until it finds a match for one of the rules specified by these variables. It then returns a cons cell of the form `(coding . source)`, where `coding` is the coding system to use and `source` is a symbol, one of `auto-coding-alist`, `auto-coding-regexp-alist`, `:coding`, or `auto-coding-functions`, indicating which one supplied the matching rule. The value `:coding` means the coding system was specified by the `coding:` tag in the file (see [coding tag](https://www.gnu.org/software/emacs/manual/html_node/emacs/Specify-Coding.html#Specify-Coding) in The GNU Emacs Manual). The order of looking for a matching rule is `auto-coding-alist` first, then `auto-coding-regexp-alist`, then the `coding:` tag, and lastly `auto-coding-functions`. If no matching rule was found, the function returns `nil`.

The second argument `size` is the size of text, in characters, following point. The function examines text only within `size` characters after point. Normally, the buffer should be positioned at the beginning when this function is called, because one of the places for the `coding:` tag is the first one or two lines of the file; in that case, `size` should be the size of the buffer.

### Function: **set-auto-coding** *filename size*

This function returns a suitable coding system for file `filename`. It uses `find-auto-coding` to find the coding system. If no coding system could be determined, the function returns `nil`. The meaning of the argument `size` is like in `find-auto-coding`.

### Function: **find-operation-coding-system** *operation \&rest arguments*

This function returns the coding system to use (by default) for performing `operation` with `arguments`. The value has this form:

```lisp
(decoding-system . encoding-system)
```

The first element, `decoding-system`, is the coding system to use for decoding (in case `operation` does decoding), and `encoding-system` is the coding system for encoding (in case `operation` does encoding).

The argument `operation` is a symbol; it should be one of `write-region`, `start-process`, `call-process`, `call-process-region`, `insert-file-contents`, or `open-network-stream`. These are the names of the Emacs I/O primitives that can do character code and eol conversion.

The remaining arguments should be the same arguments that might be given to the corresponding I/O primitive. Depending on the primitive, one of those arguments is selected as the *target*. For example, if `operation` does file I/O, whichever argument specifies the file name is the target. For subprocess primitives, the process name is the target. For `open-network-stream`, the target is the service name or port number.

Depending on `operation`, this function looks up the target in `file-coding-system-alist`, `process-coding-system-alist`, or `network-coding-system-alist`. If the target is found in the alist, `find-operation-coding-system` returns its association in the alist; otherwise it returns `nil`.

If `operation` is `insert-file-contents`, the argument corresponding to the target may be a cons cell of the form `(filename . buffer)`. In that case, `filename` is a file name to look up in `file-coding-system-alist`, and `buffer` is a buffer that contains the file’s contents (not yet decoded). If `file-coding-system-alist` specifies a function to call for this file, and that function needs to examine the file’s contents (as it usually does), it should examine the contents of `buffer` instead of reading the file.
