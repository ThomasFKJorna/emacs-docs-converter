

#### 22.17.3 Menus and the Keyboard

When a prefix key ending with a keyboard event (a character or function key) has a definition that is a menu keymap, the keymap operates as a keyboard menu; the user specifies the next event by choosing a menu item with the keyboard.

Emacs displays the keyboard menu with the map’s overall prompt string, followed by the alternatives (the item strings of the map’s bindings), in the echo area. If the bindings don’t all fit at once, the user can type `SPC` to see the next line of alternatives. Successive uses of `SPC` eventually get to the end of the menu and then cycle around to the beginning. (The variable `menu-prompt-more-char` specifies which character is used for this; `SPC` is the default.)

When the user has found the desired alternative from the menu, he or she should type the corresponding character—the one whose binding is that alternative.

### Variable: **menu-prompt-more-char**

This variable specifies the character to use to ask to see the next line of a menu. Its initial value is 32, the code for `SPC`.
