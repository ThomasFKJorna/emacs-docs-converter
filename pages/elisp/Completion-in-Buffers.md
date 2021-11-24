

#### 20.6.8 Completion in Ordinary Buffers

Although completion is usually done in the minibuffer, the completion facility can also be used on the text in ordinary Emacs buffers. In many major modes, in-buffer completion is performed by the `C-M-i` or `M-TAB` command, bound to `completion-at-point`. See [Symbol Completion](https://www.gnu.org/software/emacs/manual/html_node/emacs/Symbol-Completion.html#Symbol-Completion) in The GNU Emacs Manual. This command uses the abnormal hook variable `completion-at-point-functions`:

### Variable: **completion-at-point-functions**

The value of this abnormal hook should be a list of functions, which are used to compute a completion table (see [Basic Completion](Basic-Completion.html)) for completing the text at point. It can be used by major modes to provide mode-specific completion tables (see [Major Mode Conventions](Major-Mode-Conventions.html)).

When the command `completion-at-point` runs, it calls the functions in the list one by one, without any argument. Each function should return `nil` unless it can and wants to take responsibility for the completion data for the text at point. Otherwise it should return a list of the following form:

```lisp
(start end collection . props)
```

`start` and `end` delimit the text to complete (which should enclose point). `collection` is a completion table for completing that text, in a form suitable for passing as the second argument to `try-completion` (see [Basic Completion](Basic-Completion.html)); completion alternatives will be generated from this completion table in the usual way, via the completion styles defined in `completion-styles` (see [Completion Variables](Completion-Variables.html)). `props` is a property list for additional information; any of the properties in `completion-extra-properties` are recognized (see [Completion Variables](Completion-Variables.html)), as well as the following additional ones:

*   `:predicate`

    The value should be a predicate that completion candidates need to satisfy.

*   `:exclusive`

    If the value is `no`, then if the completion table fails to match the text at point, `completion-at-point` moves on to the next function in `completion-at-point-functions` instead of reporting a completion failure.

The functions on this hook should generally return quickly, since they may be called very often (e.g., from `post-command-hook`). Supplying a function for `collection` is strongly recommended if generating the list of completions is an expensive operation. Emacs may internally call functions in `completion-at-point-functions` many times, but care about the value of `collection` for only some of these calls. By supplying a function for `collection`, Emacs can defer generating completions until necessary. You can use `completion-table-dynamic` to create a wrapper function:

```lisp
;; Avoid this pattern.
(let ((beg ...) (end ...) (my-completions (my-make-completions)))
  (list beg end my-completions))

;; Use this instead.
(let ((beg ...) (end ...))
  (list beg
        end
        (completion-table-dynamic
          (lambda (_)
            (my-make-completions)))))
```

Additionally, the `collection` should generally not be pre-filtered based on the current text between `start` and `end`, because that is the responsibility of the caller of `completion-at-point-functions` to do that according to the completion styles it decides to use.

A function in `completion-at-point-functions` may also return a function instead of a list as described above. In that case, that returned function is called, with no argument, and it is entirely responsible for performing the completion. We discourage this usage; it is only intended to help convert old code to using `completion-at-point`.

The first function in `completion-at-point-functions` to return a non-`nil` value is used by `completion-at-point`. The remaining functions are not called. The exception to this is when there is an `:exclusive` specification, as described above.

The following function provides a convenient way to perform completion on an arbitrary stretch of text in an Emacs buffer:

### Function: **completion-in-region** *start end collection \&optional predicate*

This function completes the text in the current buffer between the positions `start` and `end`, using `collection`. The argument `collection` has the same meaning as in `try-completion` (see [Basic Completion](Basic-Completion.html)).

This function inserts the completion text directly into the current buffer. Unlike `completing-read` (see [Minibuffer Completion](Minibuffer-Completion.html)), it does not activate the minibuffer.

For this function to work, point must be somewhere between `start` and `end`.
