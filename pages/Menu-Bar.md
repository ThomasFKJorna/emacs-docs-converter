

Next: [Tool Bar](Tool-Bar.html), Previous: [Menu Example](Menu-Example.html), Up: [Menu Keymaps](Menu-Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 22.17.5 The Menu Bar

Emacs usually shows a *menu bar* at the top of each frame. See [Menu Bars](https://www.gnu.org/software/emacs/manual/html_node/emacs/Menu-Bars.html#Menu-Bars) in The GNU Emacs Manual. Menu bar items are subcommands of the fake function key `MENU-BAR`, as defined in the active keymaps.

To add an item to the menu bar, invent a fake function key of your own (let’s call it `key`), and make a binding for the key sequence `[menu-bar key]`. Most often, the binding is a menu keymap, so that pressing a button on the menu bar item leads to another menu.

When more than one active keymap defines the same function key for the menu bar, the item appears just once. If the user clicks on that menu bar item, it brings up a single, combined menu containing all the subcommands of that item—the global subcommands, the local subcommands, and the minor mode subcommands.

The variable `overriding-local-map` is normally ignored when determining the menu bar contents. That is, the menu bar is computed from the keymaps that would be active if `overriding-local-map` were `nil`. See [Active Keymaps](Active-Keymaps.html).

Here’s an example of setting up a menu bar item:

```lisp
;; Make a menu keymap (with a prompt string)
;; and make it the menu bar item’s definition.
(define-key global-map [menu-bar words]
  (cons "Words" (make-sparse-keymap "Words")))
```

```lisp
```

```lisp
;; Define specific subcommands in this menu.
(define-key global-map
  [menu-bar words forward]
  '("Forward word" . forward-word))
```

```lisp
(define-key global-map
  [menu-bar words backward]
  '("Backward word" . backward-word))
```

A local keymap can cancel a menu bar item made by the global keymap by rebinding the same fake function key with `undefined` as the binding. For example, this is how Dired suppresses the ‘`Edit`’ menu bar item:

```lisp
(define-key dired-mode-map [menu-bar edit] 'undefined)
```

Here, `edit` is the symbol produced by a fake function key, it is used by the global map for the ‘`Edit`’ menu bar item. The main reason to suppress a global menu bar item is to regain space for mode-specific items.

*   Variable: **menu-bar-final-items**

    Normally the menu bar shows global items followed by items defined by the local maps.

    This variable holds a list of fake function keys for items to display at the end of the menu bar rather than in normal sequence. The default value is `(help-menu)`; thus, the ‘`Help`’ menu item normally appears at the end of the menu bar, following local menu items.

<!---->

*   Variable: **menu-bar-update-hook**

    This normal hook is run by redisplay to update the menu bar contents, before redisplaying the menu bar. You can use it to update menus whose contents should vary. Since this hook is run frequently, we advise you to ensure that the functions it calls do not take much time in the usual case.

Next to every menu bar item, Emacs displays a key binding that runs the same command (if such a key binding exists). This serves as a convenient hint for users who do not know the key binding. If a command has multiple bindings, Emacs normally displays the first one it finds. You can specify one particular key binding by assigning an `:advertised-binding` symbol property to the command. See [Keys in Documentation](Keys-in-Documentation.html).

Next: [Tool Bar](Tool-Bar.html), Previous: [Menu Example](Menu-Example.html), Up: [Menu Keymaps](Menu-Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
