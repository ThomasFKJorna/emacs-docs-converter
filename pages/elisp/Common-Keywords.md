

### 15.1 Common Item Keywords

The customization declarations that we will describe in the next few sections—`defcustom`, `defgroup`, etc.—all accept keyword arguments (see [Constant Variables](Constant-Variables.html)) for specifying various information. This section describes keywords that apply to all types of customization declarations.

All of these keywords, except `:tag`, can be used more than once in a given item. Each use of the keyword has an independent effect. The keyword `:tag` is an exception because any given item can only display one name.

### `:tag label`

Use `label`, a string, instead of the item’s name, to label the item in customization menus and buffers. **Don’t use a tag which is substantially different from the item’s real name; that would cause confusion.**

### `:group group`

Put this customization item in group `group`. If this keyword is missing from a customization item, it’ll be placed in the same group that was last defined (in the current file).

When you use `:group` in a `defgroup`, it makes the new group a subgroup of `group`.

If you use this keyword more than once, you can put a single item into more than one group. Displaying any of those groups will show this item. Please don’t overdo this, since the result would be annoying.

### `:link link-data`

Include an external link after the documentation string for this item. This is a sentence containing a button that references some other documentation.

There are several alternatives you can use for `link-data`:

*   `(custom-manual info-node)`

    Link to an Info node; `info-node` is a string which specifies the node name, as in `"(emacs)Top"`. The link appears as ‘`[Manual]`’ in the customization buffer and enters the built-in Info reader on `info-node`.

*   `(info-link info-node)`

    Like `custom-manual` except that the link appears in the customization buffer with the Info node name.

*   `(url-link url)`

    Link to a web page; `url` is a string which specifies the URL. The link appears in the customization buffer as `url` and invokes the WWW browser specified by `browse-url-browser-function`.

*   `(emacs-commentary-link library)`

    Link to the commentary section of a library; `library` is a string which specifies the library name. See [Library Headers](Library-Headers.html).

*   `(emacs-library-link library)`

    Link to an Emacs Lisp library file; `library` is a string which specifies the library name.

*   `(file-link file)`

    Link to a file; `file` is a string which specifies the name of the file to visit with `find-file` when the user invokes this link.

*   `(function-link function)`

    Link to the documentation of a function; `function` is a string which specifies the name of the function to describe with `describe-function` when the user invokes this link.

*   `(variable-link variable)`

    Link to the documentation of a variable; `variable` is a string which specifies the name of the variable to describe with `describe-variable` when the user invokes this link.

*   `(custom-group-link group)`

    Link to another customization group. Invoking it creates a new customization buffer for `group`.

You can specify the text to use in the customization buffer by adding `:tag name` after the first element of the `link-data`; for example, `(info-link :tag "foo" "(emacs)Top")` makes a link to the Emacs manual which appears in the buffer as ‘`foo`’.

You can use this keyword more than once, to add multiple links.

### `:load file`

Load file `file` (a string) before displaying this customization item (see [Loading](Loading.html)). Loading is done with `load`, and only if the file is not already loaded.

### `:require feature`

Execute `(require 'feature)` when your saved customizations set the value of this item. `feature` should be a symbol.

The most common reason to use `:require` is when a variable enables a feature such as a minor mode, and just setting the variable won’t have any effect unless the code which implements the mode is loaded.

### `:version version`

This keyword specifies that the item was first introduced in Emacs version `version`, or that its default value was changed in that version. The value `version` must be a string.

### `:package-version '(package . version)`

This keyword specifies that the item was first introduced in `package` version `version`, or that its meaning or default value was changed in that version. This keyword takes priority over `:version`.

`package` should be the official name of the package, as a symbol (e.g., `MH-E`). `version` should be a string. If the package `package` is released as part of Emacs, `package` and `version` should appear in the value of `customize-package-emacs-version-alist`.

Packages distributed as part of Emacs that use the `:package-version` keyword must also update the `customize-package-emacs-version-alist` variable.

### Variable: **customize-package-emacs-version-alist**

This alist provides a mapping for the versions of Emacs that are associated with versions of a package listed in the `:package-version` keyword. Its elements are:

```lisp
(package (pversion . eversion)…)
```

For each `package`, which is a symbol, there are one or more elements that contain a package version `pversion` with an associated Emacs version `eversion`. These versions are strings. For example, the MH-E package updates this alist with the following:

```lisp
(add-to-list 'customize-package-emacs-version-alist
             '(MH-E ("6.0" . "22.1") ("6.1" . "22.1") ("7.0" . "22.1")
                    ("7.1" . "22.1") ("7.2" . "22.1") ("7.3" . "22.1")
                    ("7.4" . "22.1") ("8.0" . "22.1")))
```

The value of `package` needs to be unique and it needs to match the `package` value appearing in the `:package-version` keyword. Since the user might see the value in an error message, a good choice is the official name of the package, such as MH-E or Gnus.
