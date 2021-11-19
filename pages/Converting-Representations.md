

Next: [Selecting a Representation](Selecting-a-Representation.html), Previous: [Disabling Multibyte](Disabling-Multibyte.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 33.3 Converting Text Representations

Emacs can convert unibyte text to multibyte; it can also convert multibyte text to unibyte, provided that the multibyte text contains only ASCII and 8-bit raw bytes. In general, these conversions happen when inserting text into a buffer, or when putting text from several strings together in one string. You can also explicitly convert a string’s contents to either representation.

Emacs chooses the representation for a string based on the text from which it is constructed. The general rule is to convert unibyte text to multibyte text when combining it with other multibyte text, because the multibyte representation is more general and can hold whatever characters the unibyte text has.

When inserting text into a buffer, Emacs converts the text to the buffer’s representation, as specified by `enable-multibyte-characters` in that buffer. In particular, when you insert multibyte text into a unibyte buffer, Emacs converts the text to unibyte, even though this conversion cannot in general preserve all the characters that might be in the multibyte text. The other natural alternative, to convert the buffer contents to multibyte, is not acceptable because the buffer’s representation is a choice made by the user that cannot be overridden automatically.

Converting unibyte text to multibyte text leaves ASCII characters unchanged, and converts bytes with codes 128 through 255 to the multibyte representation of raw eight-bit bytes.

Converting multibyte text to unibyte converts all ASCII and eight-bit characters to their single-byte form, but loses information for non-ASCII characters by discarding all but the low 8 bits of each character’s codepoint. Converting unibyte text to multibyte and back to unibyte reproduces the original unibyte text.

The next two functions either return the argument `string`, or a newly created string with no text properties.

*   Function: **string-to-multibyte** *string*

    This function returns a multibyte string containing the same sequence of characters as `string`. If `string` is a multibyte string, it is returned unchanged. The function assumes that `string` includes only ASCII characters and raw 8-bit bytes; the latter are converted to their multibyte representation corresponding to the codepoints `#x3FFF80` through `#x3FFFFF`, inclusive (see [codepoints](Text-Representations.html)).

<!---->

*   Function: **string-to-unibyte** *string*

    This function returns a unibyte string containing the same sequence of characters as `string`. It signals an error if `string` contains a non-ASCII character. If `string` is a unibyte string, it is returned unchanged. Use this function for `string` arguments that contain only ASCII and eight-bit characters.

<!---->

*   Function: **byte-to-string** *byte*

    This function returns a unibyte string containing a single byte of character data, `byte`. It signals an error if `byte` is not an integer between 0 and 255.

<!---->

*   Function: **multibyte-char-to-unibyte** *char*

    This converts the multibyte character `char` to a unibyte character, and returns that character. If `char` is neither ASCII nor eight-bit, the function returns -1.

<!---->

*   Function: **unibyte-char-to-multibyte** *char*

    This convert the unibyte character `char` to a multibyte character, assuming `char` is either ASCII or raw 8-bit byte.

Next: [Selecting a Representation](Selecting-a-Representation.html), Previous: [Disabling Multibyte](Disabling-Multibyte.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
