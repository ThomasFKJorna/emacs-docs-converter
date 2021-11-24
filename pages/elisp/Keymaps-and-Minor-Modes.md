

#### 23.3.2 Keymaps and Minor Modes

Each minor mode can have its own keymap, which is active when the mode is enabled. To set up a keymap for a minor mode, add an element to the alist `minor-mode-map-alist`. See [Definition of minor-mode-map-alist](Controlling-Active-Maps.html#Definition-of-minor_002dmode_002dmap_002dalist).

One use of minor mode keymaps is to modify the behavior of certain self-inserting characters so that they do something else as well as self-insert. (Another way to customize `self-insert-command` is through `post-self-insert-hook`, see [Commands for Insertion](Commands-for-Insertion.html). Apart from this, the facilities for customizing `self-insert-command` are limited to special cases, designed for abbrevs and Auto Fill mode. Do not try substituting your own definition of `self-insert-command` for the standard one. The editor command loop handles this function specially.)

Minor modes may bind commands to key sequences consisting of `C-c` followed by a punctuation character. However, sequences consisting of `C-c` followed by one of `{}<>:;`, or a control character or digit, are reserved for major modes. Also, `C-c letter` is reserved for users. See [Key Binding Conventions](Key-Binding-Conventions.html).
