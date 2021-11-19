

Next: [Describing Characters](Describing-Characters.html), Previous: [Keys in Documentation](Keys-in-Documentation.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 24.4 Text Quoting Style

Typically, grave accents and apostrophes are treated specially in documentation strings and diagnostic messages, and translate to matching single quotation marks (also called “curved quotes”). For example, the documentation string ``"Alias for `foo'."`` and the function call ``(message "Alias for `foo'.")`` both translate to `"Alias for ‘foo’."`. Less commonly, Emacs displays grave accents and apostrophes as themselves, or as apostrophes only (e.g., `"Alias for 'foo'."`). Documentation strings and message formats should be written so that they display well with any of these styles. For example, the documentation string `"Alias for 'foo'."` is probably not what you want, as it can display as `"Alias for ’foo’."`, an unusual style in English.

Sometimes you may need to display a grave accent or apostrophe without translation, regardless of text quoting style. In a documentation string, you can do this with escapes. For example, in the documentation string ``"\\=`(a ,(sin 0)) ==> (a 0.0)"`` the grave accent is intended to denote Lisp code, so it is escaped and displays as itself regardless of quoting style. In a call to `message` or `error`, you can avoid translation by using a format `"%s"` with an argument that is a call to `format`. For example, ``(message "%s" (format "`(a ,(sin %S)) ==> (a %S)" x (sin x)))`` displays a message that starts with grave accent regardless of text quoting style.

*   User Option: **text-quoting-style**

    The value of this user option is a symbol that specifies the style Emacs should use for single quotes in the wording of help and messages. If the option’s value is `curve`, the style is `‘like this’` with curved single quotes. If the value is `straight`, the style is `'like this'` with straight apostrophes. If the value is `grave`, quotes are not translated and the style is `` `like this' `` with grave accent and apostrophe, the standard style before Emacs version 25. The default value `nil` acts like `curve` if curved single quotes seem to be displayable, and like `grave` otherwise.

    This option is useful on platforms that have problems with curved quotes. You can customize it freely according to your personal preference.

Next: [Describing Characters](Describing-Characters.html), Previous: [Keys in Documentation](Keys-in-Documentation.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
