

Next: [SVG Images](SVG-Images.html), Previous: [XPM Images](XPM-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.17.5 ImageMagick Images

If your Emacs build has ImageMagick support, you can use the ImageMagick library to load many image formats (see [File Conveniences](https://www.gnu.org/software/emacs/manual/html_node/emacs/File-Conveniences.html#File-Conveniences) in The GNU Emacs Manual). The image type symbol for images loaded via ImageMagick is `imagemagick`, regardless of the actual underlying image format.

To check for ImageMagick support, use the following:

```lisp
(image-type-available-p 'imagemagick)
```

*   Function: **imagemagick-types**

    This function returns a list of image file extensions supported by the current ImageMagick installation. Each list element is a symbol representing an internal ImageMagick name for an image type, such as `BMP` for `.bmp` images.

<!---->

*   User Option: **imagemagick-enabled-types**

    The value of this variable is a list of ImageMagick image types which Emacs may attempt to render using ImageMagick. Each list element should be one of the symbols in the list returned by `imagemagick-types`, or an equivalent string. Alternatively, a value of `t` enables ImageMagick for all possible image types. Regardless of the value of this variable, `imagemagick-types-inhibit` (see below) takes precedence.

<!---->

*   User Option: **imagemagick-types-inhibit**

    The value of this variable lists the ImageMagick image types which should never be rendered using ImageMagick, regardless of the value of `imagemagick-enabled-types`. A value of `t` disables ImageMagick entirely.

<!---->

*   Variable: **image-format-suffixes**

    This variable is an alist mapping image types to file name extensions. Emacs uses this in conjunction with the `:format` image property (see below) to give a hint to the ImageMagick library as to the type of an image. Each element has the form `(type extension)`, where `type` is a symbol specifying an image content-type, and `extension` is a string that specifies the associated file name extension.

Images loaded with ImageMagick support the following additional image descriptor properties:

*   `:background background`

    `background`, if non-`nil`, should be a string specifying a color, which is used as the image’s background color if the image supports transparency. If the value is `nil`, it defaults to the frame’s background color.

*   `:format type`

    The value, `type`, should be a symbol specifying the type of the image data, as found in `image-format-suffixes`. This is used when the image does not have an associated file name, to provide a hint to ImageMagick to help it detect the image type.

*   `:crop geometry`

    The value of `geometry` should be a list of the form `(width height x y)`. `width` and `height` specify the width and height of the cropped image. If `x` is a positive number it specifies the offset of the cropped area from the left of the original image, and if negative the offset from the right. If `y` is a positive number it specifies the offset from the top of the original image, and if negative from the bottom. If `x` or `y` are `nil` or unspecified the crop area will be centered on the original image.

    If the crop area is outside or overlaps the edge of the image it will be reduced to exclude any areas outside of the image. This means it is not possible to use `:crop` to increase the size of the image by entering large `width` or `height` values.

    Cropping is performed after scaling but before rotation.

Next: [SVG Images](SVG-Images.html), Previous: [XPM Images](XPM-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
