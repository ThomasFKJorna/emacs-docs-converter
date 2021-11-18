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

Next: [Character Sets](Character-Sets.html), Previous: [Character Codes](Character-Codes.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 33.6 Character Properties

A *character property* is a named attribute of a character that specifies how the character behaves and how it should be handled during text processing and display. Thus, character properties are an important part of specifying the character’s semantics.

On the whole, Emacs follows the Unicode Standard in its implementation of character properties. In particular, Emacs supports the [Unicode Character Property Model](https://www.unicode.org/reports/tr23/), and the Emacs character property database is derived from the Unicode Character Database (UCD). See the [Character Properties chapter of the Unicode Standard](https://www.unicode.org/versions/Unicode12.1.0/ch04.pdf), for a detailed description of Unicode character properties and their meaning. This section assumes you are already familiar with that chapter of the Unicode Standard, and want to apply that knowledge to Emacs Lisp programs.

In Emacs, each property has a name, which is a symbol, and a set of possible values, whose types depend on the property; if a character does not have a certain property, the value is `nil`. As a general rule, the names of character properties in Emacs are produced from the corresponding Unicode properties by downcasing them and replacing each ‘`_`’ character with a dash ‘`-`’. For example, `Canonical_Combining_Class` becomes `canonical-combining-class`. However, sometimes we shorten the names to make their use easier.

Some codepoints are left *unassigned* by the UCD—they don’t correspond to any character. The Unicode Standard defines default values of properties for such codepoints; they are mentioned below for each property.

Here is the full list of value types for all the character properties that Emacs knows about:

*   `name`

    Corresponds to the `Name` Unicode property. The value is a string consisting of upper-case Latin letters A to Z, digits, spaces, and hyphen ‘`-`’ characters. For unassigned codepoints, the value is `nil`.

*   `general-category`

    Corresponds to the `General_Category` Unicode property. The value is a symbol whose name is a 2-letter abbreviation of the character’s classification. For unassigned codepoints, the value is `Cn`.

*   `canonical-combining-class`

    Corresponds to the `Canonical_Combining_Class` Unicode property. The value is an integer. For unassigned codepoints, the value is zero.

*   `bidi-class`

    Corresponds to the Unicode `Bidi_Class` property. The value is a symbol whose name is the Unicode *directional type* of the character. Emacs uses this property when it reorders bidirectional text for display (see [Bidirectional Display](Bidirectional-Display.html)). For unassigned codepoints, the value depends on the code blocks to which the codepoint belongs: most unassigned codepoints get the value of `L` (strong L), but some get values of `AL` (Arabic letter) or `R` (strong R).

*   `decomposition`

    Corresponds to the Unicode properties `Decomposition_Type` and `Decomposition_Value`. The value is a list, whose first element may be a symbol representing a compatibility formatting tag, such as `small`[18](#FOOT18); the other elements are characters that give the compatibility decomposition sequence of this character. For characters that don’t have decomposition sequences, and for unassigned codepoints, the value is a list with a single member, the character itself.

*   `decimal-digit-value`

    Corresponds to the Unicode `Numeric_Value` property for characters whose `Numeric_Type` is ‘`Decimal`’. The value is an integer, or `nil` if the character has no decimal digit value. For unassigned codepoints, the value is `nil`, which means NaN, or “not a number”.

*   `digit-value`

    Corresponds to the Unicode `Numeric_Value` property for characters whose `Numeric_Type` is ‘`Digit`’. The value is an integer. Examples of such characters include compatibility subscript and superscript digits, for which the value is the corresponding number. For characters that don’t have any numeric value, and for unassigned codepoints, the value is `nil`, which means NaN.

*   `numeric-value`

    Corresponds to the Unicode `Numeric_Value` property for characters whose `Numeric_Type` is ‘`Numeric`’. The value of this property is a number. Examples of characters that have this property include fractions, subscripts, superscripts, Roman numerals, currency numerators, and encircled numbers. For example, the value of this property for the character U+2155 VULGAR FRACTION ONE FIFTH is `0.2`. For characters that don’t have any numeric value, and for unassigned codepoints, the value is `nil`, which means NaN.

*   `mirrored`

    Corresponds to the Unicode `Bidi_Mirrored` property. The value of this property is a symbol, either `Y` or `N`. For unassigned codepoints, the value is `N`.

*   `mirroring`

    Corresponds to the Unicode `Bidi_Mirroring_Glyph` property. The value of this property is a character whose glyph represents the mirror image of the character’s glyph, or `nil` if there’s no defined mirroring glyph. All the characters whose `mirrored` property is `N` have `nil` as their `mirroring` property; however, some characters whose `mirrored` property is `Y` also have `nil` for `mirroring`, because no appropriate characters exist with mirrored glyphs. Emacs uses this property to display mirror images of characters when appropriate (see [Bidirectional Display](Bidirectional-Display.html)). For unassigned codepoints, the value is `nil`.

*   `paired-bracket`

    Corresponds to the Unicode `Bidi_Paired_Bracket` property. The value of this property is the codepoint of a character’s *paired bracket*, or `nil` if the character is not a bracket character. This establishes a mapping between characters that are treated as bracket pairs by the Unicode Bidirectional Algorithm; Emacs uses this property when it decides how to reorder for display parentheses, braces, and other similar characters (see [Bidirectional Display](Bidirectional-Display.html)).

*   `bracket-type`

    Corresponds to the Unicode `Bidi_Paired_Bracket_Type` property. For characters whose `paired-bracket` property is non-`nil`, the value of this property is a symbol, either `o` (for opening bracket characters) or `c` (for closing bracket characters). For characters whose `paired-bracket` property is `nil`, the value is the symbol `n` (None). Like `paired-bracket`, this property is used for bidirectional display.

*   `old-name`

    Corresponds to the Unicode `Unicode_1_Name` property. The value is a string. For unassigned codepoints, and characters that have no value for this property, the value is `nil`.

*   `iso-10646-comment`

    Corresponds to the Unicode `ISO_Comment` property. The value is either a string or `nil`. For unassigned codepoints, the value is `nil`.

*   `uppercase`

    Corresponds to the Unicode `Simple_Uppercase_Mapping` property. The value of this property is a single character. For unassigned codepoints, the value is `nil`, which means the character itself.

*   `lowercase`

    Corresponds to the Unicode `Simple_Lowercase_Mapping` property. The value of this property is a single character. For unassigned codepoints, the value is `nil`, which means the character itself.

*   `titlecase`

    Corresponds to the Unicode `Simple_Titlecase_Mapping` property. *Title case* is a special form of a character used when the first character of a word needs to be capitalized. The value of this property is a single character. For unassigned codepoints, the value is `nil`, which means the character itself.

*   `special-uppercase`

    Corresponds to Unicode language- and context-independent special upper-casing rules. The value of this property is a string (which may be empty). For example mapping for U+00DF LATIN SMALL LETTER SHARP S is `"SS"`. For characters with no special mapping, the value is `nil` which means `uppercase` property needs to be consulted instead.

*   `special-lowercase`

    Corresponds to Unicode language- and context-independent special lower-casing rules. The value of this property is a string (which may be empty). For example mapping for U+0130 LATIN CAPITAL LETTER I WITH DOT ABOVE the value is `"i\u0307"` (i.e. 2-character string consisting of LATIN SMALL LETTER I followed by U+0307 COMBINING DOT ABOVE). For characters with no special mapping, the value is `nil` which means `lowercase` property needs to be consulted instead.

*   `special-titlecase`

    Corresponds to Unicode unconditional special title-casing rules. The value of this property is a string (which may be empty). For example mapping for U+FB01 LATIN SMALL LIGATURE FI the value is `"Fi"`. For characters with no special mapping, the value is `nil` which means `titlecase` property needs to be consulted instead.

<!---->

*   Function: **get-char-code-property** *char propname*

    This function returns the value of `char`’s `propname` property.

        (get-char-code-property ?\s 'general-category)
             ⇒ Zs

    <!---->

        (get-char-code-property ?1 'general-category)
             ⇒ Nd

    <!---->

        ;; U+2084
        (get-char-code-property ?\N{SUBSCRIPT FOUR}
                                'digit-value)
             ⇒ 4

    <!---->

        ;; U+2155
        (get-char-code-property ?\N{VULGAR FRACTION ONE FIFTH}
                                'numeric-value)
             ⇒ 0.2

    <!---->

        ;; U+2163
        (get-char-code-property ?\N{ROMAN NUMERAL FOUR}
                                'numeric-value)
             ⇒ 4

    <!---->

        (get-char-code-property ?\( 'paired-bracket)
             ⇒ 41  ;; closing parenthesis

    <!---->

        (get-char-code-property ?\) 'bracket-type)
             ⇒ c

<!---->

*   Function: **char-code-property-description** *prop value*

    This function returns the description string of property `prop`’s `value`, or `nil` if `value` has no description.

        (char-code-property-description 'general-category 'Zs)
             ⇒ "Separator, Space"

    <!---->

        (char-code-property-description 'general-category 'Nd)
             ⇒ "Number, Decimal Digit"

    <!---->

        (char-code-property-description 'numeric-value '1/5)
             ⇒ nil

<!---->

*   Function: **put-char-code-property** *char propname value*

    This function stores `value` as the value of the property `propname` for the character `char`.

<!---->

*   Variable: **unicode-category-table**

    The value of this variable is a char-table (see [Char-Tables](Char_002dTables.html)) that specifies, for each character, its Unicode `General_Category` property as a symbol.

<!---->

*   Variable: **char-script-table**

    The value of this variable is a char-table that specifies, for each character, a symbol whose name is the script to which the character belongs, according to the Unicode Standard classification of the Unicode code space into script-specific blocks. This char-table has a single extra slot whose value is the list of all script symbols.

<!---->

*   Variable: **char-width-table**

    The value of this variable is a char-table that specifies the width of each character in columns that it will occupy on the screen.

<!---->

*   Variable: **printable-chars**

    The value of this variable is a char-table that specifies, for each character, whether it is printable or not. That is, if evaluating `(aref printable-chars char)` results in `t`, the character is printable, and if it results in `nil`, it is not.

***

#### Footnotes

##### [(18)](#DOCF18)

The Unicode specification writes these tag names inside ‘`<..>`’ brackets, but the tag names in Emacs do not include the brackets; e.g., Unicode specifies ‘`<small>`’ where Emacs uses ‘`small`’.

Next: [Character Sets](Character-Sets.html), Previous: [Character Codes](Character-Codes.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
