

Next: [Defining Faces](Defining-Faces.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.12.1 Face Attributes

*Face attributes* determine the visual appearance of a face. The following table lists all the face attributes, their possible values, and their effects.

Apart from the values given below, each face attribute can have the value `unspecified`. This special value means that the face doesn’t specify that attribute directly. An `unspecified` attribute tells Emacs to refer instead to a parent face (see the description `:inherit` attribute below); or, failing that, to an underlying face (see [Displaying Faces](Displaying-Faces.html)). The `default` face must specify all attributes.

Some of these attributes are meaningful only on certain kinds of displays. If your display cannot handle a certain attribute, the attribute is ignored.

*   `:family`

    Font family name (a string). See [Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Fonts.html#Fonts) in The GNU Emacs Manual, for more information about font families. The function `font-family-list` (see below) returns a list of available family names.

*   `:foundry`

    The name of the *font foundry* for the font family specified by the `:family` attribute (a string). See [Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Fonts.html#Fonts) in The GNU Emacs Manual.

*   `:width`

    Relative character width. This should be one of the symbols `ultra-condensed`, `extra-condensed`, `condensed`, `semi-condensed`, `normal`, `semi-expanded`, `expanded`, `extra-expanded`, or `ultra-expanded`.

*   `:height`

    The height of the font. In the simplest case, this is an integer in units of 1/10 point.

    The value can also be floating point or a function, which specifies the height relative to an *underlying face* (see [Displaying Faces](Displaying-Faces.html)). A floating-point value specifies the amount by which to scale the height of the underlying face. A function value is called with one argument, the height of the underlying face, and returns the height of the new face. If the function is passed an integer argument, it must return an integer.

    The height of the default face must be specified using an integer; floating point and function values are not allowed.

*   `:weight`

    Font weight—one of the symbols (from densest to faintest) `ultra-bold`, `extra-bold`, `bold`, `semi-bold`, `normal`, `semi-light`, `light`, `extra-light`, or `ultra-light`. On text terminals which support variable-brightness text, any weight greater than normal is displayed as extra bright, and any weight less than normal is displayed as half-bright.

*   `:slant`

    Font slant—one of the symbols `italic`, `oblique`, `normal`, `reverse-italic`, or `reverse-oblique`. On text terminals that support variable-brightness text, slanted text is displayed as half-bright.

*   `:foreground`

    Foreground color, a string. The value can be a system-defined color name, or a hexadecimal color specification. See [Color Names](Color-Names.html). On black-and-white displays, certain shades of gray are implemented by stipple patterns.

*   `:distant-foreground`

    Alternative foreground color, a string. This is like `:foreground` but the color is only used as a foreground when the background color is near to the foreground that would have been used. This is useful for example when marking text (i.e., the region face). If the text has a foreground that is visible with the region face, that foreground is used. If the foreground is near the region face background, `:distant-foreground` is used instead so the text is readable.

*   `:background`

    Background color, a string. The value can be a system-defined color name, or a hexadecimal color specification. See [Color Names](Color-Names.html).

*   `:underline`

    Whether or not characters should be underlined, and in what way. The possible values of the `:underline` attribute are:

    *   `nil`

        Don’t underline.

    *   `t`

        Underline with the foreground color of the face.

    *   `color`

        Underline in color `color`, a string specifying a color.

    *   `(:color color :style style)`

        `color` is either a string, or the symbol `foreground-color`, meaning the foreground color of the face. Omitting the attribute `:color` means to use the foreground color of the face. `style` should be a symbol `line` or `wave`, meaning to use a straight or wavy line. Omitting the attribute `:style` means to use a straight line.

*   `:overline`

    Whether or not characters should be overlined, and in what color. If the value is `t`, overlining uses the foreground color of the face. If the value is a string, overlining uses that color. The value `nil` means do not overline.

*   `:strike-through`

    Whether or not characters should be strike-through, and in what color. The value is used like that of `:overline`.

*   `:box`

    Whether or not a box should be drawn around characters, its color, the width of the box lines, and 3D appearance. Here are the possible values of the `:box` attribute, and what they mean:

    *   `nil`

        Don’t draw a box.

    *   `t`

        Draw a box with lines of width 1, in the foreground color.

    *   `color`

        Draw a box with lines of width 1, in color `color`.

    *   `(:line-width width :color color :style style)`

        This way you can explicitly specify all aspects of the box. The value `width` specifies the width of the lines to draw; it defaults to 1. A negative width -`n` means to draw a line of width `n` whose top and bottom parts occupy the space of the underlying text, thus avoiding any increase in the character height.

        The value `color` specifies the color to draw with. The default is the foreground color of the face for simple boxes, and the background color of the face for 3D boxes.

        The value `style` specifies whether to draw a 3D box. If it is `released-button`, the box looks like a 3D button that is not being pressed. If it is `pressed-button`, the box looks like a 3D button that is being pressed. If it is `nil` or omitted, a plain 2D box is used.

*   `:inverse-video`

    Whether or not characters should be displayed in inverse video. The value should be `t` (yes) or `nil` (no).

*   `:stipple`

    The background stipple, a bitmap.

    The value can be a string; that should be the name of a file containing external-format X bitmap data. The file is found in the directories listed in the variable `x-bitmap-file-path`.

    Alternatively, the value can specify the bitmap directly, with a list of the form `(width height data)`. Here, `width` and `height` specify the size in pixels, and `data` is a string containing the raw bits of the bitmap, row by row. Each row occupies *(`width` + 7) / 8* consecutive bytes in the string (which should be a unibyte string for best results). This means that each row always occupies at least one whole byte.

    If the value is `nil`, that means use no stipple pattern.

    Normally you do not need to set the stipple attribute, because it is used automatically to handle certain shades of gray.

*   `:font`

    The font used to display the face. Its value should be a font object or a fontset. See [Low-Level Font](Low_002dLevel-Font.html), for information about font objects, font specs, and font entities. See [Fontsets](Fontsets.html), for information about fontsets.

    When specifying this attribute using `set-face-attribute` or `set-face-font` (see [Attribute Functions](Attribute-Functions.html)), you may also supply a font spec, a font entity, or a string. Emacs converts such values to an appropriate font object, and stores that font object as the actual attribute value. If you specify a string, the contents of the string should be a font name (see [Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Fonts.html#Fonts) in The GNU Emacs Manual); if the font name is an XLFD containing wildcards, Emacs chooses the first font matching those wildcards. Specifying this attribute also changes the values of the `:family`, `:foundry`, `:width`, `:height`, `:weight`, and `:slant` attributes.

*   `:inherit`

    The name of a face from which to inherit attributes, or a list of face names. Attributes from inherited faces are merged into the face like an underlying face would be, with higher priority than underlying faces (see [Displaying Faces](Displaying-Faces.html)). If the face to inherit from is `unspecified`, it is treated the same as `nil`, since Emacs never merges `:inherit` attributes. If a list of faces is used, attributes from faces earlier in the list override those from later faces.

*   `:extend`

    Whether or not this face will be extended beyond end of line and will affect the display of the empty space between the end of line and the edge of the window. The value should be `t` to display the empty space between end of line and edge of the window using this face, or `nil` to not use this face for the space between the end of the line and the edge of the window. When Emacs merges several faces for displaying the empty space beyond end of line, only those faces with `:extend` non-`nil` will be merged. By default, only a small number of faces, notably, `region`, have this attribute set. This attribute is different from the others in that when a theme doesn’t specify an explicit value for a face, the value from the original face definition by `defface` is inherited (see [Defining Faces](Defining-Faces.html)).

<!---->

*   Function: **font-family-list** *\&optional frame*

    This function returns a list of available font family names. The optional argument `frame` specifies the frame on which the text is to be displayed; if it is `nil`, the selected frame is used.

<!---->

*   User Option: **underline-minimum-offset**

    This variable specifies the minimum distance between the baseline and the underline, in pixels, when displaying underlined text.

<!---->

*   User Option: **x-bitmap-file-path**

    This variable specifies a list of directories for searching for bitmap files, for the `:stipple` attribute.

<!---->

*   Function: **bitmap-spec-p** *object*

    This returns `t` if `object` is a valid bitmap specification, suitable for use with `:stipple` (see above). It returns `nil` otherwise.

Next: [Defining Faces](Defining-Faces.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
