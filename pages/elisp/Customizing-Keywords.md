

#### 23.6.3 Customizing Search-Based Fontification

You can use `font-lock-add-keywords` to add additional search-based fontification rules to a major mode, and `font-lock-remove-keywords` to remove rules.

### Function: **font-lock-add-keywords** *mode keywords \&optional how*

This function adds highlighting `keywords`, for the current buffer or for major mode `mode`. The argument `keywords` should be a list with the same format as the variable `font-lock-keywords`.

If `mode` is a symbol which is a major mode command name, such as `c-mode`, the effect is that enabling Font Lock mode in `mode` will add `keywords` to `font-lock-keywords`. Calling with a non-`nil` value of `mode` is correct only in your `~/.emacs` file.

If `mode` is `nil`, this function adds `keywords` to `font-lock-keywords` in the current buffer. This way of calling `font-lock-add-keywords` is usually used in mode hook functions.

By default, `keywords` are added at the beginning of `font-lock-keywords`. If the optional argument `how` is `set`, they are used to replace the value of `font-lock-keywords`. If `how` is any other non-`nil` value, they are added at the end of `font-lock-keywords`.

Some modes provide specialized support you can use in additional highlighting patterns. See the variables `c-font-lock-extra-types`, `c++-font-lock-extra-types`, and `java-font-lock-extra-types`, for example.

**Warning:** Major mode commands must not call `font-lock-add-keywords` under any circumstances, either directly or indirectly, except through their mode hooks. (Doing so would lead to incorrect behavior for some minor modes.) They should set up their rules for search-based fontification by setting `font-lock-keywords`.

### Function: **font-lock-remove-keywords** *mode keywords*

This function removes `keywords` from `font-lock-keywords` for the current buffer or for major mode `mode`. As in `font-lock-add-keywords`, `mode` should be a major mode command name or `nil`. All the caveats and requirements for `font-lock-add-keywords` apply here too. The argument `keywords` must exactly match the one used by the corresponding `font-lock-add-keywords`.

For example, the following code adds two fontification patterns for C mode: one to fontify the word ‘`FIXME`’, even in comments, and another to fontify the words ‘`and`’, ‘`or`’ and ‘`not`’ as keywords.

```lisp
(font-lock-add-keywords 'c-mode
 '(("\\<\\(FIXME\\):" 1 font-lock-warning-face prepend)
   ("\\<\\(and\\|or\\|not\\)\\>" . font-lock-keyword-face)))
```

This example affects only C mode proper. To add the same patterns to C mode *and* all modes derived from it, do this instead:

```lisp
(add-hook 'c-mode-hook
 (lambda ()
  (font-lock-add-keywords nil
   '(("\\<\\(FIXME\\):" 1 font-lock-warning-face prepend)
     ("\\<\\(and\\|or\\|not\\)\\>" .
      font-lock-keyword-face)))))
```
