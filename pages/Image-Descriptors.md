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

Next: [XBM Images](XBM-Images.html), Previous: [Image Formats](Image-Formats.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.17.2 Image Descriptors

An *image descriptor* is a list which specifies the underlying data for an image, and how to display it. It is typically used as the value of a `display` overlay or text property (see [Other Display Specs](Other-Display-Specs.html)); but See [Showing Images](Showing-Images.html), for convenient helper functions to insert images into buffers.

Each image descriptor has the form `(image . props)`, where `props` is a property list of alternating keyword symbols and values, including at least the pair `:type type` that specifies the image type.

The following is a list of properties that are meaningful for all image types (there are also properties which are meaningful only for certain image types, as documented in the following subsections):

*   `:type type`

    The image type. See [Image Formats](Image-Formats.html). Every image descriptor must include this property.

*   `:file file`

    This says to load the image from file `file`. If `file` is not an absolute file name, it is expanded relative to the `images` subdirectory of `data-directory`, and failing that, relative to the directories listed by `x-bitmap-file-path` (see [Face Attributes](Face-Attributes.html)).

*   `:data data`

    This specifies the raw image data. Each image descriptor must have either `:data` or `:file`, but not both.

    For most image types, the value of a `:data` property should be a string containing the image data. Some image types do not support `:data`; for some others, `:data` alone is not enough, so you need to use other image properties along with `:data`. See the following subsections for details.

*   `:margin margin`

    This specifies how many pixels to add as an extra margin around the image. The value, `margin`, must be a non-negative number, or a pair `(x . y)` of such numbers. If it is a pair, `x` specifies how many pixels to add horizontally, and `y` specifies how many pixels to add vertically. If `:margin` is not specified, the default is zero.

*   `:ascent ascent`

    This specifies the amount of the image’s height to use for its ascent—that is, the part above the baseline. The value, `ascent`, must be a number in the range 0 to 100, or the symbol `center`.

    If `ascent` is a number, that percentage of the image’s height is used for its ascent.

    If `ascent` is `center`, the image is vertically centered around a centerline which would be the vertical centerline of text drawn at the position of the image, in the manner specified by the text properties and overlays that apply to the image.

    If this property is omitted, it defaults to 50.

*   `:relief relief`

    This adds a shadow rectangle around the image. The value, `relief`, specifies the width of the shadow lines, in pixels. If `relief` is negative, shadows are drawn so that the image appears as a pressed button; otherwise, it appears as an unpressed button.

*   `:width width, :height height`

    The `:width` and `:height` keywords are used for scaling the image. If only one of them is specified, the other one will be calculated so as to preserve the aspect ratio. If both are specified, aspect ratio may not be preserved.

*   `:max-width max-width, :max-height max-height`

    The `:max-width` and `:max-height` keywords are used for scaling if the size of the image exceeds these values. If `:width` is set, it will have precedence over `max-width`, and if `:height` is set, it will have precedence over `max-height`, but you can otherwise mix these keywords as you wish.

    If both `:max-width` and `:height` are specified, but `:width` is not, preserving the aspect ratio might require that width exceeds `:max-width`. If this happens, scaling will use a smaller value for the height so as to preserve the aspect ratio while not exceeding `:max-width`. Similarly when both `:max-height` and `:width` are specified, but `:height` is not. For example, if you have a 200x100 image and specify that `:width` should be 400 and `:max-height` should be 150, you’ll end up with an image that is 300x150: Preserving the aspect ratio and not exceeding the “max” setting. This combination of parameters is a useful way of saying “display this image as large as possible, but no larger than the available display area”.

*   `:scale scale`

    This should be a number, where values higher than 1 means to increase the size, and lower means to decrease the size, by multiplying both the width and height. For instance, a value of 0.25 will make the image a quarter size of what it originally was. If the scaling makes the image larger than specified by `:max-width` or `:max-height`, the resulting size will not exceed those two values. If both `:scale` and `:height`/`:width` are specified, the height/width will be adjusted by the specified scaling factor.

*   `:rotation angle`

    Specifies a rotation angle in degrees. Only multiples of 90 degrees are supported, unless the image type is `imagemagick`. Positive values rotate clockwise, negative values counter-clockwise. Rotation is performed after scaling and cropping.

*   `:index frame`

    See [Multi-Frame Images](Multi_002dFrame-Images.html).

*   `:conversion algorithm`

    This specifies a conversion algorithm that should be applied to the image before it is displayed; the value, `algorithm`, specifies which algorithm.

    *   *   `laplace`
        *   `emboss`

        Specifies the Laplace edge detection algorithm, which blurs out small differences in color while highlighting larger differences. People sometimes consider this useful for displaying the image for a disabled button.

    *   `(edge-detection :matrix matrix :color-adjust adjust)`

        Specifies a general edge-detection algorithm. `matrix` must be either a nine-element list or a nine-element vector of numbers. A pixel at position *x/y* in the transformed image is computed from original pixels around that position. `matrix` specifies, for each pixel in the neighborhood of *x/y*, a factor with which that pixel will influence the transformed pixel; element *0* specifies the factor for the pixel at *x-1/y-1*, element *1* the factor for the pixel at *x/y-1* etc., as shown below:

              (x-1/y-1  x/y-1  x+1/y-1
               x-1/y    x/y    x+1/y
               x-1/y+1  x/y+1  x+1/y+1)

        The resulting pixel is computed from the color intensity of the color resulting from summing up the RGB values of surrounding pixels, multiplied by the specified factors, and dividing that sum by the sum of the factors’ absolute values.

        Laplace edge-detection currently uses a matrix of

              (1  0  0
               0  0  0
               0  0 -1)

        Emboss edge-detection uses a matrix of

              ( 2 -1  0
               -1  0  1
                0  1 -2)

    *   `disabled`

        Specifies transforming the image so that it looks disabled.

*   `:mask mask`

    If `mask` is `heuristic` or `(heuristic bg)`, build a clipping mask for the image, so that the background of a frame is visible behind the image. If `bg` is not specified, or if `bg` is `t`, determine the background color of the image by looking at the four corners of the image, assuming the most frequently occurring color from the corners is the background color of the image. Otherwise, `bg` must be a list `(red green blue)` specifying the color to assume for the background of the image.

    If `mask` is `nil`, remove a mask from the image, if it has one. Images in some formats include a mask which can be removed by specifying `:mask nil`.

*   `:pointer shape`

    This specifies the pointer shape when the mouse pointer is over this image. See [Pointer Shape](Pointer-Shape.html), for available pointer shapes.

*   `:map map`

    This associates an image map of *hot spots* with this image.

    An image map is an alist where each element has the format `(area id plist)`. An `area` is specified as either a rectangle, a circle, or a polygon.

    A rectangle is a cons `(rect . ((x0 . y0) . (x1 . y1)))` which specifies the pixel coordinates of the upper left and bottom right corners of the rectangle area.

    A circle is a cons `(circle . ((x0 . y0) . r))` which specifies the center and the radius of the circle; `r` may be a float or integer.

    A polygon is a cons `(poly . [x0 y0 x1 y1 ...])` where each pair in the vector describes one corner in the polygon.

    When the mouse pointer lies on a hot-spot area of an image, the `plist` of that hot-spot is consulted; if it contains a `help-echo` property, that defines a tool-tip for the hot-spot, and if it contains a `pointer` property, that defines the shape of the mouse cursor when it is on the hot-spot. See [Pointer Shape](Pointer-Shape.html), for available pointer shapes.

    When you click the mouse when the mouse pointer is over a hot-spot, an event is composed by combining the `id` of the hot-spot with the mouse event; for instance, `[area4 mouse-1]` if the hot-spot’s `id` is `area4`.

<!---->

*   Function: **image-mask-p** *spec \&optional frame*

    This function returns `t` if image `spec` has a mask bitmap. `frame` is the frame on which the image will be displayed. `frame` `nil` or omitted means to use the selected frame (see [Input Focus](Input-Focus.html)).

<!---->

*   Function: **image-transforms-p** *\&optional frame*

    This function returns non-`nil` if `frame` supports image scaling and rotation. `frame` `nil` or omitted means to use the selected frame (see [Input Focus](Input-Focus.html)). The returned list includes symbols that indicate which image transform operations are supported:

    *   `scale`

        Image scaling is supported by `frame` via the `:scale`, `:width`, `:height`, `:max-width`, and `:max-height` properties.

    *   `rotate90`

        Image rotation is supported by `frame` if the rotation angle is an integral multiple of 90 degrees.

    If image transforms are not supported, `:rotation`, `:crop`, `:width`, `:height`, `:scale`, `:max-width` and `:max-height` will only be usable through ImageMagick, if available (see [ImageMagick Images](ImageMagick-Images.html)).

Next: [XBM Images](XBM-Images.html), Previous: [Image Formats](Image-Formats.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
