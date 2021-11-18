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

Next: [GnuTLS Cryptography](GnuTLS-Cryptography.html), Previous: [Base 64](Base-64.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.26 Checksum/Hash

Emacs has built-in support for computing *cryptographic hashes*. A cryptographic hash, or *checksum*, is a digital fingerprint of a piece of data (e.g., a block of text) which can be used to check that you have an unaltered copy of that data.

Emacs supports several common cryptographic hash algorithms: MD5, SHA-1, SHA-2, SHA-224, SHA-256, SHA-384 and SHA-512. MD5 is the oldest of these algorithms, and is commonly used in *message digests* to check the integrity of messages transmitted over a network. MD5 and SHA-1 are not collision resistant (i.e., it is possible to deliberately design different pieces of data which have the same MD5 or SHA-1 hash), so you should not use them for anything security-related. For security-related applications you should use the other hash types, such as SHA-2 (e.g. `sha256` or `sha512`).

*   Function: **secure-hash-algorithms**

    This function returns a list of symbols representing algorithms that `secure-hash` can use.

<!---->

*   Function: **secure-hash** *algorithm object \&optional start end binary*

    This function returns a hash for `object`. The argument `algorithm` is a symbol stating which hash to compute: one of `md5`, `sha1`, `sha224`, `sha256`, `sha384` or `sha512`. The argument `object` should be a buffer or a string.

    The optional arguments `start` and `end` are character positions specifying the portion of `object` to compute the message digest for. If they are `nil` or omitted, the hash is computed for the whole of `object`.

    If the argument `binary` is omitted or `nil`, the function returns the *text form* of the hash, as an ordinary Lisp string. If `binary` is non-`nil`, it returns the hash in *binary form*, as a sequence of bytes stored in a unibyte string.

    This function does not compute the hash directly from the internal representation of `object`’s text (see [Text Representations](Text-Representations.html)). Instead, it encodes the text using a coding system (see [Coding Systems](Coding-Systems.html)), and computes the hash from that encoded text. If `object` is a buffer, the coding system used is the one which would be chosen by default for writing the text into a file. If `object` is a string, the user’s preferred coding system is used (see [Recognize Coding](https://www.gnu.org/software/emacs/manual/html_node/emacs/Recognize-Coding.html#Recognize-Coding) in GNU Emacs Manual).

<!---->

*   Function: **md5** *object \&optional start end coding-system noerror*

    This function returns an MD5 hash. It is semi-obsolete, since for most purposes it is equivalent to calling `secure-hash` with `md5` as the `algorithm` argument. The `object`, `start` and `end` arguments have the same meanings as in `secure-hash`.

    If `coding-system` is non-`nil`, it specifies a coding system to use to encode the text; if omitted or `nil`, the default coding system is used, like in `secure-hash`.

    Normally, `md5` signals an error if the text can’t be encoded using the specified or chosen coding system. However, if `noerror` is non-`nil`, it silently uses `raw-text` coding instead.

<!---->

*   Function: **buffer-hash** *\&optional buffer-or-name*

    Return a hash of `buffer-or-name`. If `nil`, this defaults to the current buffer. As opposed to `secure-hash`, this function computes the hash based on the internal representation of the buffer, disregarding any coding systems. It’s therefore only useful when comparing two buffers running in the same Emacs, and is not guaranteed to return the same hash between different Emacs versions. It should be somewhat more efficient on larger buffers than `secure-hash` is, and should not allocate more memory.

Next: [GnuTLS Cryptography](GnuTLS-Cryptography.html), Previous: [Base 64](Base-64.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
