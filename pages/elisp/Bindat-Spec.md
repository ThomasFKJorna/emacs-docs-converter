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

Next: [Bindat Functions](Bindat-Functions.html), Up: [Byte Packing](Byte-Packing.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 38.20.1 Describing Data Layout

To control unpacking and packing, you write a *data layout specification*, a special nested list describing named and typed *fields*. This specification controls the length of each field to be processed, and how to pack or unpack it. We normally keep bindat specs in variables whose names end in ‘`-bindat-spec`’; that kind of name is automatically recognized as risky.

A field’s *type* describes the size (in bytes) of the object that the field represents and, in the case of multibyte fields, how the bytes are ordered within the field. The two possible orderings are *big endian* (also known as “network byte ordering”) and *little endian*. For instance, the number `#x23cd` (decimal 9165) in big endian would be the two bytes `#x23` `#xcd`; and in little endian, `#xcd` `#x23`. Here are the possible type values:

*   *   `u8`
    *   `byte`

    Unsigned byte, with length 1.

*   *   `u16`
    *   `word`
    *   `short`

    Unsigned integer in network byte order, with length 2.

*   `u24`

    Unsigned integer in network byte order, with length 3.

*   *   `u32`
    *   `dword`
    *   `long`

    Unsigned integer in network byte order, with length 4. Note: These values may be limited by Emacs’s integer implementation limits.

*   *   `u16r`
    *   `u24r`
    *   `u32r`

    Unsigned integer in little endian order, with length 2, 3 and 4, respectively.

*   `str len`

    String of length `len`.

*   `strz len`

    Zero-terminated string, in a fixed-size field with length `len`.

*   `vec len [type]`

    Vector of `len` elements of type `type`, defaulting to bytes. The `type` is any of the simple types above, or another vector specified as a list of the form `(vec len [type])`.

*   `ip`

    Four-byte vector representing an Internet address. For example: `[127 0 0 1]` for localhost.

*   `bits len`

    List of set bits in `len` bytes. The bytes are taken in big endian order and the bits are numbered starting with `8 * len - 1` and ending with zero. For example: `bits 2` unpacks `#x28` `#x1c` to `(2 3 4 11 13)` and `#x1c` `#x28` to `(3 5 10 11 12)`.

*   `(eval form)`

    `form` is a Lisp expression evaluated at the moment the field is unpacked or packed. The result of the evaluation should be one of the above-listed type specifications.

For a fixed-size field, the length `len` is given as an integer specifying the number of bytes in the field.

When the length of a field is not fixed, it typically depends on the value of a preceding field. In this case, the length `len` can be given either as a list `(name ...)` identifying a *field name* in the format specified for `bindat-get-field` below, or by an expression `(eval form)` where `form` should evaluate to an integer, specifying the field length.

A field specification generally has the form `([name] handler)`, where `name` is optional. Don’t use names that are symbols meaningful as type specifications (above) or handler specifications (below), since that would be ambiguous. `name` can be a symbol or an expression `(eval form)`, in which case `form` should evaluate to a symbol.

`handler` describes how to unpack or pack the field and can be one of the following:

*   `type`

    Unpack/pack this field according to the type specification `type`.

*   `eval form`

    Evaluate `form`, a Lisp expression, for side-effect only. If the field name is specified, the value is bound to that field name.

*   `fill len`

    Skip `len` bytes. In packing, this leaves them unchanged, which normally means they remain zero. In unpacking, this means they are ignored.

*   `align len`

    Skip to the next multiple of `len` bytes.

*   `struct spec-name`

    Process `spec-name` as a sub-specification. This describes a structure nested within another structure.

*   `union form (tag spec)…`

    Evaluate `form`, a Lisp expression, find the first `tag` that matches it, and process its associated data layout specification `spec`. Matching can occur in one of three ways:

    *   If a `tag` has the form `(eval expr)`, evaluate `expr` with the variable `tag` dynamically bound to the value of `form`. A non-`nil` result indicates a match.
    *   `tag` matches if it is `equal` to the value of `form`.
    *   `tag` matches unconditionally if it is `t`.

*   `repeat count field-specs…`

    Process the `field-specs` recursively, in order, then repeat starting from the first one, processing all the specifications `count` times overall. The `count` is given using the same formats as a field length—if an `eval` form is used, it is evaluated just once. For correct operation, each specification in `field-specs` must include a name.

For the `(eval form)` forms used in a bindat specification, the `form` can access and update these dynamically bound variables during evaluation:

*   `last`

    Value of the last field processed.

*   `bindat-raw`

    The data as a byte array.

*   `bindat-idx`

    Current index (within `bindat-raw`) for unpacking or packing.

*   `struct`

    The alist containing the structured data that have been unpacked so far, or the entire structure being packed. You can use `bindat-get-field` to access specific fields of this structure.

*   *   `count`
    *   `index`

    Inside a `repeat` block, these contain the maximum number of repetitions (as specified by the `count` parameter), and the current repetition number (counting from 0). Setting `count` to zero will terminate the inner-most repeat block after the current repetition has completed.

Next: [Bindat Functions](Bindat-Functions.html), Up: [Byte Packing](Byte-Packing.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
