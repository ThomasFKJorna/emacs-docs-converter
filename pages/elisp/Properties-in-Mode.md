

#### 23.4.6 Properties in the Mode Line

Certain text properties are meaningful in the mode line. The `face` property affects the appearance of text; the `help-echo` property associates help strings with the text, and `keymap` can make the text mouse-sensitive.

There are four ways to specify text properties for text in the mode line:

1.  Put a string with a text property directly into the mode line data structure.
2.  Put a text property on a mode line %-construct such as ‘`%12b`’; then the expansion of the %-construct will have that same text property.
3.  Use a `(:propertize elt props…)` construct to give `elt` a text property specified by `props`.
4.  Use a list containing `:eval form` in the mode line data structure, and make `form` evaluate to a string that has a text property.

You can use the `keymap` property to specify a keymap. This keymap only takes real effect for mouse clicks; binding character keys and function keys to it has no effect, since it is impossible to move point into the mode line.

When the mode line refers to a variable which does not have a non-`nil` `risky-local-variable` property, any text properties given or specified within that variable’s values are ignored. This is because such properties could otherwise specify functions to be called, and those functions could come from file local variables.
