

Next: [XPM Images](XPM-Images.html), Previous: [Image Descriptors](Image-Descriptors.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.17.3 XBM Images

To use XBM format, specify `xbm` as the image type. This image format doesn’t require an external library, so images of this type are always supported.

Additional image properties supported for the `xbm` image type are:

*   `:foreground foreground`

    The value, `foreground`, should be a string specifying the image foreground color, or `nil` for the default color. This color is used for each pixel in the XBM that is 1. The default is the frame’s foreground color.

*   `:background background`

    The value, `background`, should be a string specifying the image background color, or `nil` for the default color. This color is used for each pixel in the XBM that is 0. The default is the frame’s background color.

If you specify an XBM image using data within Emacs instead of an external file, use the following three properties:

*   `:data data`

    The value, `data`, specifies the contents of the image. There are three formats you can use for `data`:

    *   A vector of strings or bool-vectors, each specifying one line of the image. Do specify `:height` and `:width`.
    *   A string containing the same byte sequence as an XBM file would contain. You must not specify `:height` and `:width` in this case, because omitting them is what indicates the data has the format of an XBM file. The file contents specify the height and width of the image.
    *   A string or a bool-vector containing the bits of the image (plus perhaps some extra bits at the end that will not be used). It should contain at least `stride * height` bits, where `stride` is the smallest multiple of 8 greater than or equal to the width of the image. In this case, you should specify `:height`, `:width` and `:stride`, both to indicate that the string contains just the bits rather than a whole XBM file, and to specify the size of the image.

*   `:width width`

    The value, `width`, specifies the width of the image, in pixels.

*   `:height height`

    The value, `height`, specifies the height of the image, in pixels.

*   `:stride stride`

    The number of bool vector entries stored for each row; the smallest multiple of 8 greater than or equal to `width`.

Next: [XPM Images](XPM-Images.html), Previous: [Image Descriptors](Image-Descriptors.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
