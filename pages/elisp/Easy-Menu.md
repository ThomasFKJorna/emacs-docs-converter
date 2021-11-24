

#### 22.17.8 Easy Menu

The following macro provides a convenient way to define pop-up menus and/or menu bar menus.

### Macro: **easy-menu-define** *symbol maps doc menu*

This macro defines a pop-up menu and/or menu bar submenu, whose contents are given by `menu`.

If `symbol` is non-`nil`, it should be a symbol; then this macro defines `symbol` as a function for popping up the menu (see [Pop-Up Menus](Pop_002dUp-Menus.html)), with `doc` as its documentation string. `symbol` should not be quoted.

Regardless of the value of `symbol`, if `maps` is a keymap, the menu is added to that keymap, as a top-level menu for the menu bar (see [Menu Bar](Menu-Bar.html)). It can also be a list of keymaps, in which case the menu is added separately to each of those keymaps.

The first element of `menu` must be a string, which serves as the menu label. It may be followed by any number of the following keyword-argument pairs:

*   `:filter function`

    `function` must be a function which, if called with one argument—the list of the other menu items—returns the actual items to be displayed in the menu.

*   `:visible include`

    `include` is an expression; if it evaluates to `nil`, the menu is made invisible. `:included` is an alias for `:visible`.

*   `:active enable`

    `enable` is an expression; if it evaluates to `nil`, the menu is not selectable. `:enable` is an alias for `:active`.

The remaining elements in `menu` are menu items.

A menu item can be a vector of three elements, `[name callback enable]`. `name` is the menu item name (a string). `callback` is a command to run, or an expression to evaluate, when the item is chosen. `enable` is an expression; if it evaluates to `nil`, the item is disabled for selection.

Alternatively, a menu item may have the form:

```lisp
   [ name callback [ keyword arg ]... ]
```

where `name` and `callback` have the same meanings as above, and each optional `keyword` and `arg` pair should be one of the following:

*   `:keys keys`

    `keys` is a string to display as keyboard equivalent to the menu item. This is normally not needed, as keyboard equivalents are computed automatically. `keys` is expanded with `substitute-command-keys` before it is displayed (see [Keys in Documentation](Keys-in-Documentation.html)).

*   `:key-sequence keys`

    `keys` is a hint indicating which key sequence to display as keyboard equivalent, in case the command is bound to several key sequences. It has no effect if `keys` is not bound to same command as this menu item.

*   `:active enable`

    `enable` is an expression; if it evaluates to `nil`, the item is make unselectable.. `:enable` is an alias for `:active`.

*   `:visible include`

    `include` is an expression; if it evaluates to `nil`, the item is made invisible. `:included` is an alias for `:visible`.

*   `:label form`

    `form` is an expression that is evaluated to obtain a value which serves as the menu item’s label (the default is `name`).

*   `:suffix form`

    `form` is an expression that is dynamically evaluated and whose value is concatenated with the menu entry’s label.

*   `:style style`

    `style` is a symbol describing the type of menu item; it should be `toggle` (a checkbox), or `radio` (a radio button), or anything else (meaning an ordinary menu item).

*   `:selected selected`

    `selected` is an expression; the checkbox or radio button is selected whenever the expression’s value is non-`nil`.

*   `:help help`

    `help` is a string describing the menu item.

Alternatively, a menu item can be a string. Then that string appears in the menu as unselectable text. A string consisting of dashes is displayed as a separator (see [Menu Separators](Menu-Separators.html)).

Alternatively, a menu item can be a list with the same format as `menu`. This is a submenu.

Here is an example of using `easy-menu-define` to define a menu similar to the one defined in the example in [Menu Bar](Menu-Bar.html):

```lisp
(easy-menu-define words-menu global-map
  "Menu for word navigation commands."
  '("Words"
     ["Forward word" forward-word]
     ["Backward word" backward-word]))
```
