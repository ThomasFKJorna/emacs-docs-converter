

Next: [Levels of Font Lock](Levels-of-Font-Lock.html), Previous: [Customizing Keywords](Customizing-Keywords.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.6.4 Other Font Lock Variables

This section describes additional variables that a major mode can set by means of `other-vars` in `font-lock-defaults` (see [Font Lock Basics](Font-Lock-Basics.html)).

*   Variable: **font-lock-mark-block-function**

    If this variable is non-`nil`, it should be a function that is called with no arguments, to choose an enclosing range of text for refontification for the command `M-o M-o` (`font-lock-fontify-block`).

    The function should report its choice by placing the region around it. A good choice is a range of text large enough to give proper results, but not too large so that refontification becomes slow. Typical values are `mark-defun` for programming modes or `mark-paragraph` for textual modes.

<!---->

*   Variable: **font-lock-extra-managed-props**

    This variable specifies additional properties (other than `font-lock-face`) that are being managed by Font Lock mode. It is used by `font-lock-default-unfontify-region`, which normally only manages the `font-lock-face` property. If you want Font Lock to manage other properties as well, you must specify them in a `facespec` in `font-lock-keywords` as well as add them to this list. See [Search-based Fontification](Search_002dbased-Fontification.html).

<!---->

*   Variable: **font-lock-fontify-buffer-function**

    Function to use for fontifying the buffer. The default value is `font-lock-default-fontify-buffer`.

<!---->

*   Variable: **font-lock-unfontify-buffer-function**

    Function to use for unfontifying the buffer. This is used when turning off Font Lock mode. The default value is `font-lock-default-unfontify-buffer`.

<!---->

*   Variable: **font-lock-fontify-region-function**

    Function to use for fontifying a region. It should take two arguments, the beginning and end of the region, and an optional third argument `verbose`. If `verbose` is non-`nil`, the function should print status messages. The default value is `font-lock-default-fontify-region`.

<!---->

*   Variable: **font-lock-unfontify-region-function**

    Function to use for unfontifying a region. It should take two arguments, the beginning and end of the region. The default value is `font-lock-default-unfontify-region`.

<!---->

*   Variable: **font-lock-flush-function**

    Function to use for declaring that a region’s fontification is out of date. It takes two arguments, the beginning and end of the region. The default value of this variable is `font-lock-after-change-function`.

<!---->

*   Variable: **font-lock-ensure-function**

    Function to use for making sure a region of the current buffer has been fontified. It is called with two arguments, the beginning and end of the region. The default value of this variable is a function that calls `font-lock-default-fontify-buffer` if the buffer is not fontified; the effect is to make sure the entire accessible portion of the buffer is fontified.

<!---->

*   Function: **jit-lock-register** *function \&optional contextual*

    This function tells Font Lock mode to run the Lisp function `function` any time it has to fontify or refontify part of the current buffer. It calls `function` before calling the default fontification functions, and gives it two arguments, `start` and `end`, which specify the region to be fontified or refontified.

    The optional argument `contextual`, if non-`nil`, forces Font Lock mode to always refontify a syntactically relevant part of the buffer, and not just the modified lines. This argument can usually be omitted.

    When Font Lock is activated in a buffer, it calls this function with a non-`nil` value of `contextual` if the value of `font-lock-keywords-only` (see [Syntactic Font Lock](Syntactic-Font-Lock.html)) is `nil`.

<!---->

*   Function: **jit-lock-unregister** *function*

    If `function` was previously registered as a fontification function using `jit-lock-register`, this function unregisters it.

Next: [Levels of Font Lock](Levels-of-Font-Lock.html), Previous: [Customizing Keywords](Customizing-Keywords.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
