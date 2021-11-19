

Next: [Display Feature Testing](Display-Feature-Testing.html), Previous: [Text Terminal Colors](Text-Terminal-Colors.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.24 X Resources

This section describes some of the functions and variables for querying and using X resources, or their equivalent on your operating system. See [X Resources](https://www.gnu.org/software/emacs/manual/html_node/emacs/X-Resources.html#X-Resources) in The GNU Emacs Manual, for more information about X resources.

*   Function: **x-get-resource** *attribute class \&optional component subclass*

    The function `x-get-resource` retrieves a resource value from the X Window defaults database.

    Resources are indexed by a combination of a *key* and a *class*. This function searches using a key of the form ‘`instance.attribute`’ (where `instance` is the name under which Emacs was invoked), and using ‘`Emacs.class`’ as the class.

    The optional arguments `component` and `subclass` add to the key and the class, respectively. You must specify both of them or neither. If you specify them, the key is ‘`instance.component.attribute`’, and the class is ‘`Emacs.class.subclass`’.

<!---->

*   Variable: **x-resource-class**

    This variable specifies the application name that `x-get-resource` should look up. The default value is `"Emacs"`. You can examine X resources for other application names by binding this variable to some other string, around a call to `x-get-resource`.

<!---->

*   Variable: **x-resource-name**

    This variable specifies the instance name that `x-get-resource` should look up. The default value is the name Emacs was invoked with, or the value specified with the ‘`-name`’ or ‘`-rn`’ switches.

To illustrate some of the above, suppose that you have the line:

```lisp
xterm.vt100.background: yellow
```

in your X resources file (whose name is usually `~/.Xdefaults` or `~/.Xresources`). Then:

```lisp
(let ((x-resource-class "XTerm") (x-resource-name "xterm"))
  (x-get-resource "vt100.background" "VT100.Background"))
     ⇒ "yellow"
```

```lisp
(let ((x-resource-class "XTerm") (x-resource-name "xterm"))
  (x-get-resource "background" "VT100" "vt100" "Background"))
     ⇒ "yellow"
```

*   Variable: **inhibit-x-resources**

    If this variable is non-`nil`, Emacs does not look up X resources, and X resources do not have any effect when creating new frames.

Next: [Display Feature Testing](Display-Feature-Testing.html), Previous: [Text Terminal Colors](Text-Terminal-Colors.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
