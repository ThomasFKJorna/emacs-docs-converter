

Next: [Extended Menu Items](Extended-Menu-Items.html), Up: [Defining Menus](Defining-Menus.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 22.17.1.1 Simple Menu Items

The simpler (and original) way to define a menu item is to bind some event type (it doesn’t matter what event type) to a binding like this:

```lisp
(item-string . real-binding)
```

The CAR, `item-string`, is the string to be displayed in the menu. It should be short—preferably one to three words. It should describe the action of the command it corresponds to. Note that not all graphical toolkits can display non-ASCII text in menus (it will work for keyboard menus and will work to a large extent with the GTK+ toolkit).

You can also supply a second string, called the help string, as follows:

```lisp
(item-string help . real-binding)
```

`help` specifies a help-echo string to display while the mouse is on that item in the same way as `help-echo` text properties (see [Help display](Special-Properties.html#Help-display)).

As far as `define-key` is concerned, `item-string` and `help-string` are part of the event’s binding. However, `lookup-key` returns just `real-binding`, and only `real-binding` is used for executing the key.

If `real-binding` is `nil`, then `item-string` appears in the menu but cannot be selected.

If `real-binding` is a symbol and has a non-`nil` `menu-enable` property, that property is an expression that controls whether the menu item is enabled. Every time the keymap is used to display a menu, Emacs evaluates the expression, and it enables the menu item only if the expression’s value is non-`nil`. When a menu item is disabled, it is displayed in a fuzzy fashion, and cannot be selected.

The menu bar does not recalculate which items are enabled every time you look at a menu. This is because the X toolkit requires the whole tree of menus in advance. To force recalculation of the menu bar, call `force-mode-line-update` (see [Mode Line Format](Mode-Line-Format.html)).

Next: [Extended Menu Items](Extended-Menu-Items.html), Up: [Defining Menus](Defining-Menus.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
