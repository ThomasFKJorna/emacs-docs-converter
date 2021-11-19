

Next: [Making Buttons](Making-Buttons.html), Previous: [Button Properties](Button-Properties.html), Up: [Buttons](Buttons.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.19.2 Button Types

Every button has a *button type*, which defines default values for the button’s properties. Button types are arranged in a hierarchy, with specialized types inheriting from more general types, so that it’s easy to define special-purpose types of buttons for specific tasks.

*   Function: **define-button-type** *name \&rest properties*

    Define a button type called `name` (a symbol). The remaining arguments form a sequence of `property value` pairs, specifying default property values for buttons with this type (a button’s type may be set by giving it a `type` property when creating the button, using the `:type` keyword argument).

    In addition, the keyword argument `:supertype` may be used to specify a button-type from which `name` inherits its default property values. Note that this inheritance happens only when `name` is defined; subsequent changes to a supertype are not reflected in its subtypes.

Using `define-button-type` to define default properties for buttons is not necessary—buttons without any specified type use the built-in button-type `button`—but it is encouraged, since doing so usually makes the resulting code clearer and more efficient.
