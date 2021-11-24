

#### 39.12.12 Low-Level Font Representation

Normally, it is not necessary to manipulate fonts directly. In case you need to do so, this section explains how.

In Emacs Lisp, fonts are represented using three different Lisp object types: *font objects*, *font specs*, and *font entities*.

### Function: **fontp** *object \&optional type*

Return `t` if `object` is a font object, font spec, or font entity. Otherwise, return `nil`.

The optional argument `type`, if non-`nil`, determines the exact type of Lisp object to check for. In that case, `type` should be one of `font-object`, `font-spec`, or `font-entity`.

A font object is a Lisp object that represents a font that Emacs has *opened*. Font objects cannot be modified in Lisp, but they can be inspected.

### Function: **font-at** *position \&optional window string*

Return the font object that is being used to display the character at position `position` in the window `window`. If `window` is `nil`, it defaults to the selected window. If `string` is `nil`, `position` specifies a position in the current buffer; otherwise, `string` should be a string, and `position` specifies a position in that string.

A font spec is a Lisp object that contains a set of specifications that can be used to find a font. More than one font may match the specifications in a font spec.

### Function: **font-spec** *\&rest arguments*

Return a new font spec using the specifications in `arguments`, which should come in `property`-`value` pairs. The possible specifications are as follows:

