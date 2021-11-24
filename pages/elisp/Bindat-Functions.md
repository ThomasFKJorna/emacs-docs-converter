

#### 38.20.2 Functions to Unpack and Pack Bytes

In the following documentation, `spec` refers to a data layout specification, `bindat-raw` to a byte array, and `struct` to an alist representing unpacked field data.

### Function: **bindat-unpack** *spec bindat-raw \&optional bindat-idx*

This function unpacks data from the unibyte string or byte array `bindat-raw` according to `spec`. Normally, this starts unpacking at the beginning of the byte array, but if `bindat-idx` is non-`nil`, it specifies a zero-based starting position to use instead.

The value is an alist or nested alist in which each element describes one unpacked field.

### Function: **bindat-get-field** *struct \&rest name*

This function selects a field’s data from the nested alist `struct`. Usually `struct` was returned by `bindat-unpack`. If `name` corresponds to just one argument, that means to extract a top-level field value. Multiple `name` arguments specify repeated lookup of sub-structures. An integer name acts as an array index.

For example, if `name` is `(a b 2 c)`, that means to find field `c` in the third element of subfield `b` of field `a`. (This corresponds to `struct.a.b[2].c` in C.)

Although packing and unpacking operations change the organization of data (in memory), they preserve the data’s *total length*, which is the sum of all the fields’ lengths, in bytes. This value is not generally inherent in either the specification or alist alone; instead, both pieces of information contribute to its calculation. Likewise, the length of a string or array being unpacked may be longer than the data’s total length as described by the specification.

### Function: **bindat-length** *spec struct*

This function returns the total length of the data in `struct`, according to `spec`.

### Function: **bindat-pack** *spec struct \&optional bindat-raw bindat-idx*

This function returns a byte array packed according to `spec` from the data in the alist `struct`. It normally creates and fills a new byte array starting at the beginning. However, if `bindat-raw` is non-`nil`, it specifies a pre-allocated unibyte string or vector to pack into. If `bindat-idx` is non-`nil`, it specifies the starting offset for packing into `bindat-raw`.

When pre-allocating, you should make sure `(length bindat-raw)` meets or exceeds the total length to avoid an out-of-range error.

### Function: **bindat-ip-to-string** *ip*

Convert the Internet address vector `ip` to a string in the usual dotted notation.

```lisp
(bindat-ip-to-string [127 0 0 1])
     ⇒ "127.0.0.1"
```
