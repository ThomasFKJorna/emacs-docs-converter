

Next: [Porting Old Advice](Porting-Old-Advice.html), Previous: [Advising Named Functions](Advising-Named-Functions.html), Up: [Advising Functions](Advising-Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 13.11.3 Ways to compose advice

Here are the different possible values for the `where` argument of `add-function` and `advice-add`, specifying how the advice `function` and the original function should be composed.

*   `:before`

    Call `function` before the old function. Both functions receive the same arguments, and the return value of the composition is the return value of the old function. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (apply function r) (apply oldfun r))
    ```

    `(add-function :before funvar function)` is comparable for single-function hooks to `(add-hook 'hookvar function)` for normal hooks.

*   `:after`

    Call `function` after the old function. Both functions receive the same arguments, and the return value of the composition is the return value of the old function. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (prog1 (apply oldfun r) (apply function r)))
    ```

    `(add-function :after funvar function)` is comparable for single-function hooks to `(add-hook 'hookvar function 'append)` for normal hooks.

*   `:override`

    This completely replaces the old function with the new one. The old function can of course be recovered if you later call `remove-function`.

*   `:around`

    Call `function` instead of the old function, but provide the old function as an extra argument to `function`. This is the most flexible composition. For example, it lets you call the old function with different arguments, or many times, or within a let-binding, or you can sometimes delegate the work to the old function and sometimes override it completely. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (apply function oldfun r))
    ```

*   `:before-while`

    Call `function` before the old function and don’t call the old function if `function` returns `nil`. Both functions receive the same arguments, and the return value of the composition is the return value of the old function. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (and (apply function r) (apply oldfun r)))
    ```

    `(add-function :before-while funvar function)` is comparable for single-function hooks to `(add-hook 'hookvar function)` when `hookvar` is run via `run-hook-with-args-until-failure`.

*   `:before-until`

    Call `function` before the old function and only call the old function if `function` returns `nil`. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (or (apply function r) (apply oldfun r)))
    ```

    `(add-function :before-until funvar function)` is comparable for single-function hooks to `(add-hook 'hookvar function)` when `hookvar` is run via `run-hook-with-args-until-success`.

*   `:after-while`

    Call `function` after the old function and only if the old function returned non-`nil`. Both functions receive the same arguments, and the return value of the composition is the return value of `function`. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (and (apply oldfun r) (apply function r)))
    ```

    `(add-function :after-while funvar function)` is comparable for single-function hooks to `(add-hook 'hookvar function 'append)` when `hookvar` is run via `run-hook-with-args-until-failure`.

*   `:after-until`

    Call `function` after the old function and only if the old function returned `nil`. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (or  (apply oldfun r) (apply function r)))
    ```

    `(add-function :after-until funvar function)` is comparable for single-function hooks to `(add-hook 'hookvar function 'append)` when `hookvar` is run via `run-hook-with-args-until-success`.

*   `:filter-args`

    Call `function` first and use the result (which should be a list) as the new arguments to pass to the old function. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (apply oldfun (funcall function r)))
    ```

*   `:filter-return`

    Call the old function first and pass the result to `function`. More specifically, the composition of the two functions behaves like:

    ```lisp
    (lambda (&rest r) (funcall function (apply oldfun r)))
    ```

Next: [Porting Old Advice](Porting-Old-Advice.html), Previous: [Advising Named Functions](Advising-Named-Functions.html), Up: [Advising Functions](Advising-Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
