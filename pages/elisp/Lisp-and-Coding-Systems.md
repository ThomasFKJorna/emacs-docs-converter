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

Next: [User-Chosen Coding Systems](User_002dChosen-Coding-Systems.html), Previous: [Encoding and I/O](Encoding-and-I_002fO.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 33.10.3 Coding Systems in Lisp

Here are the Lisp facilities for working with coding systems:

*   Function: **coding-system-list** *\&optional base-only*

    This function returns a list of all coding system names (symbols). If `base-only` is non-`nil`, the value includes only the base coding systems. Otherwise, it includes alias and variant coding systems as well.

<!---->

*   Function: **coding-system-p** *object*

    This function returns `t` if `object` is a coding system name or `nil`.

<!---->

*   Function: **check-coding-system** *coding-system*

    This function checks the validity of `coding-system`. If that is valid, it returns `coding-system`. If `coding-system` is `nil`, the function return `nil`. For any other values, it signals an error whose `error-symbol` is `coding-system-error` (see [signal](Signaling-Errors.html)).

<!---->

*   Function: **coding-system-eol-type** *coding-system*

    This function returns the type of end-of-line (a.k.a. *eol*) conversion used by `coding-system`. If `coding-system` specifies a certain eol conversion, the return value is an integer 0, 1, or 2, standing for `unix`, `dos`, and `mac`, respectively. If `coding-system` doesn’t specify eol conversion explicitly, the return value is a vector of coding systems, each one with one of the possible eol conversion types, like this:

        (coding-system-eol-type 'latin-1)
             ⇒ [latin-1-unix latin-1-dos latin-1-mac]

    If this function returns a vector, Emacs will decide, as part of the text encoding or decoding process, what eol conversion to use. For decoding, the end-of-line format of the text is auto-detected, and the eol conversion is set to match it (e.g., DOS-style CRLF format will imply `dos` eol conversion). For encoding, the eol conversion is taken from the appropriate default coding system (e.g., default value of `buffer-file-coding-system` for `buffer-file-coding-system`), or from the default eol conversion appropriate for the underlying platform.

<!---->

*   Function: **coding-system-change-eol-conversion** *coding-system eol-type*

    This function returns a coding system which is like `coding-system` except for its eol conversion, which is specified by `eol-type`. `eol-type` should be `unix`, `dos`, `mac`, or `nil`. If it is `nil`, the returned coding system determines the end-of-line conversion from the data.

    `eol-type` may also be 0, 1 or 2, standing for `unix`, `dos` and `mac`, respectively.

<!---->

*   Function: **coding-system-change-text-conversion** *eol-coding text-coding*

    This function returns a coding system which uses the end-of-line conversion of `eol-coding`, and the text conversion of `text-coding`. If `text-coding` is `nil`, it returns `undecided`, or one of its variants according to `eol-coding`.

<!---->

*   Function: **find-coding-systems-region** *from to*

    This function returns a list of coding systems that could be used to encode a text between `from` and `to`. All coding systems in the list can safely encode any multibyte characters in that portion of the text.

    If the text contains no multibyte characters, the function returns the list `(undecided)`.

<!---->

*   Function: **find-coding-systems-string** *string*

    This function returns a list of coding systems that could be used to encode the text of `string`. All coding systems in the list can safely encode any multibyte characters in `string`. If the text contains no multibyte characters, this returns the list `(undecided)`.

<!---->

*   Function: **find-coding-systems-for-charsets** *charsets*

    This function returns a list of coding systems that could be used to encode all the character sets in the list `charsets`.

<!---->

*   Function: **check-coding-systems-region** *start end coding-system-list*

    This function checks whether coding systems in the list `coding-system-list` can encode all the characters in the region between `start` and `end`. If all of the coding systems in the list can encode the specified text, the function returns `nil`. If some coding systems cannot encode some of the characters, the value is an alist, each element of which has the form `(coding-system1 pos1 pos2 …)`, meaning that `coding-system1` cannot encode characters at buffer positions `pos1`, `pos2`, ....

    `start` may be a string, in which case `end` is ignored and the returned value references string indices instead of buffer positions.

<!---->

*   Function: **detect-coding-region** *start end \&optional highest*

    This function chooses a plausible coding system for decoding the text from `start` to `end`. This text should be a byte sequence, i.e., unibyte text or multibyte text with only ASCII and eight-bit characters (see [Explicit Encoding](Explicit-Encoding.html)).

    Normally this function returns a list of coding systems that could handle decoding the text that was scanned. They are listed in order of decreasing priority. But if `highest` is non-`nil`, then the return value is just one coding system, the one that is highest in priority.

    If the region contains only ASCII characters except for such ISO-2022 control characters ISO-2022 as `ESC`, the value is `undecided` or `(undecided)`, or a variant specifying end-of-line conversion, if that can be deduced from the text.

    If the region contains null bytes, the value is `no-conversion`, even if the region contains text encoded in some coding system.

<!---->

*   Function: **detect-coding-string** *string \&optional highest*

    This function is like `detect-coding-region` except that it operates on the contents of `string` instead of bytes in the buffer.

<!---->

*   Variable: **inhibit-nul-byte-detection**

    If this variable has a non-`nil` value, null bytes are ignored when detecting the encoding of a region or a string. This allows the encoding of text that contains null bytes to be correctly detected, such as Info files with Index nodes.

<!---->

*   Variable: **inhibit-iso-escape-detection**

    If this variable has a non-`nil` value, ISO-2022 escape sequences are ignored when detecting the encoding of a region or a string. The result is that no text is ever detected as encoded in some ISO-2022 encoding, and all escape sequences become visible in a buffer. **Warning:** *Use this variable with extreme caution, because many files in the Emacs distribution use ISO-2022 encoding.*

<!---->

*   Function: **coding-system-charset-list** *coding-system*

    This function returns the list of character sets (see [Character Sets](Character-Sets.html)) supported by `coding-system`. Some coding systems that support too many character sets to list them all yield special values:

    *   If `coding-system` supports all Emacs characters, the value is `(emacs)`.
    *   If `coding-system` supports all Unicode characters, the value is `(unicode)`.
    *   If `coding-system` supports all ISO-2022 charsets, the value is `iso-2022`.
    *   If `coding-system` supports all the characters in the internal coding system used by Emacs version 21 (prior to the implementation of internal Unicode support), the value is `emacs-mule`.

See [Process Information](Process-Information.html#Coding-systems-for-a-subprocess), in particular the description of the functions `process-coding-system` and `set-process-coding-system`, for how to examine or set the coding systems used for I/O to a subprocess.

Next: [User-Chosen Coding Systems](User_002dChosen-Coding-Systems.html), Previous: [Encoding and I/O](Encoding-and-I_002fO.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
