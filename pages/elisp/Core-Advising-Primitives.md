

#### 13.11.1 Primitives to manipulate advices

### Macro: **add-function** *where place function \&optional props*

This macro is the handy way to add the advice `function` to the function stored in `place` (see [Generalized Variables](Generalized-Variables.html)).

`where` determines how `function` is composed with the existing function, e.g., whether `function` should be called before, or after the original function. See [Advice Combinators](Advice-Combinators.html), for the list of available ways to compose the two functions.

When modifying a variable (whose name will usually end with `-function`), you can choose whether `function` is used globally or only in the current buffer: if `place` is just a symbol, then `function` is added to the global value of `place`. Whereas if `place` is of the form `(local symbol)`, where `symbol` is an expression which returns the variable name, then `function` will only be added in the current buffer. Finally, if you want to modify a lexical variable, you will have to use `(var variable)`.

Every function added with `add-function` can be accompanied by an association list of properties `props`. Currently only two of those properties have a special meaning:

*   `name`

    This gives a name to the advice, which `remove-function` can use to identify which function to remove. Typically used when `function` is an anonymous function.

*   `depth`

    This specifies how to order the advice, should several pieces of advice be present. By default, the depth is 0. A depth of 100 indicates that this piece of advice should be kept as deep as possible, whereas a depth of -100 indicates that it should stay as the outermost piece. When two pieces of advice specify the same depth, the most recently added one will be outermost.

    For `:before` advice, being outermost means that this advice will be run first, before any other advice, whereas being innermost means that it will run right before the original function, with no other advice run between itself and the original function. Similarly, for `:after` advice innermost means that it will run right after the original function, with no other advice run in between, whereas outermost means that it will be run right at the end after all other advice. An innermost `:override` piece of advice will only override the original function and other pieces of advice will apply to it, whereas an outermost `:override` piece of advice will override not only the original function but all other advice applied to it as well.

If `function` is not interactive, then the combined function will inherit the interactive spec, if any, of the original function. Else, the combined function will be interactive and will use the interactive spec of `function`. One exception: if the interactive spec of `function` is a function (i.e., a `lambda` expression or an `fbound` symbol rather than an expression or a string), then the interactive spec of the combined function will be a call to that function with as sole argument the interactive spec of the original function. To interpret the spec received as argument, use `advice-eval-interactive-spec`.

Note: The interactive spec of `function` will apply to the combined function and should hence obey the calling convention of the combined function rather than that of `function`. In many cases, it makes no difference since they are identical, but it does matter for `:around`, `:filter-args`, and `:filter-return`, where `function` receives different arguments than the original function stored in `place`.

### Macro: **remove-function** *place function*

This macro removes `function` from the function stored in `place`. This only works if `function` was added to `place` using `add-function`.

`function` is compared with functions added to `place` using `equal`, to try and make it work also with lambda expressions. It is additionally compared also with the `name` property of the functions added to `place`, which can be more reliable than comparing lambda expressions using `equal`.

### Function: **advice-function-member-p** *advice function-def*

Return non-`nil` if `advice` is already in `function-def`. Like for `remove-function` above, instead of `advice` being the actual function, it can also be the `name` of the piece of advice.

### Function: **advice-function-mapc** *f function-def*

Call the function `f` for every piece of advice that was added to `function-def`. `f` is called with two arguments: the advice function and its properties.

### Function: **advice-eval-interactive-spec** *spec*

Evaluate the interactive `spec` just like an interactive call to a function with such a spec would, and then return the corresponding list of arguments that was built. E.g., `(advice-eval-interactive-spec "r\nP")` will return a list of three elements, containing the boundaries of the region and the current prefix argument.

For instance, if you want to make the `C-x m` (`compose-mail`) command prompt for a ‘`From:`’ header, you could say something like this:

```lisp
(defun my-compose-mail-advice (orig &rest args)
  "Read From: address interactively."
  (interactive
   (lambda (spec)
     (let* ((user-mail-address
             (completing-read "From: "
                              '("one.address@example.net"
                                "alternative.address@example.net")))
            (from (message-make-from user-full-name
                                     user-mail-address))
            (spec (advice-eval-interactive-spec spec)))
       ;; Put the From header into the OTHER-HEADERS argument.
       (push (cons 'From from) (nth 2 spec))
       spec)))
  (apply orig args))

(advice-add 'compose-mail :around #'my-compose-mail-advice)
```
