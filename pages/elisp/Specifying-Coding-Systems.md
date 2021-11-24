

#### 33.10.6 Specifying a Coding System for One Operation

You can specify the coding system for a specific operation by binding the variables `coding-system-for-read` and/or `coding-system-for-write`.

### Variable: **coding-system-for-read**

If this variable is non-`nil`, it specifies the coding system to use for reading a file, or for input from a synchronous subprocess.

It also applies to any asynchronous subprocess or network stream, but in a different way: the value of `coding-system-for-read` when you start the subprocess or open the network stream specifies the input decoding method for that subprocess or network stream. It remains in use for that subprocess or network stream unless and until overridden.

The right way to use this variable is to bind it with `let` for a specific I/O operation. Its global value is normally `nil`, and you should not globally set it to any other value. Here is an example of the right way to use the variable:

```lisp
;; Read the file with no character code conversion.
(let ((coding-system-for-read 'no-conversion))
  (insert-file-contents filename))
```

When its value is non-`nil`, this variable takes precedence over all other methods of specifying a coding system to use for input, including `file-coding-system-alist`, `process-coding-system-alist` and `network-coding-system-alist`.

### Variable: **coding-system-for-write**

This works much like `coding-system-for-read`, except that it applies to output rather than input. It affects writing to files, as well as sending output to subprocesses and net connections. It also applies to encoding command-line arguments with which Emacs invokes subprocesses.

When a single operation does both input and output, as do `call-process-region` and `start-process`, both `coding-system-for-read` and `coding-system-for-write` affect it.

### Variable: **coding-system-require-warning**

Binding `coding-system-for-write` to a non-`nil` value prevents output primitives from calling the function specified by `select-safe-coding-system-function` (see [User-Chosen Coding Systems](User_002dChosen-Coding-Systems.html)). This is because `C-x RET c` (`universal-coding-system-argument`) works by binding `coding-system-for-write`, and Emacs should obey user selection. If a Lisp program binds `coding-system-for-write` to a value that might not be safe for encoding the text to be written, it can also bind `coding-system-require-warning` to a non-`nil` value, which will force the output primitives to check the encoding by calling the value of `select-safe-coding-system-function` even though `coding-system-for-write` is non-`nil`. Alternatively, call `select-safe-coding-system` explicitly before using the specified encoding.

### User Option: **inhibit-eol-conversion**

When this variable is non-`nil`, no end-of-line conversion is done, no matter which coding system is specified. This applies to all the Emacs I/O and subprocess primitives, and to the explicit encoding and decoding functions (see [Explicit Encoding](Explicit-Encoding.html)).

Sometimes, you need to prefer several coding systems for some operation, rather than fix a single one. Emacs lets you specify a priority order for using coding systems. This ordering affects the sorting of lists of coding systems returned by functions such as `find-coding-systems-region` (see [Lisp and Coding Systems](Lisp-and-Coding-Systems.html)).

### Function: **coding-system-priority-list** *\&optional highestp*

This function returns the list of coding systems in the order of their current priorities. Optional argument `highestp`, if non-`nil`, means return only the highest priority coding system.

### Function: **set-coding-system-priority** *\&rest coding-systems*

This function puts `coding-systems` at the beginning of the priority list for coding systems, thus making their priority higher than all the rest.

### Macro: **with-coding-priority** *coding-systems \&rest body*

This macro executes `body`, like `progn` does (see [progn](Sequencing.html)), with `coding-systems` at the front of the priority list for coding systems. `coding-systems` should be a list of coding systems to prefer during execution of `body`.