*   `:name`

    The font name (a string), in either XLFD, Fontconfig, or GTK+ format. See [Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Fonts.html#Fonts) in The GNU Emacs Manual.

*   *   `:family`
    *   `:foundry`
    *   `:weight`
    *   `:slant`
    *   `:width`

    These have the same meanings as the face attributes of the same name. See [Face Attributes](Face-Attributes.html). `:family` and `:foundry` are strings, while the other three are symbols. As example values, `:slant` may be `italic`, `:weight` may be `bold` and `:width` may be `normal`.

*   `:size`

    The font size—either a non-negative integer that specifies the pixel size, or a floating-point number that specifies the point size.

*   `:adstyle`

    Additional typographic style information for the font, such as ‘`sans`’. The value should be a string or a symbol.

*   `:registry`

    The charset registry and encoding of the font, such as ‘`iso8859-1`’. The value should be a string or a symbol.

*   `:dpi`

    The resolution in dots per inch for which the font is designed. The value must be a non-negative number.

*   `:spacing`

    The spacing of the font: proportional, dual, mono, or charcell. The value should be either an integer (0 for proportional, 90 for dual, 100 for mono, 110 for charcell) or a one-letter symbol (one of `P`, `D`, `M`, or `C`).

*   `:avgwidth`

    The average width of the font in 1/10 pixel units. The value should be a non-negative number.

*   `:script`

    The script that the font must support (a symbol).

*   `:lang`

    The language that the font should support. The value should be a symbol whose name is a two-letter ISO-639 language name. On X, the value is matched against the “Additional Style” field of the XLFD name of a font, if it is non-empty. On MS-Windows, fonts matching the spec are required to support codepages needed for the language. Currently, only a small set of CJK languages is supported with this property: ‘`ja`’, ‘`ko`’, and ‘`zh`’.

*   `:otf`

    The font must be an OpenType font that supports these OpenType features, provided Emacs is compiled with a library, such as ‘`libotf`’ on GNU/Linux, that supports complex text layout for scripts which need that. The value must be a list of the form

    ```lisp
    (script-tag langsys-tag gsub gpos)
    ```

    where `script-tag` is the OpenType script tag symbol; `langsys-tag` is the OpenType language system tag symbol, or `nil` to use the default language system; `gsub` is a list of OpenType GSUB feature tag symbols, or `nil` if none is required; and `gpos` is a list of OpenType GPOS feature tag symbols, or `nil` if none is required. If `gsub` or `gpos` is a list, a `nil` element in that list means that the font must not match any of the remaining tag symbols. The `gpos` element may be omitted.

### Function: **font-put** *font-spec property value*

Set the font property `property` in the font-spec `font-spec` to `value`.

A font entity is a reference to a font that need not be open. Its properties are intermediate between a font object and a font spec: like a font object, and unlike a font spec, it refers to a single, specific font. Unlike a font object, creating a font entity does not load the contents of that font into computer memory. Emacs may open multiple font objects of different sizes from a single font entity referring to a scalable font.

### Function: **find-font** *font-spec \&optional frame*

This function returns a font entity that best matches the font spec `font-spec` on frame `frame`. If `frame` is `nil`, it defaults to the selected frame.

### Function: **list-fonts** *font-spec \&optional frame num prefer*

This function returns a list of all font entities that match the font spec `font-spec`.

The optional argument `frame`, if non-`nil`, specifies the frame on which the fonts are to be displayed. The optional argument `num`, if non-`nil`, should be an integer that specifies the maximum length of the returned list. The optional argument `prefer`, if non-`nil`, should be another font spec, which is used to control the order of the returned list; the returned font entities are sorted in order of decreasing closeness to that font spec.

If you call `set-face-attribute` and pass a font spec, font entity, or font name string as the value of the `:font` attribute, Emacs opens the best matching font that is available for display. It then stores the corresponding font object as the actual value of the `:font` attribute for that face.

The following functions can be used to obtain information about a font. For these functions, the `font` argument can be a font object, a font entity, or a font spec.

### Function: **font-get** *font property*

This function returns the value of the font property `property` for `font`.

If `font` is a font spec and the font spec does not specify `property`, the return value is `nil`. If `font` is a font object or font entity, the value for the `:script` property may be a list of scripts supported by the font.

### Function: **font-face-attributes** *font \&optional frame*

This function returns a list of face attributes corresponding to `font`. The optional argument `frame` specifies the frame on which the font is to be displayed. If it is `nil`, the selected frame is used. The return value has the form

```lisp
(:family family :height height :weight weight
   :slant slant :width width)
```

where the values of `family`, `height`, `weight`, `slant`, and `width` are face attribute values. Some of these key-attribute pairs may be omitted from the list if they are not specified by `font`.

### Function: **font-xlfd-name** *font \&optional fold-wildcards*

This function returns the XLFD (X Logical Font Descriptor), a string, matching `font`. See [Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Fonts.html#Fonts) in The GNU Emacs Manual, for information about XLFDs. If the name is too long for an XLFD (which can contain at most 255 characters), the function returns `nil`.

If the optional argument `fold-wildcards` is non-`nil`, consecutive wildcards in the XLFD are folded into one.

The following two functions return important information about a font.

### Function: **font-info** *name \&optional frame*

This function returns information about a font specified by its `name`, a string, as it is used on `frame`. If `frame` is omitted or `nil`, it defaults to the selected frame.

The value returned by the function is a vector of the form `[opened-name full-name size height baseline-offset relative-compose default-ascent max-width ascent descent space-width average-width filename capability]`. Here’s the description of each components of this vector:

*   `opened-name`

    The name used to open the font, a string.

*   `full-name`

    The full name of the font, a string.

*   `size`

    The pixel size of the font.

*   `height`

    The height of the font in pixels.

*   `baseline-offset`

    The offset in pixels from the ASCII baseline, positive upward.

*   *   `relative-compose`
    *   `default-ascent`

    Numbers controlling how to compose characters.

*   `max-width`

    The maximum advance width of the font.

*   *   `ascent`
    *   `descent`

    The ascent and descent of this font. The sum of these two numbers should be equal to the value of `height` above.

*   `space-width`

    The width, in pixels, of the font’s space character.

*   `average-width`

    The average width of the font characters. If this is zero, Emacs uses the value of `space-width` instead, when it calculates text layout on display.

*   `filename`

    The file name of the font as a string. This can be `nil` if the font back-end does not provide a way to find out the font’s file name.

*   `capability`

    A list whose first element is a symbol representing the font type, one of `x`, `opentype`, `truetype`, `type1`, `pcf`, or `bdf`. For OpenType fonts, the list includes 2 additional elements describing the GSUB and GPOS features supported by the font. Each of these elements is a list of the form `((script (langsys feature …) …) …)`, where `script` is a symbol representing an OpenType script tag, `langsys` is a symbol representing an OpenType langsys tag (or `nil`, which stands for the default langsys), and each `feature` is a symbol representing an OpenType feature tag.

### Function: **query-font** *font-object*

This function returns information about a `font-object`. (This is in contrast to `font-info`, which takes the font name, a string, as its argument.)

The value returned by the function is a vector of the form `[name filename pixel-size max-width ascent descent space-width average-width capability]`. Here’s the description of each components of this vector:

*   `name`

    The font name, a string.

*   `filename`

    The file name of the font as a string. This can be `nil` if the font back-end does not provide a way to find out the font’s file name.

*   `pixel-size`

    The pixel size of the font used to open the font.

*   `max-width`

    The maximum advance width of the font.

*   *   `ascent`
    *   `descent`

    The ascent and descent of this font. The sum of these two numbers gives the font height.

*   `space-width`

    The width, in pixels, of the font’s space character.

*   `average-width`

    The average width of the font characters. If this is zero, Emacs uses the value of `space-width` instead, when it calculates text layout on display.

*   `capability`

    A list whose first element is a symbol representing the font type, one of `x`, `opentype`, `truetype`, `type1`, `pcf`, or `bdf`. For OpenType fonts, the list includes 2 additional elements describing the GSUB and GPOS features supported by the font. Each of these elements is a list of the form `((script (langsys feature …) …) …)`, where `script` is a symbol representing an OpenType script tag, `langsys` is a symbol representing an OpenType langsys tag (or `nil`, which stands for the default langsys), and each `feature` is a symbol representing an OpenType feature tag.

The following four functions return size information about fonts used by various faces, allowing various layout considerations in Lisp programs. These functions take face remapping into consideration, returning information about the remapped face, if the face in question was remapped. See [Face Remapping](Face-Remapping.html).

### Function: **default-font-width**

This function returns the average width in pixels of the font used by the current buffer’s default face, as that face is defined for the selected frame.

### Function: **default-font-height**

This function returns the height in pixels of the font used by the current buffer’s default face, as that face is defined for the selected frame.

### Function: **window-font-width** *\&optional window face*

This function returns the average width in pixels for the font used by `face` in `window`. The specified `window` must be a live window. If `nil` or omitted, `window` defaults to the selected window, and `face` defaults to the default face in `window`.

### Function: **window-font-height** *\&optional window face*

This function returns the height in pixels for the font used by `face` in `window`. The specified `window` must be a live window. If `nil` or omitted, `window` defaults to the selected window, and `face` defaults to the default face in `window`.
