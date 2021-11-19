

Next: [Case Conversion](Case-Conversion.html), Previous: [Formatting Strings](Formatting-Strings.html), Up: [Strings and Characters](Strings-and-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 4.8 Custom Format Strings

Sometimes it is useful to allow users and Lisp programs alike to control how certain text is generated via custom format control strings. For example, a format string could control how to display someone’s forename, surname, and email address. Using the function `format` described in the previous section, the format string could be something like `"%s %s <%s>"`. This approach quickly becomes impractical, however, as it can be unclear which specification character corresponds to which piece of information.

A more convenient format string for such cases would be something like `"%f %l <%e>"`, where each specification character carries more semantic information and can easily be rearranged relative to other specification characters, making such format strings more easily customizable by the user.

The function `format-spec` described in this section performs a similar function to `format`, except it operates on format control strings that use arbitrary specification characters.

*   Function: **format-spec** *template spec-alist \&optional only-present*

    This function returns a string produced from the format string `template` according to conversions specified in `spec-alist`, which is an alist (see [Association Lists](Association-Lists.html)) of the form `(letter . replacement)`. Each specification `%letter` in `template` will be replaced by `replacement` when formatting the resulting string.

    The characters in `template`, other than the format specifications, are copied directly into the output, including their text properties, if any. Any text properties of the format specifications are copied to their replacements.

    Using an alist to specify conversions gives rise to some useful properties:

    *   If `spec-alist` contains more unique `letter` keys than there are unique specification characters in `template`, the unused keys are simply ignored.
    *   If `spec-alist` contains more than one association with the same `letter`, the closest one to the start of the list is used.
    *   If `template` contains the same specification character more than once, then the same `replacement` found in `spec-alist` is used as a basis for all of that character’s substitutions.
    *   The order of specifications in `template` need not correspond to the order of associations in `spec-alist`.

    The optional argument `only-present` indicates how to handle specification characters in `template` that are not found in `spec-alist`. If it is `nil` or omitted, the function signals an error. Otherwise, those format specifications and any occurrences of ‘`%%`’ in `template` are left verbatim in the output, including their text properties, if any.

The syntax of format specifications accepted by `format-spec` is similar, but not identical, to that accepted by `format`. In both cases, a format specification is a sequence of characters beginning with ‘`%`’ and ending with an alphabetic letter such as ‘`s`’.

Unlike `format`, which assigns specific meanings to a fixed set of specification characters, `format-spec` accepts arbitrary specification characters and treats them all equally. For example:

```lisp
(setq my-site-info
      (list (cons ?s system-name)
            (cons ?t (symbol-name system-type))
            (cons ?c system-configuration)
            (cons ?v emacs-version)
            (cons ?e invocation-name)
            (cons ?p (number-to-string (emacs-pid)))
            (cons ?a user-mail-address)
            (cons ?n user-full-name)))

(format-spec "%e %v (%c)" my-site-info)
     ⇒ "emacs 27.1 (x86_64-pc-linux-gnu)"

(format-spec "%n <%a>" my-site-info)
     ⇒ "Emacs Developers <emacs-devel@gnu.org>"
```

A format specification can include any number of the following flag characters immediately after the ‘`%`’ to modify aspects of the substitution.

*   ‘`0`’

    This flag causes any padding specified by the width to consist of ‘`0`’ characters instead of spaces.

*   ‘`-`’

    This flag causes any padding specified by the width to be inserted on the right rather than the left.

*   ‘`<`’

    This flag causes the substitution to be truncated on the left to the given width, if specified.

*   ‘`>`’

    This flag causes the substitution to be truncated on the right to the given width, if specified.

*   ‘`^`’

    This flag converts the substituted text to upper case (see [Case Conversion](Case-Conversion.html)).

*   ‘`_`’

    This flag converts the substituted text to lower case (see [Case Conversion](Case-Conversion.html)).

The result of using contradictory flags (for instance, both upper and lower case) is undefined.

As is the case with `format`, a format specification can include a width, which is a decimal number that appears after any flags. If a substitution contains fewer characters than its specified width, it is padded on the left:

```lisp
(format-spec "%8a is padded on the left with spaces"
             '((?a . "alpha")))
     ⇒ "   alpha is padded on the left with spaces"
```

Here is a more complicated example that combines several aforementioned features:

```lisp
(setq my-battery-info
      (list (cons ?p "73")      ; Percentage
            (cons ?L "Battery") ; Status
            (cons ?t "2:23")    ; Remaining time
            (cons ?c "24330")   ; Capacity
            (cons ?r "10.6")))  ; Rate of discharge

(format-spec "%>^-3L : %3p%% (%05t left)" my-battery-info)
     ⇒ "BAT :  73% (02:23 left)"

(format-spec "%>^-3L : %3p%% (%05t left)"
             (cons (cons ?L "AC")
                   my-battery-info))
     ⇒ "AC  :  73% (02:23 left)"
```

As the examples in this section illustrate, `format-spec` is often used for selectively formatting an assortment of different pieces of information. This is useful in programs that provide user-customizable format strings, as the user can choose to format with a regular syntax and in any desired order only a subset of the information that the program makes available.

Next: [Case Conversion](Case-Conversion.html), Previous: [Formatting Strings](Formatting-Strings.html), Up: [Strings and Characters](Strings-and-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
