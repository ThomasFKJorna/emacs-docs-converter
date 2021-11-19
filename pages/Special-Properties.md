

Next: [Format Properties](Format-Properties.html), Previous: [Property Search](Property-Search.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.19.4 Properties with Special Meanings

Here is a table of text property names that have special built-in meanings. The following sections list a few additional special property names that control filling and property inheritance. All other names have no standard meaning, and you can use them as you like.

Note: the properties `composition`, `display`, `invisible` and `intangible` can also cause point to move to an acceptable place, after each Emacs command. See [Adjusting Point](Adjusting-Point.html).

*   `category`

    If a character has a `category` property, we call it the *property category* of the character. It should be a symbol. The properties of this symbol serve as defaults for the properties of the character.

*   `face`

    The `face` property controls the appearance of the character (see [Faces](Faces.html)). The value of the property can be the following:

    *   A face name (a symbol or string).
    *   An anonymous face: a property list of the form `(keyword value …)`, where each `keyword` is a face attribute name and `value` is a value for that attribute.
    *   A list of faces. Each list element should be either a face name or an anonymous face. This specifies a face which is an aggregate of the attributes of each of the listed faces. Faces occurring earlier in the list have higher priority.
    *   A cons cell of the form `(foreground-color . color-name)` or `(background-color . color-name)`. This specifies the foreground or background color, similar to `(:foreground color-name)` or `(:background color-name)`. This form is supported for backward compatibility only, and should be avoided.
    *   A cons cell of the form `(:filtered filter face-spec)`, that specifies the face given by `face-spec`, but only if `filter` matches when the face is used for display. The `face-spec` can use any of the forms mentioned above. The `filter` should be of the form `(:window param value)`, which matches for windows whose parameter `param` is `eq` to `value`. If the variable `face-filters-always-match` is non-`nil`, all face filters are deemed to have matched.

    Font Lock mode (see [Font Lock Mode](Font-Lock-Mode.html)) works in most buffers by dynamically updating the `face` property of characters based on the context.

    The `add-face-text-property` function provides a convenient way to set this text property. See [Changing Properties](Changing-Properties.html).

*   `font-lock-face`

    This property specifies a value for the `face` property that Font Lock mode should apply to the underlying text. It is one of the fontification methods used by Font Lock mode, and is useful for special modes that implement their own highlighting. See [Precalculated Fontification](Precalculated-Fontification.html). When Font Lock mode is disabled, `font-lock-face` has no effect.

*   `mouse-face`

    This property is used instead of `face` when the mouse is on or near the character. For this purpose, “near” means that all text between the character and where the mouse is have the same `mouse-face` property value.

    Emacs ignores all face attributes from the `mouse-face` property that alter the text size (e.g., `:height`, `:weight`, and `:slant`). Those attributes are always the same as for the unhighlighted text.

*   `fontified`

    This property says whether the text is ready for display. If `nil`, Emacs’s redisplay routine calls the functions in `fontification-functions` (see [Auto Faces](Auto-Faces.html)) to prepare this part of the buffer before it is displayed. It is used internally by the just-in-time font locking code.

*   `display`

    This property activates various features that change the way text is displayed. For example, it can make text appear taller or shorter, higher or lower, wider or narrow, or replaced with an image. See [Display Property](Display-Property.html).

*   `help-echo`

    If text has a string as its `help-echo` property, then when you move the mouse onto that text, Emacs displays that string in the echo area, or in the tooltip window (see [Tooltips](Tooltips.html)), after passing it through `substitute-command-keys`.

    If the value of the `help-echo` property is a function, that function is called with three arguments, `window`, `object` and `pos` and should return a help string or `nil` for none. The first argument, `window` is the window in which the help was found. The second, `object`, is the buffer, overlay or string which had the `help-echo` property. The `pos` argument is as follows:

    *   If `object` is a buffer, `pos` is the position in the buffer.
    *   If `object` is an overlay, that overlay has a `help-echo` property, and `pos` is the position in the overlay’s buffer.
    *   If `object` is a string (an overlay string or a string displayed with the `display` property), `pos` is the position in that string.

    If the value of the `help-echo` property is neither a function nor a string, it is evaluated to obtain a help string.

    You can alter the way help text is displayed by setting the variable `show-help-function` (see [Help display](#Help-display)).

    This feature is used in the mode line and for other active text.

*   `help-echo-inhibit-substitution`

    If the first character of a `help-echo` string has a non-`nil` `help-echo-inhibit-substitution` property, then it is displayed as-is by `show-help-function`, without being passed through `substitute-command-keys`.

*   `keymap`

    The `keymap` property specifies an additional keymap for commands. When this keymap applies, it is used for key lookup before the minor mode keymaps and before the buffer’s local map. See [Active Keymaps](Active-Keymaps.html). If the property value is a symbol, the symbol’s function definition is used as the keymap.

    The property’s value for the character before point applies if it is non-`nil` and rear-sticky, and the property’s value for the character after point applies if it is non-`nil` and front-sticky. (For mouse clicks, the position of the click is used instead of the position of point.)

*   `local-map`

    This property works like `keymap` except that it specifies a keymap to use *instead of* the buffer’s local map. For most purposes (perhaps all purposes), it is better to use the `keymap` property.

*   `syntax-table`

    The `syntax-table` property overrides what the syntax table says about this particular character. See [Syntax Properties](Syntax-Properties.html).

*   `read-only`

    If a character has the property `read-only`, then modifying that character is not allowed. Any command that would do so gets an error, `text-read-only`. If the property value is a string, that string is used as the error message.

    Insertion next to a read-only character is an error if inserting ordinary text there would inherit the `read-only` property due to stickiness. Thus, you can control permission to insert next to read-only text by controlling the stickiness. See [Sticky Properties](Sticky-Properties.html).

    Since changing properties counts as modifying the buffer, it is not possible to remove a `read-only` property unless you know the special trick: bind `inhibit-read-only` to a non-`nil` value and then remove the property. See [Read Only Buffers](Read-Only-Buffers.html).

*   `inhibit-read-only`

    Characters that have the property `inhibit-read-only` can be edited even in read-only buffers. See [Read Only Buffers](Read-Only-Buffers.html).

*   `invisible`

    A non-`nil` `invisible` property can make a character invisible on the screen. See [Invisible Text](Invisible-Text.html), for details.

*   `intangible`

    If a group of consecutive characters have equal and non-`nil` `intangible` properties, then you cannot place point between them. If you try to move point forward into the group, point actually moves to the end of the group. If you try to move point backward into the group, point actually moves to the start of the group.

    If consecutive characters have unequal non-`nil` `intangible` properties, they belong to separate groups; each group is separately treated as described above.

    When the variable `inhibit-point-motion-hooks` is non-`nil` (as it is by default), the `intangible` property is ignored.

    Beware: this property operates at a very low level, and affects a lot of code in unexpected ways. So use it with extreme caution. A common misuse is to put an intangible property on invisible text, which is actually unnecessary since the command loop will move point outside of the invisible text at the end of each command anyway. See [Adjusting Point](Adjusting-Point.html). For these reasons, this property is obsolete; use the `cursor-intangible` property instead.

*   `cursor-intangible`

    When the minor mode `cursor-intangible-mode` is turned on, point is moved away from any position that has a non-`nil` `cursor-intangible` property, just before redisplay happens.

    When the variable `cursor-sensor-inhibit` is non-`nil`, the `cursor-intangible` property and the `cursor-sensor-functions` property (described below) are ignored.

*   `field`

    Consecutive characters with the same `field` property constitute a *field*. Some motion functions including `forward-word` and `beginning-of-line` stop moving at a field boundary. See [Fields](Fields.html).

*   `cursor`

    Normally, the cursor is displayed at the beginning or the end of any overlay and text property strings present at the current buffer position. You can place the cursor on any desired character of these strings by giving that character a non-`nil` `cursor` text property. In addition, if the value of the `cursor` property is an integer, it specifies the number of buffer’s character positions, starting with the position where the overlay or the `display` property begins, for which the cursor should be displayed on that character. Specifically, if the value of the `cursor` property of a character is the number `n`, the cursor will be displayed on this character for any buffer position in the range `[ovpos..ovpos+n)`, where `ovpos` is the overlay’s starting position given by `overlay-start` (see [Managing Overlays](Managing-Overlays.html)), or the position where the `display` text property begins in the buffer.

    In other words, the string character with the `cursor` property of any non-`nil` value is the character where to display the cursor. The value of the property says for which buffer positions to display the cursor there. If the value is an integer `n`, the cursor is displayed there when point is anywhere between the beginning of the overlay or `display` property and `n` positions after that. If the value is anything else and non-`nil`, the cursor is displayed there only when point is at the beginning of the `display` property or at `overlay-start`.

    When the buffer has many overlay strings (e.g., see [before-string](Overlay-Properties.html)) that conceal some of the buffer text or `display` properties that are strings, it is a good idea to use the `cursor` property on these strings to cue the Emacs display about the places where to put the cursor while traversing these strings. This directly communicates to the display engine where the Lisp program wants to put the cursor, or where the user would expect the cursor, when point is located on some buffer position that is “covered” by the display or overlay string.

*   `pointer`

    This specifies a specific pointer shape when the mouse pointer is over this text or image. See [Pointer Shape](Pointer-Shape.html), for possible pointer shapes.

*   `line-spacing`

    A newline can have a `line-spacing` text or overlay property that controls the height of the display line ending with that newline. The property value overrides the default frame line spacing and the buffer local `line-spacing` variable. See [Line Height](Line-Height.html).

*   `line-height`

    A newline can have a `line-height` text or overlay property that controls the total height of the display line ending in that newline. See [Line Height](Line-Height.html).

*   `wrap-prefix`

    If text has a `wrap-prefix` property, the prefix it defines will be added at display time to the beginning of every continuation line due to text wrapping (so if lines are truncated, the wrap-prefix is never used). It may be a string or an image (see [Other Display Specs](Other-Display-Specs.html)), or a stretch of whitespace such as specified by the `:width` or `:align-to` display properties (see [Specified Space](Specified-Space.html)).

    A wrap-prefix may also be specified for an entire buffer using the `wrap-prefix` buffer-local variable (however, a `wrap-prefix` text-property takes precedence over the value of the `wrap-prefix` variable). See [Truncation](Truncation.html).

*   `line-prefix`

    If text has a `line-prefix` property, the prefix it defines will be added at display time to the beginning of every non-continuation line. It may be a string or an image (see [Other Display Specs](Other-Display-Specs.html)), or a stretch of whitespace such as specified by the `:width` or `:align-to` display properties (see [Specified Space](Specified-Space.html)).

    A line-prefix may also be specified for an entire buffer using the `line-prefix` buffer-local variable (however, a `line-prefix` text-property takes precedence over the value of the `line-prefix` variable). See [Truncation](Truncation.html).

*   `modification-hooks`

    If a character has the property `modification-hooks`, then its value should be a list of functions; modifying that character calls all of those functions before the actual modification. Each function receives two arguments: the beginning and end of the part of the buffer being modified. Note that if a particular modification hook function appears on several characters being modified by a single primitive, you can’t predict how many times the function will be called. Furthermore, insertion will not modify any existing character, so this hook will only be run when removing some characters, replacing them with others, or changing their text-properties.

    Unlike with other similar hooks, when Emacs calls these functions, `inhibit-modification-hooks` does *not* get bound to non-`nil`. If the functions modify the buffer, you should consider binding this variable to non-`nil` to prevent any buffer changes running the change hooks. Otherwise, you must be prepared for recursive calls. See [Change Hooks](Change-Hooks.html).

    Overlays also support the `modification-hooks` property, but the details are somewhat different (see [Overlay Properties](Overlay-Properties.html)).

*   *   `insert-in-front-hooks`
    *   `insert-behind-hooks`

    The operation of inserting text in a buffer also calls the functions listed in the `insert-in-front-hooks` property of the following character and in the `insert-behind-hooks` property of the preceding character. These functions receive two arguments, the beginning and end of the inserted text. The functions are called *after* the actual insertion takes place.

    When these functions are called, `inhibit-modification-hooks` is bound to non-`nil`. If the functions modify the buffer, you might want to bind `inhibit-modification-hooks` to `nil`, so as to cause the change hooks to run for these modifications. However, doing this may call your own change hook recursively, so be sure to prepare for that.

    See also [Change Hooks](Change-Hooks.html), for other hooks that are called when you change text in a buffer.

*   *   `point-entered`
    *   `point-left`

    The special properties `point-entered` and `point-left` record hook functions that report motion of point. Each time point moves, Emacs compares these two property values:

    *   the `point-left` property of the character after the old location, and
    *   the `point-entered` property of the character after the new location.

    If these two values differ, each of them is called (if not `nil`) with two arguments: the old value of point, and the new one.

    The same comparison is made for the characters before the old and new locations. The result may be to execute two `point-left` functions (which may be the same function) and/or two `point-entered` functions (which may be the same function). In any case, all the `point-left` functions are called first, followed by all the `point-entered` functions.

    It is possible to use `char-after` to examine characters at various buffer positions without moving point to those positions. Only an actual change in the value of point runs these hook functions.

    The variable `inhibit-point-motion-hooks` by default inhibits running the `point-left` and `point-entered` hooks, see [Inhibit point motion hooks](#Inhibit-point-motion-hooks).

    These properties are obsolete; please use `cursor-sensor-functions` instead.

*   `cursor-sensor-functions`

    This special property records a list of functions that react to cursor motion. Each function in the list is called, just before redisplay, with 3 arguments: the affected window, the previous known position of the cursor, and one of the symbols `entered` or `left`, depending on whether the cursor is entering the text that has this property or leaving it. The functions are called only when the minor mode `cursor-sensor-mode` is turned on.

    When the variable `cursor-sensor-inhibit` is non-`nil`, the `cursor-sensor-functions` property is ignored.

*   `composition`

    This text property is used to display a sequence of characters as a single glyph composed from components. But the value of the property itself is completely internal to Emacs and should not be manipulated directly by, for instance, `put-text-property`.

*   `minibuffer-message`

    This text property tells where to display temporary messages in an active minibuffer. Specifically, the first character of the minibuffer text which has this property will have the temporary message displayed before it. The default is to display temporary messages at the end of the minibuffer text. This text property is used by the function that is the default value of `set-message-function` (see [Displaying Messages](Displaying-Messages.html)).

<!---->

*   Variable: **inhibit-point-motion-hooks**

    When this obsolete variable is non-`nil`, `point-left` and `point-entered` hooks are not run, and the `intangible` property has no effect. Do not set this variable globally; bind it with `let`. Since the affected properties are obsolete, this variable’s default value is `t`, to effectively disable them.

<!---->

*   Variable: **show-help-function**

    If this variable is non-`nil`, it specifies a function called to display help strings. These may be `help-echo` properties, menu help strings (see [Simple Menu Items](Simple-Menu-Items.html), see [Extended Menu Items](Extended-Menu-Items.html)), or tool bar help strings (see [Tool Bar](Tool-Bar.html)). The specified function is called with one argument, the help string to display, which is passed through `substitute-command-keys` before being given to the function, unless the help string has a non-`nil` `help-echo-inhibit-substitution` property on its first character; see [Keys in Documentation](Keys-in-Documentation.html). See the code of Tooltip mode (see [Tooltips](https://www.gnu.org/software/emacs/manual/html_node/emacs/Tooltips.html#Tooltips) in The GNU Emacs Manual) for an example of a mode that uses `show-help-function`.

<!---->

*   Variable: **face-filters-always-match**

    If this variable is non-`nil`, face filters that specify attributes applied only when certain conditions are met will be deemed to match always.

Next: [Format Properties](Format-Properties.html), Previous: [Property Search](Property-Search.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
