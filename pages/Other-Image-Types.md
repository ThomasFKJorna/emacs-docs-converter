

Next: [Defining Images](Defining-Images.html), Previous: [SVG Images](SVG-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.17.7 Other Image Types

For PBM images, specify image type `pbm`. Color, gray-scale and monochromatic images are supported. For mono PBM images, two additional image properties are supported.

*   `:foreground foreground`

    The value, `foreground`, should be a string specifying the image foreground color, or `nil` for the default color. This color is used for each pixel in the PBM that is 1. The default is the frame’s foreground color.

*   `:background background`

    The value, `background`, should be a string specifying the image background color, or `nil` for the default color. This color is used for each pixel in the PBM that is 0. The default is the frame’s background color.

The remaining image types that Emacs can support are:

*   GIF

    Image type `gif`. Supports the `:index` property. See [Multi-Frame Images](Multi_002dFrame-Images.html).

*   JPEG

    Image type `jpeg`.

*   PNG

    Image type `png`.

*   TIFF

    Image type `tiff`. Supports the `:index` property. See [Multi-Frame Images](Multi_002dFrame-Images.html).
