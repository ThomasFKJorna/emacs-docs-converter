

### 16.8 Which File Defined a Certain Symbol

### Function: **symbol-file** *symbol \&optional type*

This function returns the name of the file that defined `symbol`. If `type` is `nil`, then any kind of definition is acceptable. If `type` is `defun`, `defvar`, or `defface`, that specifies function definition, variable definition, or face definition only.

The value is normally an absolute file name. It can also be `nil`, if the definition is not associated with any file. If `symbol` specifies an autoloaded function, the value can be a relative file name without extension.

The basis for `symbol-file` is the data in the variable `load-history`.

### Variable: **load-history**

The value of this variable is an alist that associates the names of loaded library files with the names of the functions and variables they defined, as well as the features they provided or required.

Each element in this alist describes one loaded library (including libraries that are preloaded at startup). It is a list whose CAR is the absolute file name of the library (a string). The rest of the list elements have these forms:

*   `var`

    The symbol `var` was defined as a variable.

*   `(defun . fun)`

    The function `fun` was defined.

*   `(t . fun)`

    The function `fun` was previously an autoload before this library redefined it as a function. The following element is always `(defun . fun)`, which represents defining `fun` as a function.

*   `(autoload . fun)`

    The function `fun` was defined as an autoload.

*   `(defface . face)`

    The face `face` was defined.

*   `(require . feature)`

    The feature `feature` was required.

*   `(provide . feature)`

    The feature `feature` was provided.

*   `(cl-defmethod method specializers)`

    The named `method` was defined by using `cl-defmethod`, with `specializers` as its specializers.

*   `(define-type . type)`

    The type `type` was defined.

The value of `load-history` may have one element whose CAR is `nil`. This element describes definitions made with `eval-buffer` on a buffer that is not visiting a file.

The command `eval-region` updates `load-history`, but does so by adding the symbols defined to the element for the file being visited, rather than replacing that element. See [Eval](Eval.html).
