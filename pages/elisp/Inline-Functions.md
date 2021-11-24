

### 13.13 Inline Functions

An *inline function* is a function that works just like an ordinary function, except for one thing: when you byte-compile a call to the function (see [Byte Compilation](Byte-Compilation.html)), the function’s definition is expanded into the caller.

The simple way to define an inline function, is to write `defsubst` instead of `defun`. The rest of the definition looks just the same, but using `defsubst` says to make it inline for byte compilation.

### Macro: **defsubst** *name args \[doc] \[declare] \[interactive] body…*

This macro defines an inline function. Its syntax is exactly the same as `defun` (see [Defining Functions](Defining-Functions.html)).

Making a function inline often makes its function calls run faster. But it also has disadvantages. For one thing, it reduces flexibility; if you change the definition of the function, calls already inlined still use the old definition until you recompile them.

Another disadvantage is that making a large function inline can increase the size of compiled code both in files and in memory. Since the speed advantage of inline functions is greatest for small functions, you generally should not make large functions inline.

Also, inline functions do not behave well with respect to debugging, tracing, and advising (see [Advising Functions](Advising-Functions.html)). Since ease of debugging and the flexibility of redefining functions are important features of Emacs, you should not make a function inline, even if it’s small, unless its speed is really crucial, and you’ve timed the code to verify that using `defun` actually has performance problems.

After an inline function is defined, its inline expansion can be performed later on in the same file, just like macros.

It’s possible to use `defmacro` to define a macro to expand into the same code that an inline function would execute (see [Macros](Macros.html)). But the macro would be limited to direct use in expressions—a macro cannot be called with `apply`, `mapcar` and so on. Also, it takes some work to convert an ordinary function into a macro. To convert it into an inline function is easy; just replace `defun` with `defsubst`. Since each argument of an inline function is evaluated exactly once, you needn’t worry about how many times the body uses the arguments, as you do for macros.

Alternatively, you can define a function by providing the code which will inline it as a compiler macro. The following macros make this possible.

### Macro: **define-inline** *name args \[doc] \[declare] body…*

Define a function `name` by providing code that does its inlining, as a compiler macro. The function will accept the argument list `args` and will have the specified `body`.

If present, `doc` should be the function’s documentation string (see [Function Documentation](Function-Documentation.html)); `declare`, if present, should be a `declare` form (see [Declare Form](Declare-Form.html)) specifying the function’s metadata.

Functions defined via `define-inline` have several advantages with respect to macros defined by `defsubst` or `defmacro`:

\- They can be passed to `mapcar` (see [Mapping Functions](Mapping-Functions.html)).

\- They are more efficient.

\- They can be used as *place forms* to store values (see [Generalized Variables](Generalized-Variables.html)).

\- They behave in a more predictable way than `cl-defsubst` (see [Argument Lists](https://www.gnu.org/software/emacs/manual/html_node/cl/Argument-Lists.html#Argument-Lists) in Common Lisp Extensions for GNU Emacs Lisp).

Like `defmacro`, a function inlined with `define-inline` inherits the scoping rules, either dynamic or lexical, from the call site. See [Variable Scoping](Variable-Scoping.html).

The following macros should be used in the body of a function defined by `define-inline`.

### Macro: **inline-quote** *expression*

Quote `expression` for `define-inline`. This is similar to the backquote (see [Backquote](Backquote.html)), but quotes code and accepts only `,`, not `,@`.

### Macro: **inline-letevals** *(bindings…) body…*

This is similar to `let` (see [Local Variables](Local-Variables.html)): it sets up local variables as specified by `bindings`, and then evaluates `body` with those bindings in effect. Each element of `bindings` should be either a symbol or a list of the form `(var expr)`; the result is to evaluate `expr` and bind `var` to the result. The tail of `bindings` can be either `nil` or a symbol which should hold a list of arguments, in which case each argument is evaluated, and the symbol is bound to the resulting list.

### Macro: **inline-const-p** *expression*

Return non-`nil` if the value of `expression` is already known.

### Macro: **inline-const-val** *expression*

Return the value of `expression`.

### Macro: **inline-error** *format \&rest args*

Signal an error, formatting `args` according to `format`.

Here’s an example of using `define-inline`:

```lisp
(define-inline myaccessor (obj)
  (inline-letevals (obj)
    (inline-quote (if (foo-p ,obj) (aref (cdr ,obj) 3) (aref ,obj 2)))))
```

This is equivalent to

```lisp
(defsubst myaccessor (obj)
  (if (foo-p obj) (aref (cdr obj) 3) (aref obj 2)))
```
