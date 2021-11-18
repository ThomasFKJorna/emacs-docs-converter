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

Next: [Special Properties](Special-Properties.html), Previous: [Changing Properties](Changing-Properties.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.19.3 Text Property Search Functions

In typical use of text properties, most of the time several or many consecutive characters have the same value for a property. Rather than writing your programs to examine characters one by one, it is much faster to process chunks of text that have the same property value.

Here are functions you can use to do this. They use `eq` for comparing property values. In all cases, `object` defaults to the current buffer.

For good performance, it’s very important to use the `limit` argument to these functions, especially the ones that search for a single property—otherwise, they may spend a long time scanning to the end of the buffer, if the property you are interested in does not change.

These functions do not move point; instead, they return a position (or `nil`). Remember that a position is always between two characters; the position returned by these functions is between two characters with different properties.

*   Function: **next-property-change** *pos \&optional object limit*

    The function scans the text forward from position `pos` in the string or buffer `object` until it finds a change in some text property, then returns the position of the change. In other words, it returns the position of the first character beyond `pos` whose properties are not identical to those of the character just after `pos`.

    If `limit` is non-`nil`, then the scan ends at position `limit`. If there is no property change before that point, this function returns `limit`.

    The value is `nil` if the properties remain unchanged all the way to the end of `object` and `limit` is `nil`. If the value is non-`nil`, it is a position greater than or equal to `pos`. The value equals `pos` only when `limit` equals `pos`.

    Here is an example of how to scan the buffer by chunks of text within which all properties are constant:

        (while (not (eobp))
          (let ((plist (text-properties-at (point)))
                (next-change
                 (or (next-property-change (point) (current-buffer))
                     (point-max))))
            Process text from point to next-change…
            (goto-char next-change)))

<!---->

*   Function: **previous-property-change** *pos \&optional object limit*

    This is like `next-property-change`, but scans back from `pos` instead of forward. If the value is non-`nil`, it is a position less than or equal to `pos`; it equals `pos` only if `limit` equals `pos`.

<!---->

*   Function: **next-single-property-change** *pos prop \&optional object limit*

    The function scans text for a change in the `prop` property, then returns the position of the change. The scan goes forward from position `pos` in the string or buffer `object`. In other words, this function returns the position of the first character beyond `pos` whose `prop` property differs from that of the character just after `pos`.

    If `limit` is non-`nil`, then the scan ends at position `limit`. If there is no property change before that point, `next-single-property-change` returns `limit`.

    The value is `nil` if the property remains unchanged all the way to the end of `object` and `limit` is `nil`. If the value is non-`nil`, it is a position greater than or equal to `pos`; it equals `pos` only if `limit` equals `pos`.

<!---->

*   Function: **previous-single-property-change** *pos prop \&optional object limit*

    This is like `next-single-property-change`, but scans back from `pos` instead of forward. If the value is non-`nil`, it is a position less than or equal to `pos`; it equals `pos` only if `limit` equals `pos`.

<!---->

*   Function: **next-char-property-change** *pos \&optional limit*

    This is like `next-property-change` except that it considers overlay properties as well as text properties, and if no change is found before the end of the buffer, it returns the maximum buffer position rather than `nil` (in this sense, it resembles the corresponding overlay function `next-overlay-change`, rather than `next-property-change`). There is no `object` operand because this function operates only on the current buffer. It returns the next address at which either kind of property changes.

<!---->

*   Function: **previous-char-property-change** *pos \&optional limit*

    This is like `next-char-property-change`, but scans back from `pos` instead of forward, and returns the minimum buffer position if no change is found.

<!---->

*   Function: **next-single-char-property-change** *pos prop \&optional object limit*

    This is like `next-single-property-change` except that it considers overlay properties as well as text properties, and if no change is found before the end of the `object`, it returns the maximum valid position in `object` rather than `nil`. Unlike `next-char-property-change`, this function *does* have an `object` operand; if `object` is not a buffer, only text-properties are considered.

<!---->

*   Function: **previous-single-char-property-change** *pos prop \&optional object limit*

    This is like `next-single-char-property-change`, but scans back from `pos` instead of forward, and returns the minimum valid position in `object` if no change is found.

<!---->

*   Function: **text-property-any** *start end prop value \&optional object*

    This function returns non-`nil` if at least one character between `start` and `end` has a property `prop` whose value is `value`. More precisely, it returns the position of the first such character. Otherwise, it returns `nil`.

    The optional fifth argument, `object`, specifies the string or buffer to scan. Positions are relative to `object`. The default for `object` is the current buffer.

<!---->

*   Function: **text-property-not-all** *start end prop value \&optional object*

    This function returns non-`nil` if at least one character between `start` and `end` does not have a property `prop` with value `value`. More precisely, it returns the position of the first such character. Otherwise, it returns `nil`.

    The optional fifth argument, `object`, specifies the string or buffer to scan. Positions are relative to `object`. The default for `object` is the current buffer.

<!---->

*   Function: **text-property-search-forward** *prop \&optional value predicate not-current*

    Search for the next region that has text property `prop` set to `value` according to `predicate`.

    This function is modelled after `search-forward` and friends in that it moves point, but it returns a structure that describes the match instead of returning it in `match-beginning` and friends.

    If the text property can’t be found, the function returns `nil`. If it’s found, point is placed at the end of the region that has this text property match, and a `prop-match` structure is returned.

    `predicate` can either be `t` (which is a synonym for `equal`), `nil` (which means “not equal”), or a predicate that will be called with two parameters: The first is `value`, and the second is the value of the text property we’re inspecting.

    If `not-current`, if point is in a region where we have a match, then skip past that and find the next instance instead.

    The `prop-match` structure has the following accessors: `prop-match-beginning` (the start of the match), `prop-match-end` (the end of the match), and `prop-match-value` (the value of `property` at the start of the match).

    In the examples below, imagine that you’re in a buffer that looks like this:

        This is a bold and here's bolditalic and this is the end.

    That is, the “bold” words are the `bold` face, and the “italic” word is in the `italic` face.

    With point at the start:

        (while (setq match (text-property-search-forward 'face 'bold t))
          (push (buffer-substring (prop-match-beginning match)
                                  (prop-match-end match))
                words))

    This will pick out all the words that use the `bold` face.

        (while (setq match (text-property-search-forward 'face nil t))
          (push (buffer-substring (prop-match-beginning match)
                                  (prop-match-end match))
                words))

    This will pick out all the bits that have no face properties, which will result in the list ‘`("This is a " "and here's " "and this is the end")`’ (only reversed, since we used `push`).

        (while (setq match (text-property-search-forward 'face nil nil))
          (push (buffer-substring (prop-match-beginning match)
                                  (prop-match-end match))
                words))

    This will pick out all the regions where `face` is set to something, but this is split up into where the properties change, so the result here will be ‘`("bold" "bold" "italic")`’.

    For a more realistic example where you might use this, consider that you have a buffer where certain sections represent URLs, and these are tagged with `shr-url`.

        (while (setq match (text-property-search-forward 'shr-url nil nil))
          (push (prop-match-value match) urls))

    This will give you a list of all those URLs.

<!---->

*   Function: **text-property-search-backward** *prop \&optional value predicate not-current*

    This is just like `text-property-search-backward`, but searches backward instead. Point is placed at the beginning of the matched region instead of the end, though.

Next: [Special Properties](Special-Properties.html), Previous: [Changing Properties](Changing-Properties.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
