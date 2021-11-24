

### 16.2 Load Suffixes

We now describe some technical details about the exact suffixes that `load` tries.

### Variable: **load-suffixes**

This is a list of suffixes indicating (compiled or source) Emacs Lisp files. It should not include the empty string. `load` uses these suffixes in order when it appends Lisp suffixes to the specified file name. The standard value is `(".elc" ".el")` which produces the behavior described in the previous section.

### Variable: **load-file-rep-suffixes**

This is a list of suffixes that indicate representations of the same file. This list should normally start with the empty string. When `load` searches for a file it appends the suffixes in this list, in order, to the file name, before searching for another file.

Enabling Auto Compression mode appends the suffixes in `jka-compr-load-suffixes` to this list and disabling Auto Compression mode removes them again. The standard value of `load-file-rep-suffixes` if Auto Compression mode is disabled is `("")`. Given that the standard value of `jka-compr-load-suffixes` is `(".gz")`, the standard value of `load-file-rep-suffixes` if Auto Compression mode is enabled is `("" ".gz")`.

### Function: **get-load-suffixes**

This function returns the list of all suffixes that `load` should try, in order, when its `must-suffix` argument is non-`nil`. This takes both `load-suffixes` and `load-file-rep-suffixes` into account. If `load-suffixes`, `jka-compr-load-suffixes` and `load-file-rep-suffixes` all have their standard values, this function returns `(".elc" ".elc.gz" ".el" ".el.gz")` if Auto Compression mode is enabled and `(".elc" ".el")` if Auto Compression mode is disabled.

To summarize, `load` normally first tries the suffixes in the value of `(get-load-suffixes)` and then those in `load-file-rep-suffixes`. If `nosuffix` is non-`nil`, it skips the former group, and if `must-suffix` is non-`nil`, it skips the latter group.

### User Option: **load-prefer-newer**

If this option is non-`nil`, then rather than stopping at the first suffix that exists, `load` tests them all, and uses whichever file is the newest.
