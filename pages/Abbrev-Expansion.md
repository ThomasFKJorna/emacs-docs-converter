

Next: [Standard Abbrev Tables](Standard-Abbrev-Tables.html), Previous: [Abbrev Files](Abbrev-Files.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 36.4 Looking Up and Expanding Abbreviations

Abbrevs are usually expanded by certain interactive commands, including `self-insert-command`. This section describes the subroutines used in writing such commands, as well as the variables they use for communication.

*   Function: **abbrev-symbol** *abbrev \&optional table*

    This function returns the symbol representing the abbrev named `abbrev`. It returns `nil` if that abbrev is not defined. The optional second argument `table` is the abbrev table in which to look it up. If `table` is `nil`, this function tries first the current buffer’s local abbrev table, and second the global abbrev table.

<!---->

*   Function: **abbrev-expansion** *abbrev \&optional table*

    This function returns the string that `abbrev` would expand into (as defined by the abbrev tables used for the current buffer). It returns `nil` if `abbrev` is not a valid abbrev. The optional argument `table` specifies the abbrev table to use, as in `abbrev-symbol`.

<!---->

*   Command: **expand-abbrev**

    This command expands the abbrev before point, if any. If point does not follow an abbrev, this command does nothing. To do the expansion, it calls the function that is the value of the `abbrev-expand-function` variable, with no arguments, and returns whatever that function does.

    The default expansion function returns the abbrev symbol if it did expansion, and `nil` otherwise. If the abbrev symbol has a hook function that is a symbol whose `no-self-insert` property is non-`nil`, and if the hook function returns `nil` as its value, then the default expansion function returns `nil`, even though expansion did occur.

<!---->

*   Function: **abbrev-insert** *abbrev \&optional name start end*

    This function inserts the abbrev expansion of `abbrev`, replacing the text between `start` and `end`. If `start` is omitted, it defaults to point. `name`, if non-`nil`, should be the name by which this abbrev was found (a string); it is used to figure out whether to adjust the capitalization of the expansion. The function returns `abbrev` if the abbrev was successfully inserted, otherwise it returns `nil`.

<!---->

*   Command: **abbrev-prefix-mark** *\&optional arg*

    This command marks the current location of point as the beginning of an abbrev. The next call to `expand-abbrev` will use the text from here to point (where it is then) as the abbrev to expand, rather than using the previous word as usual.

    First, this command expands any abbrev before point, unless `arg` is non-`nil`. (Interactively, `arg` is the prefix argument.) Then it inserts a hyphen before point, to indicate the start of the next abbrev to be expanded. The actual expansion removes the hyphen.

<!---->

*   User Option: **abbrev-all-caps**

    When this is set non-`nil`, an abbrev entered entirely in upper case is expanded using all upper case. Otherwise, an abbrev entered entirely in upper case is expanded by capitalizing each word of the expansion.

<!---->

*   Variable: **abbrev-start-location**

    The value of this variable is a buffer position (an integer or a marker) for `expand-abbrev` to use as the start of the next abbrev to be expanded. The value can also be `nil`, which means to use the word before point instead. `abbrev-start-location` is set to `nil` each time `expand-abbrev` is called. This variable is also set by `abbrev-prefix-mark`.

<!---->

*   Variable: **abbrev-start-location-buffer**

    The value of this variable is the buffer for which `abbrev-start-location` has been set. Trying to expand an abbrev in any other buffer clears `abbrev-start-location`. This variable is set by `abbrev-prefix-mark`.

<!---->

*   Variable: **last-abbrev**

    This is the `abbrev-symbol` of the most recent abbrev expanded. This information is left by `expand-abbrev` for the sake of the `unexpand-abbrev` command (see [Expanding Abbrevs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Expanding-Abbrevs.html#Expanding-Abbrevs) in The GNU Emacs Manual).

<!---->

*   Variable: **last-abbrev-location**

    This is the location of the most recent abbrev expanded. This contains information left by `expand-abbrev` for the sake of the `unexpand-abbrev` command.

<!---->

*   Variable: **last-abbrev-text**

    This is the exact expansion text of the most recent abbrev expanded, after case conversion (if any). Its value is `nil` if the abbrev has already been unexpanded. This contains information left by `expand-abbrev` for the sake of the `unexpand-abbrev` command.

<!---->

*   Variable: **abbrev-expand-function**

    The value of this variable is a function that `expand-abbrev` will call with no arguments to do the expansion. The function can do anything it wants before and after performing the expansion. It should return the abbrev symbol if expansion took place.

The following sample code shows a simple use of `abbrev-expand-function`. It assumes that `foo-mode` is a mode for editing certain files in which lines that start with ‘`#`’ are comments. You want to use Text mode abbrevs for those lines. The regular local abbrev table, `foo-mode-abbrev-table` is appropriate for all other lines. See [Standard Abbrev Tables](Standard-Abbrev-Tables.html), for the definitions of `local-abbrev-table` and `text-mode-abbrev-table`. See [Advising Functions](Advising-Functions.html), for details of `add-function`.

```lisp
(defun foo-mode-abbrev-expand-function (expand)
  (if (not (save-excursion (forward-line 0) (eq (char-after) ?#)))
      ;; Performs normal expansion.
      (funcall expand)
    ;; We're inside a comment: use the text-mode abbrevs.
    (let ((local-abbrev-table text-mode-abbrev-table))
      (funcall expand))))

(add-hook 'foo-mode-hook
          (lambda ()
            (add-function :around (local 'abbrev-expand-function)
                          #'foo-mode-abbrev-expand-function)))
```

Next: [Standard Abbrev Tables](Standard-Abbrev-Tables.html), Previous: [Abbrev Files](Abbrev-Files.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
