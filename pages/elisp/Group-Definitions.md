

### 15.2 Defining Customization Groups

Each Emacs Lisp package should have one main customization group which contains all the options, faces and other groups in the package. If the package has a small number of options and faces, use just one group and put everything in it. When there are more than twenty or so options and faces, then you should structure them into subgroups, and put the subgroups under the package’s main customization group. It is OK to put some of the options and faces in the package’s main group alongside the subgroups.

The package’s main or only group should be a member of one or more of the standard customization groups. (To display the full list of them, use `M-x customize`.) Choose one or more of them (but not too many), and add your group to each of them using the `:group` keyword.

The way to declare new customization groups is with `defgroup`.

### Macro: **defgroup** *group members doc \[keyword value]…*

Declare `group` as a customization group containing `members`. Do not quote the symbol `group`. The argument `doc` specifies the documentation string for the group.

The argument `members` is a list specifying an initial set of customization items to be members of the group. However, most often `members` is `nil`, and you specify the group’s members by using the `:group` keyword when defining those members.

If you want to specify group members through `members`, each element should have the form `(name widget)`. Here `name` is a symbol, and `widget` is a widget type for editing that symbol. Useful widgets are `custom-variable` for a variable, `custom-face` for a face, and `custom-group` for a group.

When you introduce a new group into Emacs, use the `:version` keyword in the `defgroup`; then you need not use it for the individual members of the group.

In addition to the common keywords (see [Common Keywords](Common-Keywords.html)), you can also use this keyword in `defgroup`:

*   `:prefix prefix`

    If the name of an item in the group starts with `prefix`, and the customizable variable `custom-unlispify-remove-prefixes` is non-`nil`, the item’s tag will omit `prefix`. A group can have any number of prefixes.

The variables and subgroups of a group are stored in the `custom-group` property of the group’s symbol. See [Symbol Plists](Symbol-Plists.html). The value of that property is a list of pairs whose `car` is the variable or subgroup symbol and the `cdr` is either `custom-variable` or `custom-group`.

### User Option: **custom-unlispify-remove-prefixes**

If this variable is non-`nil`, the prefixes specified by a group’s `:prefix` keyword are omitted from tag names, whenever the user customizes the group.

The default value is `nil`, i.e., the prefix-discarding feature is disabled. This is because discarding prefixes often leads to confusing names for options and faces.
