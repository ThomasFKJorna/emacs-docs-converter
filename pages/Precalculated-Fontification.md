

Next: [Faces for Font Lock](Faces-for-Font-Lock.html), Previous: [Levels of Font Lock](Levels-of-Font-Lock.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.6.6 Precalculated Fontification

Some major modes such as `list-buffers` and `occur` construct the buffer text programmatically. The easiest way for them to support Font Lock mode is to specify the faces of text when they insert the text in the buffer.

The way to do this is to specify the faces in the text with the special text property `font-lock-face` (see [Special Properties](Special-Properties.html)). When Font Lock mode is enabled, this property controls the display, just like the `face` property. When Font Lock mode is disabled, `font-lock-face` has no effect on the display.

It is ok for a mode to use `font-lock-face` for some text and also use the normal Font Lock machinery. But if the mode does not use the normal Font Lock machinery, it should not set the variable `font-lock-defaults`. In this case the `face` property will not be overridden, so using the `face` property could work too. However, using `font-lock-face` is generally preferable as it allows the user to control the fontification by toggling `font-lock-mode`, and lets the code work regardless of whether the mode uses Font Lock machinery or not.
