

#### 32.27.1 Format of GnuTLS Cryptography Inputs

The inputs to GnuTLS cryptographic functions can be specified in several ways, both as primitive Emacs Lisp types or as lists.

The list form is currently similar to how `md5` and `secure-hash` operate.

`buffer`

Simply passing a buffer as input means the whole buffer should be used.

`string`

A string as input will be used directly. It may be modified by the function (unlike most other Emacs Lisp functions) to reduce the chance of exposing sensitive data after the function does its work.

`(buffer-or-string start end coding-system noerror)`

This specifies a buffer or a string as described above, but an optional range can be specified with `start` and `end`.

In addition an optional `coding-system` can be specified if needed.

The last optional item, `noerror`, overrides the normal error when the text can’t be encoded using the specified or chosen coding system. When `noerror` is non-`nil`, this function silently uses `raw-text` coding instead.

`(iv-auto length)`

This will generate an IV (Initialization Vector) of the specified length using the GnuTLS `GNUTLS_RND_NONCE` generator and pass it to the function. This ensures that the IV is unpredictable and unlikely to be reused in the same session. The actual value of the IV is returned by the function as described below.
