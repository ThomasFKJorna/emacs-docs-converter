

### 29.17 Pop-Up Menus

A Lisp program can pop up a menu so that the user can choose an alternative with the mouse. On a text terminal, if the mouse is not available, the user can choose an alternative using the keyboard motion keys—`C-n`, `C-p`, or up- and down-arrow keys.

### Function: **x-popup-menu** *position menu*

This function displays a pop-up menu and returns an indication of what selection the user makes.

The argument `position` specifies where on the screen to put the top left corner of the menu. It can be either a mouse button event (which says to put the menu where the user actuated the button) or a list of this form:

```lisp
((xoffset yoffset) window)
```

where `xoffset` and `yoffset` are coordinates, measured in pixels, counting from the top left corner of `window`. `window` may be a window or a frame.

If `position` is `t`, it means to use the current mouse position (or the top-left corner of the frame if the mouse is not available on a text terminal). If `position` is `nil`, it means to precompute the key binding equivalents for the keymaps specified in `menu`, without actually displaying or popping up the menu.

The argument `menu` says what to display in the menu. It can be a keymap or a list of keymaps (see [Menu Keymaps](Menu-Keymaps.html)). In this case, the return value is the list of events corresponding to the user’s choice. This list has more than one element if the choice occurred in a submenu. (Note that `x-popup-menu` does not actually execute the command bound to that sequence of events.) On text terminals and toolkits that support menu titles, the title is taken from the prompt string of `menu` if `menu` is a keymap, or from the prompt string of the first keymap in `menu` if it is a list of keymaps (see [Defining Menus](Defining-Menus.html)).

Alternatively, `menu` can have the following form:

```lisp
(title pane1 pane2...)
```

where each pane is a list of form

```lisp
(title item1 item2...)
```

Each `item` should be a cons cell, `(line . value)`, where `line` is a string and `value` is the value to return if that `line` is chosen. Unlike in a menu keymap, a `nil` `value` does not make the menu item non-selectable. Alternatively, each `item` can be a string rather than a cons cell; this makes a non-selectable menu item.

If the user gets rid of the menu without making a valid choice, for instance by clicking the mouse away from a valid choice or by typing `C-g`, then this normally results in a quit and `x-popup-menu` does not return. But if `position` is a mouse button event (indicating that the user invoked the menu with the mouse) then no quit occurs and `x-popup-menu` returns `nil`.

**Usage note:** Don’t use `x-popup-menu` to display a menu if you could do the job with a prefix key defined with a menu keymap. If you use a menu keymap to implement a menu, `C-h c` and `C-h a` can see the individual items in that menu and provide help for them. If instead you implement the menu by defining a command that calls `x-popup-menu`, the help facilities cannot know what happens inside that command, so they cannot give any help for the menu’s items.

The menu bar mechanism, which lets you switch between submenus by moving the mouse, cannot look within the definition of a command to see that it calls `x-popup-menu`. Therefore, if you try to implement a submenu using `x-popup-menu`, it cannot work with the menu bar in an integrated fashion. This is why all menu bar submenus are implemented with menu keymaps within the parent menu, and never with `x-popup-menu`. See [Menu Bar](Menu-Bar.html).

If you want a menu bar submenu to have contents that vary, you should still use a menu keymap to implement it. To make the contents vary, add a hook function to `menu-bar-update-hook` to update the contents of the menu keymap as necessary.
