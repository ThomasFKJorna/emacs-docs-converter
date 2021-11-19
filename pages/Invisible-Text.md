

Next: [Selective Display](Selective-Display.html), Previous: [Warnings](Warnings.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.6 Invisible Text

You can make characters *invisible*, so that they do not appear on the screen, with the `invisible` property. This can be either a text property (see [Text Properties](Text-Properties.html)) or an overlay property (see [Overlays](Overlays.html)). Cursor motion also partly ignores these characters; if the command loop finds that point is inside a range of invisible text after a command, it relocates point to the other side of the text.

In the simplest case, any non-`nil` `invisible` property makes a character invisible. This is the default case—if you don’t alter the default value of `buffer-invisibility-spec`, this is how the `invisible` property works. You should normally use `t` as the value of the `invisible` property if you don’t plan to set `buffer-invisibility-spec` yourself.

More generally, you can use the variable `buffer-invisibility-spec` to control which values of the `invisible` property make text invisible. This permits you to classify the text into different subsets in advance, by giving them different `invisible` values, and subsequently make various subsets visible or invisible by changing the value of `buffer-invisibility-spec`.

Controlling visibility with `buffer-invisibility-spec` is especially useful in a program to display the list of entries in a database. It permits the implementation of convenient filtering commands to view just a part of the entries in the database. Setting this variable is very fast, much faster than scanning all the text in the buffer looking for properties to change.

*   Variable: **buffer-invisibility-spec**

    This variable specifies which kinds of `invisible` properties actually make a character invisible. Setting this variable makes it buffer-local.

    *   `t`

        A character is invisible if its `invisible` property is non-`nil`. This is the default.

    *   a list

        Each element of the list specifies a criterion for invisibility; if a character’s `invisible` property fits any one of these criteria, the character is invisible. The list can have two kinds of elements:

        *   `atom`

            A character is invisible if its `invisible` property value is `atom` or if it is a list with `atom` as a member; comparison is done with `eq`.

        *   `(atom . t)`

            A character is invisible if its `invisible` property value is `atom` or if it is a list with `atom` as a member; comparison is done with `eq`. Moreover, a sequence of such characters displays as an ellipsis.

Two functions are specifically provided for adding elements to `buffer-invisibility-spec` and removing elements from it.

*   Function: **add-to-invisibility-spec** *element*

    This function adds the element `element` to `buffer-invisibility-spec`. If `buffer-invisibility-spec` was `t`, it changes to a list, `(t)`, so that text whose `invisible` property is `t` remains invisible.

<!---->

*   Function: **remove-from-invisibility-spec** *element*

    This removes the element `element` from `buffer-invisibility-spec`. This does nothing if `element` is not in the list.

A convention for use of `buffer-invisibility-spec` is that a major mode should use the mode’s own name as an element of `buffer-invisibility-spec` and as the value of the `invisible` property:

```lisp
;; If you want to display an ellipsis:
(add-to-invisibility-spec '(my-symbol . t))
;; If you don’t want ellipsis:
(add-to-invisibility-spec 'my-symbol)

(overlay-put (make-overlay beginning end)
             'invisible 'my-symbol)

;; When done with the invisibility:
(remove-from-invisibility-spec '(my-symbol . t))
;; Or respectively:
(remove-from-invisibility-spec 'my-symbol)
```

You can check for invisibility using the following function:

*   Function: **invisible-p** *pos-or-prop*

    If `pos-or-prop` is a marker or number, this function returns a non-`nil` value if the text at that position is currently invisible.

    If `pos-or-prop` is any other kind of Lisp object, that is taken to mean a possible value of the `invisible` text or overlay property. In that case, this function returns a non-`nil` value if that value would cause text to become invisible, based on the current value of `buffer-invisibility-spec`.

    The return value of this function is `t` if the text would be completely hidden on display, or a non-`nil`, non-`t` value if the text would be replaced by an ellipsis.

Ordinarily, functions that operate on text or move point do not care whether the text is invisible, they process invisible characters and visible characters alike. The user-level line motion commands, such as `next-line`, `previous-line`, ignore invisible newlines if `line-move-ignore-invisible` is non-`nil` (the default), i.e., behave like these invisible newlines didn’t exist in the buffer, but only because they are explicitly programmed to do so.

If a command ends with point inside or at the boundary of invisible text, the main editing loop relocates point to one of the two ends of the invisible text. Emacs chooses the direction of relocation so that it is the same as the overall movement direction of the command; if in doubt, it prefers a position where an inserted char would not inherit the `invisible` property. Additionally, if the text is not replaced by an ellipsis and the command only moved within the invisible text, then point is moved one extra character so as to try and reflect the command’s movement by a visible movement of the cursor.

Thus, if the command moved point back to an invisible range (with the usual stickiness), Emacs moves point back to the beginning of that range. If the command moved point forward into an invisible range, Emacs moves point forward to the first visible character that follows the invisible text and then forward one more character.

These *adjustments* of point that ended up in the middle of invisible text can be disabled by setting `disable-point-adjustment` to a non-`nil` value. See [Adjusting Point](Adjusting-Point.html).

Incremental search can make invisible overlays visible temporarily and/or permanently when a match includes invisible text. To enable this, the overlay should have a non-`nil` `isearch-open-invisible` property. The property value should be a function to be called with the overlay as an argument. This function should make the overlay visible permanently; it is used when the match overlaps the overlay on exit from the search.

During the search, such overlays are made temporarily visible by temporarily modifying their invisible and intangible properties. If you want this to be done differently for a certain overlay, give it an `isearch-open-invisible-temporary` property which is a function. The function is called with two arguments: the first is the overlay, and the second is `nil` to make the overlay visible, or `t` to make it invisible again.

Next: [Selective Display](Selective-Display.html), Previous: [Warnings](Warnings.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
