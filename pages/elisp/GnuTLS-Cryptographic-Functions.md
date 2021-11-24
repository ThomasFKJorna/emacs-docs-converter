

#### 32.27.2 GnuTLS Cryptographic Functions

### Function: **gnutls-digests**

This function returns the alist of the GnuTLS digest algorithms.

Each entry has a key which represents the algorithm, followed by a plist with internal details about the algorithm. The plist will have `:type gnutls-digest-algorithm` and also will have the key `:digest-algorithm-length 64` to indicate the size, in bytes, of the resulting digest.

There is a name parallel between GnuTLS MAC and digest algorithms but they are separate things internally and should not be mixed.

### Function: **gnutls-hash-digest** *digest-method input*

The `digest-method` can be the whole plist from `gnutls-digests`, or just the symbol key, or a string with the name of that symbol.

The `input` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)).

This function returns `nil` on error, and signals a Lisp error if the `digest-method` or `input` are invalid. On success, it returns a list of a binary string (the output) and the IV used.

### Function: **gnutls-macs**

This function returns the alist of the GnuTLS MAC algorithms.

Each entry has a key which represents the algorithm, followed by a plist with internal details about the algorithm. The plist will have `:type gnutls-mac-algorithm` and also will have the keys `:mac-algorithm-length` `:mac-algorithm-keysize` `:mac-algorithm-noncesize` to indicate the size, in bytes, of the resulting hash, the key, and the nonce respectively.

The nonce is currently unused and only some MACs support it.

There is a name parallel between GnuTLS MAC and digest algorithms but they are separate things internally and should not be mixed.

### Function: **gnutls-hash-mac** *hash-method key input*

The `hash-method` can be the whole plist from `gnutls-macs`, or just the symbol key, or a string with the name of that symbol.

The `key` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)). The `key` will be wiped after use if it’s a string.

The `input` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)).

This function returns `nil` on error, and signals a Lisp error if the `hash-method` or `key` or `input` are invalid.

On success, it returns a list of a binary string (the output) and the IV used.

### Function: **gnutls-ciphers**

This function returns the alist of the GnuTLS ciphers.

Each entry has a key which represents the cipher, followed by a plist with internal details about the algorithm. The plist will have `:type gnutls-symmetric-cipher` and also will have the keys `:cipher-aead-capable` set to `nil` or `t` to indicate AEAD capability; and `:cipher-tagsize` `:cipher-blocksize` `:cipher-keysize` `:cipher-ivsize` to indicate the size, in bytes, of the tag, block size of the resulting data, the key, and the IV respectively.

### Function: **gnutls-symmetric-encrypt** *cipher key iv input \&optional aead\_auth*

The `cipher` can be the whole plist from `gnutls-ciphers`, or just the symbol key, or a string with the name of that symbol.

The `key` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)). The `key` will be wiped after use if it’s a string.

The `iv` and `input` and the optional `aead_auth` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)).

`aead_auth` is only checked with AEAD ciphers, that is, ciphers whose plist has `:cipher-aead-capable t`. Otherwise it’s ignored.

This function returns `nil` on error, and signals a Lisp error if the `cipher` or `key`, `iv`, or `input` are invalid, or if `aead_auth` was specified with an AEAD cipher and was invalid.

On success, it returns a list of a binary string (the output) and the IV used.

### Function: **gnutls-symmetric-decrypt** *cipher key iv input \&optional aead\_auth*

The `cipher` can be the whole plist from `gnutls-ciphers`, or just the symbol key, or a string with the name of that symbol.

The `key` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)). The `key` will be wiped after use if it’s a string.

The `iv` and `input` and the optional `aead_auth` can be specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](Format-of-GnuTLS-Cryptography-Inputs.html)).

`aead_auth` is only checked with AEAD ciphers, that is, ciphers whose plist has `:cipher-aead-capable t`. Otherwise it’s ignored.

This function returns `nil` on decryption error, and signals a Lisp error if the `cipher` or `key`, `iv`, or `input` are invalid, or if `aead_auth` was specified with an AEAD cipher and was invalid.

On success, it returns a list of a binary string (the output) and the IV used.
