

Next: [Completion in Buffers](Completion-in-Buffers.html), Previous: [Completion Variables](Completion-Variables.html), Up: [Completion](Completion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 20.6.7 Programmed Completion

Sometimes it is not possible or convenient to create an alist or an obarray containing all the intended possible completions ahead of time. In such a case, you can supply your own function to compute the completion of a given string. This is called *programmed completion*. Emacs uses programmed completion when completing file names (see [File Name Completion](File-Name-Completion.html)), among many other cases.

To use this feature, pass a function as the `collection` argument to `completing-read`. The function `completing-read` arranges to pass your completion function along to `try-completion`, `all-completions`, and other basic completion functions, which will then let your function do all the work.

The completion function should accept three arguments:

*   The string to be completed.

*   A predicate function with which to filter possible matches, or `nil` if none. The function should call the predicate for each possible match, and ignore the match if the predicate returns `nil`.

*   A flag specifying the type of completion operation to perform; see [Basic Completion](Basic-Completion.html), for the details of those operations. This flag may be one of the following values.

    *   `nil`

        This specifies a `try-completion` operation. The function should return `nil` if there are no matches; it should return `t` if the specified string is a unique and exact match; and it should return the longest common prefix substring of all matches otherwise.

    *   `t`

        This specifies an `all-completions` operation. The function should return a list of all possible completions of the specified string.

    *   `lambda`

        This specifies a `test-completion` operation. The function should return `t` if the specified string is an exact match for some completion alternative; `nil` otherwise.

    *   `(boundaries . suffix)`

        This specifies a `completion-boundaries` operation. The function should return `(boundaries start . end)`, where `start` is the position of the beginning boundary in the specified string, and `end` is the position of the end boundary in `suffix`.

    *   `metadata`

        This specifies a request for information about the state of the current completion. The return value should have the form `(metadata . alist)`, where `alist` is an alist whose elements are described below.

    If the flag has any other value, the completion function should return `nil`.

The following is a list of metadata entries that a completion function may return in response to a `metadata` flag argument:

*   `category`

    The value should be a symbol describing what kind of text the completion function is trying to complete. If the symbol matches one of the keys in `completion-category-overrides`, the usual completion behavior is overridden. See [Completion Variables](Completion-Variables.html).

*   `annotation-function`

    The value should be a function for *annotating* completions. The function should take one argument, `string`, which is a possible completion. It should return a string, which is displayed after the completion `string` in the `*Completions*` buffer.

*   `display-sort-function`

    The value should be a function for sorting completions. The function should take one argument, a list of completion strings, and return a sorted list of completion strings. It is allowed to alter the input list destructively.

*   `cycle-sort-function`

    The value should be a function for sorting completions, when `completion-cycle-threshold` is non-`nil` and the user is cycling through completion alternatives. See [Completion Options](https://www.gnu.org/software/emacs/manual/html_node/emacs/Completion-Options.html#Completion-Options) in The GNU Emacs Manual. Its argument list and return value are the same as for `display-sort-function`.

<!---->

*   Function: **completion-table-dynamic** *function \&optional switch-buffer*

    This function is a convenient way to write a function that can act as a programmed completion function. The argument `function` should be a function that takes one argument, a string, and returns a completion table (see [Basic Completion](Basic-Completion.html)) containing all the possible completions. The table returned by `function` can also include elements that don’t match the string argument; they are automatically filtered out by `completion-table-dynamic`. In particular, `function` can ignore its argument and return a full list of all possible completions. You can think of `completion-table-dynamic` as a transducer between `function` and the interface for programmed completion functions.

    If the optional argument `switch-buffer` is non-`nil`, and completion is performed in the minibuffer, `function` will be called with current buffer set to the buffer from which the minibuffer was entered.

    The return value of `completion-table-dynamic` is a function that can be used as the 2nd argument to `try-completion` and `all-completions`. Note that this function will always return empty metadata and trivial boundaries (see [Programmed Completion](#Programmed-Completion)).

<!---->

*   Function: **completion-table-with-cache** *function \&optional ignore-case*

    This is a wrapper for `completion-table-dynamic` that saves the last argument-result pair. This means that multiple lookups with the same argument only need to call `function` once. This can be useful when a slow operation is involved, such as calling an external process.

Next: [Completion in Buffers](Completion-in-Buffers.html), Previous: [Completion Variables](Completion-Variables.html), Up: [Completion](Completion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
