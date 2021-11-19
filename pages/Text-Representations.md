

Next: [Disabling Multibyte](Disabling-Multibyte.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 33.1 Text Representations

Emacs buffers and strings support a large repertoire of characters from many different scripts, allowing users to type and display text in almost any known written language.

To support this multitude of characters and scripts, Emacs closely follows the *Unicode Standard*. The Unicode Standard assigns a unique number, called a *codepoint*, to each and every character. The range of codepoints defined by Unicode, or the Unicode *codespace*, is `0..#x10FFFF` (in hexadecimal notation), inclusive. Emacs extends this range with codepoints in the range `#x110000..#x3FFFFF`, which it uses for representing characters that are not unified with Unicode and *raw 8-bit bytes* that cannot be interpreted as characters. Thus, a character codepoint in Emacs is a 22-bit integer.

To conserve memory, Emacs does not hold fixed-length 22-bit numbers that are codepoints of text characters within buffers and strings. Rather, Emacs uses a variable-length internal representation of characters, that stores each character as a sequence of 1 to 5 8-bit bytes, depending on the magnitude of its codepoint[17](#FOOT17). For example, any ASCII character takes up only 1 byte, a Latin-1 character takes up 2 bytes, etc. We call this representation of text *multibyte*.

Outside Emacs, characters can be represented in many different encodings, such as ISO-8859-1, GB-2312, Big-5, etc. Emacs converts between these external encodings and its internal representation, as appropriate, when it reads text into a buffer or a string, or when it writes text to a disk file or passes it to some other process.

Occasionally, Emacs needs to hold and manipulate encoded text or binary non-text data in its buffers or strings. For example, when Emacs visits a file, it first reads the file’s text verbatim into a buffer, and only then converts it to the internal representation. Before the conversion, the buffer holds encoded text.

Encoded text is not really text, as far as Emacs is concerned, but rather a sequence of raw 8-bit bytes. We call buffers and strings that hold encoded text *unibyte* buffers and strings, because Emacs treats them as a sequence of individual bytes. Usually, Emacs displays unibyte buffers and strings as octal codes such as `\237`. We recommend that you never use unibyte buffers and strings except for manipulating encoded text or binary non-text data.

In a buffer, the buffer-local value of the variable `enable-multibyte-characters` specifies the representation used. The representation for a string is determined and recorded in the string when the string is constructed.

*   Variable: **enable-multibyte-characters**

    This variable specifies the current buffer’s text representation. If it is non-`nil`, the buffer contains multibyte text; otherwise, it contains unibyte encoded text or binary non-text data.

    You cannot set this variable directly; instead, use the function `set-buffer-multibyte` to change a buffer’s representation.

<!---->

*   Function: **position-bytes** *position*

    Buffer positions are measured in character units. This function returns the byte-position corresponding to buffer position `position` in the current buffer. This is 1 at the start of the buffer, and counts upward in bytes. If `position` is out of range, the value is `nil`.

<!---->

*   Function: **byte-to-position** *byte-position*

    Return the buffer position, in character units, corresponding to given `byte-position` in the current buffer. If `byte-position` is out of range, the value is `nil`. In a multibyte buffer, an arbitrary value of `byte-position` can be not at character boundary, but inside a multibyte sequence representing a single character; in this case, this function returns the buffer position of the character whose multibyte sequence includes `byte-position`. In other words, the value does not change for all byte positions that belong to the same character.

The following two functions are useful when a Lisp program needs to map buffer positions to byte offsets in a file visited by the buffer.

*   Function: **bufferpos-to-filepos** *position \&optional quality coding-system*

    This function is similar to `position-bytes`, but instead of byte position in the current buffer it returns the offset from the beginning of the current buffer’s file of the byte that corresponds to the given character `position` in the buffer. The conversion requires to know how the text is encoded in the buffer’s file; this is what the `coding-system` argument is for, defaulting to the value of `buffer-file-coding-system`. The optional argument `quality` specifies how accurate the result should be; it should be one of the following:

    *   `exact`

        The result must be accurate. The function may need to encode and decode a large part of the buffer, which is expensive and can be slow.

    *   `approximate`

        The value can be an approximation. The function may avoid expensive processing and return an inexact result.

    *   `nil`

        If the exact result needs expensive processing, the function will return `nil` rather than an approximation. This is the default if the argument is omitted.

<!---->

*   Function: **filepos-to-bufferpos** *byte \&optional quality coding-system*

    This function returns the buffer position corresponding to a file position specified by `byte`, a zero-base byte offset from the file’s beginning. The function performs the conversion opposite to what `bufferpos-to-filepos` does. Optional arguments `quality` and `coding-system` have the same meaning and values as for `bufferpos-to-filepos`.

<!---->

*   Function: **multibyte-string-p** *string*

    Return `t` if `string` is a multibyte string, `nil` otherwise. This function also returns `nil` if `string` is some object other than a string.

<!---->

*   Function: **string-bytes** *string*

    This function returns the number of bytes in `string`. If `string` is a multibyte string, this can be greater than `(length string)`.

<!---->

*   Function: **unibyte-string** *\&rest bytes*

    This function concatenates all its argument `bytes` and makes the result a unibyte string.

***

#### Footnotes

##### [(17)](#DOCF17)

This internal representation is based on one of the encodings defined by the Unicode Standard, called *UTF-8*, for representing any Unicode codepoint, but Emacs extends UTF-8 to represent the additional codepoints it uses for raw 8-bit bytes and characters not unified with Unicode.

Next: [Disabling Multibyte](Disabling-Multibyte.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
