<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Finding Overlays](Finding-Overlays.html), Previous: [Managing Overlays](Managing-Overlays.html), Up: [Overlays](Overlays.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.9.2 Overlay Properties

Overlay properties are like text properties in that the properties that alter how a character is displayed can come from either source. But in most respects they are different. See [Text Properties](Text-Properties.html), for comparison.

Text properties are considered a part of the text; overlays and their properties are specifically considered not to be part of the text. Thus, copying text between various buffers and strings preserves text properties, but does not try to preserve overlays. Changing a buffer’s text properties marks the buffer as modified, while moving an overlay or changing its properties does not. Unlike text property changes, overlay property changes are not recorded in the buffer’s undo list.

Since more than one overlay can specify a property value for the same character, Emacs lets you specify a priority value of each overlay. The priority value is used to decide which of the overlapping overlays will “win”.

These functions read and set the properties of an overlay:

*   Function: **overlay-get** *overlay prop*

    This function returns the value of property `prop` recorded in `overlay`, if any. If `overlay` does not record any value for that property, but it does have a `category` property which is a symbol, that symbol’s `prop` property is used. Otherwise, the value is `nil`.

<!---->

*   Function: **overlay-put** *overlay prop value*

    This function sets the value of property `prop` recorded in `overlay` to `value`. It returns `value`.

<!---->

*   Function: **overlay-properties** *overlay*

    This returns a copy of the property list of `overlay`.

See also the function `get-char-property` which checks both overlay properties and text properties for a given character. See [Examining Properties](Examining-Properties.html).

Many overlay properties have special meanings; here is a table of them:

*   `priority`

    This property’s value determines the priority of the overlay. If you want to specify a priority value, use either `nil` (or zero), or a positive integer. Any other value has undefined behavior.

    The priority matters when two or more overlays cover the same character and both specify the same property; the one whose `priority` value is larger overrides the other. (For the `face` property, the higher priority overlay’s value does not completely override the other value; instead, its face attributes override the face attributes of the lower priority `face` property.) If two overlays have the same priority value, and one is nested in the other, then the inner one will prevail over the outer one. If neither is nested in the other then you should not make assumptions about which overlay will prevail.

    Currently, all overlays take priority over text properties.

    Note that Emacs sometimes uses non-numeric priority values for some of its internal overlays, so do not try to do arithmetic on the priority of an overlay (unless it is one that you created). In particular, the overlay used for showing the region uses a priority value of the form `(primary . secondary)`<!-- /@w -->, where the `primary` value is used as described above, and `secondary` is the fallback value used when `primary` and the nesting considerations fail to resolve the precedence between overlays. However, you are advised not to design Lisp programs based on this implementation detail; if you need to put overlays in priority order, use the `sorted` argument of `overlays-at`. See [Finding Overlays](Finding-Overlays.html).

*   `window`

    If the `window` property is non-`nil`, then the overlay applies only on that window.

*   `category`

    If an overlay has a `category` property, we call it the *category* of the overlay. It should be a symbol. The properties of the symbol serve as defaults for the properties of the overlay.

*   `face`

    This property controls the appearance of the text (see [Faces](Faces.html)). The value of the property can be the following:

    *   A face name (a symbol or string).
    *   An anonymous face: a property list of the form `(keyword value …)`, where each `keyword` is a face attribute name and `value` is a value for that attribute.
    *   A list of faces. Each list element should be either a face name or an anonymous face. This specifies a face which is an aggregate of the attributes of each of the listed faces. Faces occurring earlier in the list have higher priority.
    *   A cons cell of the form `(foreground-color . color-name)` or `(background-color . color-name)`. This specifies the foreground or background color, similar to `(:foreground color-name)` or `(:background color-name)`. This form is supported for backward compatibility only, and should be avoided.

*   `mouse-face`

    This property is used instead of `face` when the mouse is within the range of the overlay. However, Emacs ignores all face attributes from this property that alter the text size (e.g., `:height`, `:weight`, and `:slant`). Those attributes are always the same as in the unhighlighted text.

*   `display`

    This property activates various features that change the way text is displayed. For example, it can make text appear taller or shorter, higher or lower, wider or narrower, or replaced with an image. See [Display Property](Display-Property.html).

*   `help-echo`

    If an overlay has a `help-echo` property, then when you move the mouse onto the text in the overlay, Emacs displays a help string in the echo area, or in the tooltip window. For details see [Text help-echo](Special-Properties.html#Text-help_002decho).

*   `field`

    Consecutive characters with the same `field` property constitute a *field*. Some motion functions including `forward-word` and `beginning-of-line` stop moving at a field boundary. See [Fields](Fields.html).

*   `modification-hooks`

    This property’s value is a list of functions to be called if any character within the overlay is changed or if text is inserted strictly within the overlay.

    The hook functions are called both before and after each change. If the functions save the information they receive, and compare notes between calls, they can determine exactly what change has been made in the buffer text.

    When called before a change, each function receives four arguments: the overlay, `nil`, and the beginning and end of the text range to be modified.

    When called after a change, each function receives five arguments: the overlay, `t`, the beginning and end of the text range just modified, and the length of the pre-change text replaced by that range. (For an insertion, the pre-change length is zero; for a deletion, that length is the number of characters deleted, and the post-change beginning and end are equal.)

    When these functions are called, `inhibit-modification-hooks` is bound to non-`nil`. If the functions modify the buffer, you might want to bind `inhibit-modification-hooks` to `nil`, so as to cause the change hooks to run for these modifications. However, doing this may call your own change hook recursively, so be sure to prepare for that. See [Change Hooks](Change-Hooks.html).

    Text properties also support the `modification-hooks` property, but the details are somewhat different (see [Special Properties](Special-Properties.html)).

*   `insert-in-front-hooks`

    This property’s value is a list of functions to be called before and after inserting text right at the beginning of the overlay. The calling conventions are the same as for the `modification-hooks` functions.

*   `insert-behind-hooks`

    This property’s value is a list of functions to be called before and after inserting text right at the end of the overlay. The calling conventions are the same as for the `modification-hooks` functions.

*   `invisible`

    The `invisible` property can make the text in the overlay invisible, which means that it does not appear on the screen. See [Invisible Text](Invisible-Text.html), for details.

*   `intangible`

    The `intangible` property on an overlay works just like the `intangible` text property. It is obsolete. See [Special Properties](Special-Properties.html), for details.

*   `isearch-open-invisible`

    This property tells incremental search how to make an invisible overlay visible, permanently, if the final match overlaps it. See [Invisible Text](Invisible-Text.html).

*   `isearch-open-invisible-temporary`

    This property tells incremental search how to make an invisible overlay visible, temporarily, during the search. See [Invisible Text](Invisible-Text.html).

*   `before-string`

    This property’s value is a string to add to the display at the beginning of the overlay. The string does not appear in the buffer in any sense—only on the screen.

*   `after-string`

    This property’s value is a string to add to the display at the end of the overlay. The string does not appear in the buffer in any sense—only on the screen.

*   `line-prefix`

    This property specifies a display spec to prepend to each non-continuation line at display-time. See [Truncation](Truncation.html).

*   `wrap-prefix`

    This property specifies a display spec to prepend to each continuation line at display-time. See [Truncation](Truncation.html).

*   `evaporate`

    If this property is non-`nil`, the overlay is deleted automatically if it becomes empty (i.e., if its length becomes zero). If you give an empty overlay (see [empty overlay](Managing-Overlays.html)) a non-`nil` `evaporate` property, that deletes it immediately. Note that, unless an overlay has this property, it will not be deleted when the text between its starting and ending positions is deleted from the buffer.

*   `keymap`

    If this property is non-`nil`, it specifies a keymap for a portion of the text. This keymap is used when the character after point is within the overlay, and takes precedence over most other keymaps. See [Active Keymaps](Active-Keymaps.html).

*   `local-map`

    The `local-map` property is similar to `keymap` but replaces the buffer’s local map rather than augmenting existing keymaps. This also means it has lower precedence than minor mode keymaps.

The `keymap` and `local-map` properties do not affect a string displayed by the `before-string`, `after-string`, or `display` properties. This is only relevant for mouse clicks and other mouse events that fall on the string, since point is never on the string. To bind special mouse events for the string, assign it a `keymap` or `local-map` text property. See [Special Properties](Special-Properties.html).

Next: [Finding Overlays](Finding-Overlays.html), Previous: [Managing Overlays](Managing-Overlays.html), Up: [Overlays](Overlays.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
