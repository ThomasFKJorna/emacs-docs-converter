

Next: [Abstract Display](Abstract-Display.html), Previous: [Xwidgets](Xwidgets.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.19 Buttons

The Button package defines functions for inserting and manipulating *buttons* that can be activated with the mouse or via keyboard commands. These buttons are typically used for various kinds of hyperlinks.

A button is essentially a set of text or overlay properties, attached to a stretch of text in a buffer. These properties are called *button properties*. One of these properties, the *action property*, specifies a function which is called when the user invokes the button using the keyboard or the mouse. The action function may examine the button and use its other properties as desired.

In some ways, the Button package duplicates the functionality in the Widget package. See [Introduction](../widget/index.html#Top) in The Emacs Widget Library. The advantage of the Button package is that it is faster, smaller, and simpler to program. From the point of view of the user, the interfaces produced by the two packages are very similar.

|                                                         |    |                                                    |
| :------------------------------------------------------ | -- | :------------------------------------------------- |
| • [Button Properties](Button-Properties.html)           |    | Button properties with special meanings.           |
| • [Button Types](Button-Types.html)                     |    | Defining common properties for classes of buttons. |
| • [Making Buttons](Making-Buttons.html)                 |    | Adding buttons to Emacs buffers.                   |
| • [Manipulating Buttons](Manipulating-Buttons.html)     |    | Getting and setting properties of buttons.         |
| • [Button Buffer Commands](Button-Buffer-Commands.html) |    | Buffer-wide commands and bindings for buttons.     |
