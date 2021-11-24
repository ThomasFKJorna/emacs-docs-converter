

### 38.20 Packing and Unpacking Byte Arrays

This section describes how to pack and unpack arrays of bytes, usually for binary network protocols. These functions convert byte arrays to alists, and vice versa. The byte array can be represented as a unibyte string or as a vector of integers, while the alist associates symbols either with fixed-size objects or with recursive sub-alists. To use the functions referred to in this section, load the `bindat` library.

Conversion from byte arrays to nested alists is also known as *deserializing* or *unpacking*, while going in the opposite direction is also known as *serializing* or *packing*.

|                                             |    |                                  |
| :------------------------------------------ | -- | :------------------------------- |
| • [Bindat Spec](Bindat-Spec.html)           |    | Describing data layout.          |
| • [Bindat Functions](Bindat-Functions.html) |    | Doing the unpacking and packing. |
