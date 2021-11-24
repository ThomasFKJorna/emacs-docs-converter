

### 33.4 Selecting a Representation

Sometimes it is useful to examine an existing buffer or string as multibyte when it was unibyte, or vice versa.

### Function: **set-buffer-multibyte** *multibyte*

Set the representation type of the current buffer. If `multibyte` is non-`nil`, the buffer becomes multibyte. If `multibyte` is `nil`, the buffer becomes unibyte.

This function leaves the buffer contents unchanged when viewed as a sequence of bytes. As a consequence, it can change the contents viewed as characters; for instance, a sequence of three bytes which is treated as one character in multibyte representation will count as three characters in unibyte representation. Eight-bit characters representing raw bytes are an exception. They are represented by one byte in a unibyte buffer, but when the buffer is set to multibyte, they are converted to two-byte sequences, and vice versa.

This function sets `enable-multibyte-characters` to record which representation is in use. It also adjusts various data in the buffer (including overlays, text properties and markers) so that they cover the same text as they did before.

This function signals an error if the buffer is narrowed, since the narrowing might have occurred in the middle of multibyte character sequences.

This function also signals an error if the buffer is an indirect buffer. An indirect buffer always inherits the representation of its base buffer.

### Function: **string-as-unibyte** *string*

If `string` is already a unibyte string, this function returns `string` itself. Otherwise, it returns a new string with the same bytes as `string`, but treating each byte as a separate character (so that the value may have more characters than `string`); as an exception, each eight-bit character representing a raw byte is converted into a single byte. The newly-created string contains no text properties.

### Function: **string-as-multibyte** *string*

If `string` is a multibyte string, this function returns `string` itself. Otherwise, it returns a new string with the same bytes as `string`, but treating each multibyte sequence as one character. This means that the value may have fewer characters than `string` has. If a byte sequence in `string` is invalid as a multibyte representation of a single character, each byte in the sequence is treated as a raw 8-bit byte. The newly-created string contains no text properties.
