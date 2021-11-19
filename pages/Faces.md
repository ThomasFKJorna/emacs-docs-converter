

Next: [Fringes](Fringes.html), Previous: [Line Height](Line-Height.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.12 Faces

A *face* is a collection of graphical attributes for displaying text: font, foreground color, background color, optional underlining, etc. Faces control how Emacs displays text in buffers, as well as other parts of the frame such as the mode line.

One way to represent a face is as a property list of attributes, like `(:foreground "red" :weight bold)`. Such a list is called an *anonymous face*. For example, you can assign an anonymous face as the value of the `face` text property, and Emacs will display the underlying text with the specified attributes. See [Special Properties](Special-Properties.html).

More commonly, a face is referred to via a *face name*: a Lisp symbol associated with a set of face attributes[22](#FOOT22). Named faces are defined using the `defface` macro (see [Defining Faces](Defining-Faces.html)). Emacs comes with several standard named faces (see [Basic Faces](Basic-Faces.html)).

Some parts of Emacs require named faces (e.g., the functions documented in [Attribute Functions](Attribute-Functions.html)). Unless otherwise stated, we will use the term *face* to refer only to named faces.

*   Function: **facep** *object*

    This function returns a non-`nil` value if `object` is a named face: a Lisp symbol or string which serves as a face name. Otherwise, it returns `nil`.

|                                                   |    |                                                                           |
| :------------------------------------------------ | -- | :------------------------------------------------------------------------ |
| • [Face Attributes](Face-Attributes.html)         |    | What is in a face?                                                        |
| • [Defining Faces](Defining-Faces.html)           |    | How to define a face.                                                     |
| • [Attribute Functions](Attribute-Functions.html) |    | Functions to examine and set face attributes.                             |
| • [Displaying Faces](Displaying-Faces.html)       |    | How Emacs combines the faces specified for a character.                   |
| • [Face Remapping](Face-Remapping.html)           |    | Remapping faces to alternative definitions.                               |
| • [Face Functions](Face-Functions.html)           |    | How to define and examine faces.                                          |
| • [Auto Faces](Auto-Faces.html)                   |    | Hook for automatic face assignment.                                       |
| • [Basic Faces](Basic-Faces.html)                 |    | Faces that are defined by default.                                        |
| • [Font Selection](Font-Selection.html)           |    | Finding the best available font for a face.                               |
| • [Font Lookup](Font-Lookup.html)                 |    | Looking up the names of available fonts and information about them.       |
| • [Fontsets](Fontsets.html)                       |    | A fontset is a collection of fonts that handle a range of character sets. |
| • [Low-Level Font](Low_002dLevel-Font.html)       |    | Lisp representation for character display fonts.                          |

***

#### Footnotes

##### [(22)](#DOCF22)

For backward compatibility, you can also use a string to specify a face name; that is equivalent to a Lisp symbol with the same name.

Next: [Fringes](Fringes.html), Previous: [Line Height](Line-Height.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
