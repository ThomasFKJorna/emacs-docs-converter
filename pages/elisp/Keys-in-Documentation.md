

### 24.3 Substituting Key Bindings in Documentation

When documentation strings refer to key sequences, they should use the current, actual key bindings. They can do so using certain special text sequences described below. Accessing documentation strings in the usual way substitutes current key binding information for these special sequences. This works by calling `substitute-command-keys`. You can also call that function yourself.

Here is a list of the special sequences and what they mean:

`\[command]`

stands for a key sequence that will invoke `command`, or ‘`M-x command`’ if `command` has no key bindings.

`\{mapvar}`

stands for a summary of the keymap which is the value of the variable `mapvar`. The summary is made using `describe-bindings`.

`\<mapvar>`

stands for no text itself. It is used only for a side effect: it specifies `mapvar`’s value as the keymap for any following ‘`\[command]`’ sequences in this documentation string.

`` ` ``

(grave accent) stands for a left quote. This generates a left single quotation mark, an apostrophe, or a grave accent depending on the value of `text-quoting-style`. See [Text Quoting Style](Text-Quoting-Style.html).

`'`

(apostrophe) stands for a right quote. This generates a right single quotation mark or an apostrophe depending on the value of `text-quoting-style`.

`\=`

quotes the following character and is discarded; thus, ‘`` \=` ``’ puts ‘`` ` ``’ into the output, ‘`\=\[`’ puts ‘`\[`’ into the output, and ‘`\=\=`’ puts ‘`\=`’ into the output.

**Please note:** Each ‘`\`’ must be doubled when written in a string in Emacs Lisp.

### User Option: **text-quoting-style**

The value of this variable is a symbol that specifies the style Emacs should use for single quotes in the wording of help and messages. If the variable’s value is `curve`, the style is `‘like this’` with curved single quotes. If the value is `straight`, the style is `'like this'` with straight apostrophes. If the value is `grave`, quotes are not translated and the style is `` `like this' `` with grave accent and apostrophe, the standard style before Emacs version 25. The default value `nil` acts like `curve` if curved single quotes seem to be displayable, and like `grave` otherwise.

This option is useful on platforms that have problems with curved quotes. You can customize it freely according to your personal preference.

### Function: **substitute-command-keys** *string*

This function scans `string` for the above special sequences and replaces them by what they stand for, returning the result as a string. This permits display of documentation that refers accurately to the user’s own customized key bindings.

If a command has multiple bindings, this function normally uses the first one it finds. You can specify one particular key binding by assigning an `:advertised-binding` symbol property to the command, like this:

```lisp
(put 'undo :advertised-binding [?\C-/])
```

The `:advertised-binding` property also affects the binding shown in menu items (see [Menu Bar](Menu-Bar.html)). The property is ignored if it specifies a key binding that the command does not actually have.

Here are examples of the special sequences:

```lisp
(substitute-command-keys
   "To abort recursive edit, type `\\[abort-recursive-edit]'.")
⇒ "To abort recursive edit, type ‘C-]’."
```

```lisp
```

```lisp
(substitute-command-keys
   "The keys that are defined for the minibuffer here are:
  \\{minibuffer-local-must-match-map}")
⇒ "The keys that are defined for the minibuffer here are:
```

```lisp

?               minibuffer-completion-help
SPC             minibuffer-complete-word
TAB             minibuffer-complete
C-j             minibuffer-complete-and-exit
RET             minibuffer-complete-and-exit
C-g             abort-recursive-edit
"
```

```lisp
(substitute-command-keys
   "To abort a recursive edit from the minibuffer, type \
`\\<minibuffer-local-must-match-map>\\[abort-recursive-edit]'.")
⇒ "To abort a recursive edit from the minibuffer, type ‘C-g’."
```

There are other special conventions for the text in documentation strings—for instance, you can refer to functions, variables, and sections of this manual. See [Documentation Tips](Documentation-Tips.html), for details.
