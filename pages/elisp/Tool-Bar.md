

#### 22.17.6 Tool bars

A *tool bar* is a row of clickable icons at the top of a frame, just below the menu bar. See [Tool Bars](https://www.gnu.org/software/emacs/manual/html_node/emacs/Tool-Bars.html#Tool-Bars) in The GNU Emacs Manual. Emacs normally shows a tool bar on graphical displays.

On each frame, the frame parameter `tool-bar-lines` controls how many lines’ worth of height to reserve for the tool bar. A zero value suppresses the tool bar. If the value is nonzero, and `auto-resize-tool-bars` is non-`nil`, the tool bar expands and contracts automatically as needed to hold the specified contents. If the value is `grow-only`, the tool bar expands automatically, but does not contract automatically.

The tool bar contents are controlled by a menu keymap attached to a fake function key called `TOOL-BAR` (much like the way the menu bar is controlled). So you define a tool bar item using `define-key`, like this:

```lisp
(define-key global-map [tool-bar key] item)
```

where `key` is a fake function key to distinguish this item from other items, and `item` is a menu item key binding (see [Extended Menu Items](Extended-Menu-Items.html)), which says how to display this item and how it behaves.

The usual menu keymap item properties, `:visible`, `:enable`, `:button`, and `:filter`, are useful in tool bar bindings and have their normal meanings. The `real-binding` in the item must be a command, not a keymap; in other words, it does not work to define a tool bar icon as a prefix key.

The `:help` property specifies a help-echo string to display while the mouse is on that item. This is displayed in the same way as `help-echo` text properties (see [Help display](Special-Properties.html#Help-display)).

In addition, you should use the `:image` property; this is how you specify the image to display in the tool bar:

### `:image image`

`image` is either a single image specification (see [Images](Images.html)) or a vector of four image specifications. If you use a vector of four, one of them is used, depending on circumstances:

*   item 0

    Used when the item is enabled and selected.

*   item 1

    Used when the item is enabled and deselected.

*   item 2

    Used when the item is disabled and selected.

*   item 3

    Used when the item is disabled and deselected.

The GTK+ and NS versions of Emacs ignores items 1 to 3, because disabled and/or deselected images are autocomputed from item 0.

If `image` is a single image specification, Emacs draws the tool bar button in disabled state by applying an edge-detection algorithm to the image.

The `:rtl` property specifies an alternative image to use for right-to-left languages. Only the GTK+ version of Emacs supports this at present.

Like the menu bar, the tool bar can display separators (see [Menu Separators](Menu-Separators.html)). Tool bar separators are vertical rather than horizontal, though, and only a single style is supported. They are represented in the tool bar keymap by `(menu-item "--")` entries; properties like `:visible` are not supported for tool bar separators. Separators are rendered natively in GTK+ and Nextstep tool bars; in the other cases, they are rendered using an image of a vertical line.

The default tool bar is defined so that items specific to editing do not appear for major modes whose command symbol has a `mode-class` property of `special` (see [Major Mode Conventions](Major-Mode-Conventions.html)). Major modes may add items to the global bar by binding `[tool-bar foo]` in their local map. It makes sense for some major modes to replace the default tool bar items completely, since not many can be accommodated conveniently, and the default bindings make this easy by using an indirection through `tool-bar-map`.

### Variable: **tool-bar-map**

By default, the global map binds `[tool-bar]` as follows:

```lisp
(global-set-key [tool-bar]
                `(menu-item ,(purecopy "tool bar") ignore
                            :filter tool-bar-make-keymap))
```

The function `tool-bar-make-keymap`, in turn, derives the actual tool bar map dynamically from the value of the variable `tool-bar-map`. Hence, you should normally adjust the default (global) tool bar by changing that map. Some major modes, such as Info mode, completely replace the global tool bar by making `tool-bar-map` buffer-local and setting it to a different keymap.

There are two convenience functions for defining tool bar items, as follows.

### Function: **tool-bar-add-item** *icon def key \&rest props*

This function adds an item to the tool bar by modifying `tool-bar-map`. The image to use is defined by `icon`, which is the base name of an XPM, XBM or PBM image file to be located by `find-image`. Given a value ‘`"exit"`’, say, `exit.xpm`, `exit.pbm` and `exit.xbm` would be searched for in that order on a color display. On a monochrome display, the search order is ‘`.pbm`’, ‘`.xbm`’ and ‘`.xpm`’. The binding to use is the command `def`, and `key` is the fake function key symbol in the prefix keymap. The remaining arguments `props` are additional property list elements to add to the menu item specification.

To define items in some local map, bind `tool-bar-map` with `let` around calls of this function:

```lisp
(defvar foo-tool-bar-map
  (let ((tool-bar-map (make-sparse-keymap)))
    (tool-bar-add-item …)
    …
    tool-bar-map))
```

### Function: **tool-bar-add-item-from-menu** *command icon \&optional map \&rest props*

This function is a convenience for defining tool bar items which are consistent with existing menu bar bindings. The binding of `command` is looked up in the menu bar in `map` (default `global-map`) and modified to add an image specification for `icon`, which is found in the same way as by `tool-bar-add-item`. The resulting binding is then placed in `tool-bar-map`, so use this function only for global tool bar items.

`map` must contain an appropriate keymap bound to `[menu-bar]`. The remaining arguments `props` are additional property list elements to add to the menu item specification.

### Function: **tool-bar-local-item-from-menu** *command icon in-map \&optional from-map \&rest props*

This function is used for making non-global tool bar items. Use it like `tool-bar-add-item-from-menu` except that `in-map` specifies the local map to make the definition in. The argument `from-map` is like the `map` argument of `tool-bar-add-item-from-menu`.

### Variable: **auto-resize-tool-bars**

If this variable is non-`nil`, the tool bar automatically resizes to show all defined tool bar items—but not larger than a quarter of the frame’s height.

If the value is `grow-only`, the tool bar expands automatically, but does not contract automatically. To contract the tool bar, the user has to redraw the frame by entering `C-l`.

If Emacs is built with GTK+ or Nextstep, the tool bar can only show one line, so this variable has no effect.

### Variable: **auto-raise-tool-bar-buttons**

If this variable is non-`nil`, tool bar items display in raised form when the mouse moves over them.

### Variable: **tool-bar-button-margin**

This variable specifies an extra margin to add around tool bar items. The value is an integer, a number of pixels. The default is 4.

### Variable: **tool-bar-button-relief**

This variable specifies the shadow width for tool bar items. The value is an integer, a number of pixels. The default is 1.

### Variable: **tool-bar-border**

This variable specifies the height of the border drawn below the tool bar area. An integer specifies height as a number of pixels. If the value is one of `internal-border-width` (the default) or `border-width`, the tool bar border height corresponds to the corresponding frame parameter.

You can define a special meaning for clicking on a tool bar item with the shift, control, meta, etc., modifiers. You do this by setting up additional items that relate to the original item through the fake function keys. Specifically, the additional items should use the modified versions of the same fake function key used to name the original item.

Thus, if the original item was defined this way,

```lisp
(define-key global-map [tool-bar shell]
  '(menu-item "Shell" shell
              :image (image :type xpm :file "shell.xpm")))
```

then here is how you can define clicking on the same tool bar image with the shift modifier:

```lisp
(define-key global-map [tool-bar S-shell] 'some-command)
```

See [Function Keys](Function-Keys.html), for more information about how to add modifiers to function keys.
