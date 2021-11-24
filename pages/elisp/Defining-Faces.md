

#### 39.12.2 Defining Faces

The usual way to define a face is through the `defface` macro. This macro associates a face name (a symbol) with a default *face spec*. A face spec is a construct which specifies what attributes a face should have on any given terminal; for example, a face spec might specify one foreground color on high-color terminals, and a different foreground color on low-color terminals.

People are sometimes tempted to create a variable whose value is a face name. In the vast majority of cases, this is not necessary; the usual procedure is to define a face with `defface`, and then use its name directly.

Note that once you have defined a face (usually with `defface`), you cannot later undefine this face safely, except by restarting Emacs.

### Macro: **defface** *face spec doc \[keyword value]…*

This macro declares `face` as a named face whose default face spec is given by `spec`. You should not quote the symbol `face`, and it should not end in ‘`-face`’ (that would be redundant). The argument `doc` is a documentation string for the face. The additional `keyword` arguments have the same meanings as in `defgroup` and `defcustom` (see [Common Keywords](Common-Keywords.html)).

If `face` already has a default face spec, this macro does nothing.

The default face spec determines `face`’s appearance when no customizations are in effect (see [Customization](Customization.html)). If `face` has already been customized (via Custom themes or via customizations read from the init file), its appearance is determined by the custom face spec(s), which override the default face spec `spec`. However, if the customizations are subsequently removed, the appearance of `face` will again be determined by its default face spec.

As an exception, if you evaluate a `defface` form with `C-M-x` in Emacs Lisp mode (`eval-defun`), a special feature of `eval-defun` overrides any custom face specs on the face, causing the face to reflect exactly what the `defface` says.

The `spec` argument is a *face spec*, which states how the face should appear on different kinds of terminals. It should be an alist whose elements each have the form

```lisp
(display . plist)
```

`display` specifies a class of terminals (see below). `plist` is a property list of face attributes and their values, specifying how the face appears on such terminals. For backward compatibility, you can also write an element as `(display plist)`.

The `display` part of an element of `spec` determines which terminals the element matches. If more than one element of `spec` matches a given terminal, the first element that matches is the one used for that terminal. There are three possibilities for `display`:

*   `default`

    This element of `spec` doesn’t match any terminal; instead, it specifies defaults that apply to all terminals. This element, if used, must be the first element of `spec`. Each of the following elements can override any or all of these defaults.

*   `t`

    This element of `spec` matches all terminals. Therefore, any subsequent elements of `spec` are never used. Normally `t` is used in the last (or only) element of `spec`.

*   a list

    If `display` is a list, each element should have the form `(characteristic value…)`. Here `characteristic` specifies a way of classifying terminals, and the `value`s are possible classifications which `display` should apply to. Here are the possible values of `characteristic`:

    *   `type`

        The kind of window system the terminal uses—either `graphic` (any graphics-capable display), `x`, `pc` (for the MS-DOS console), `w32` (for MS Windows 9X/NT/2K/XP), or `tty` (a non-graphics-capable display). See [window-system](Window-Systems.html).

    *   `class`

        What kinds of colors the terminal supports—either `color`, `grayscale`, or `mono`.

    *   `background`

        The kind of background—either `light` or `dark`.

    *   `min-colors`

        An integer that represents the minimum number of colors the terminal should support. This matches a terminal if its `display-color-cells` value is at least the specified integer.

    *   `supports`

        Whether or not the terminal can display the face attributes given in `value`… (see [Face Attributes](Face-Attributes.html)). See [Display Face Attribute Testing](Display-Feature-Testing.html#Display-Face-Attribute-Testing), for more information on exactly how this testing is done.

    If an element of `display` specifies more than one `value` for a given `characteristic`, any of those values is acceptable. If `display` has more than one element, each element should specify a different `characteristic`; then *each* characteristic of the terminal must match one of the `value`s specified for it in `display`.

For example, here’s the definition of the standard face `highlight`:

```lisp
(defface highlight
  '((((class color) (min-colors 88) (background light))
     :background "darkseagreen2")
    (((class color) (min-colors 88) (background dark))
     :background "darkolivegreen")
    (((class color) (min-colors 16) (background light))
     :background "darkseagreen2")
    (((class color) (min-colors 16) (background dark))
     :background "darkolivegreen")
    (((class color) (min-colors 8))
     :background "green" :foreground "black")
    (t :inverse-video t))
  "Basic face for highlighting."
  :group 'basic-faces)
```

Internally, Emacs stores each face’s default spec in its `face-defface-spec` symbol property (see [Symbol Properties](Symbol-Properties.html)). The `saved-face` property stores any face spec saved by the user using the customization buffer; the `customized-face` property stores the face spec customized for the current session, but not saved; and the `theme-face` property stores an alist associating the active customization settings and Custom themes with the face specs for that face. The face’s documentation string is stored in the `face-documentation` property.

Normally, a face is declared just once, using `defface`, and any further changes to its appearance are applied using the Customize framework (e.g., via the Customize user interface or via the `custom-set-faces` function; see [Applying Customizations](Applying-Customizations.html)), or by face remapping (see [Face Remapping](Face-Remapping.html)). In the rare event that you need to change a face spec directly from Lisp, you can use the `face-spec-set` function.

### Function: **face-spec-set** *face spec \&optional spec-type*

This function applies `spec` as a face spec for `face`. `spec` should be a face spec, as described in the above documentation for `defface`.

This function also defines `face` as a valid face name if it is not already one, and (re)calculates its attributes on existing frames.

The optional argument `spec-type` determines which spec to set. If it is omitted or `nil` or `face-override-spec`, this function sets the *override spec*, which overrides face specs on `face` of all the other types mentioned below. This is useful when calling this function outside of Custom code. If `spec-type` is `customized-face` or `saved-face`, this function sets the customized spec or the saved custom spec, respectively. If it is `face-defface-spec`, this function sets the default face spec (the same one set by `defface`). If it is `reset`, this function clears out all customization specs and override specs from `face` (in this case, the value of `spec` is ignored). The effect of any other value of `spec-type` on the face specs is reserved for internal use, but the function will still define `face` itself and recalculate its attributes, as described above.
