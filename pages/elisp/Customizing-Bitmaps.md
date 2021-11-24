

#### 39.13.5 Customizing Fringe Bitmaps

### Function: **define-fringe-bitmap** *bitmap bits \&optional height width align*

This function defines the symbol `bitmap` as a new fringe bitmap, or replaces an existing bitmap with that name.

The argument `bits` specifies the image to use. It should be either a string or a vector of integers, where each element (an integer) corresponds to one row of the bitmap. Each bit of an integer corresponds to one pixel of the bitmap, where the low bit corresponds to the rightmost pixel of the bitmap. (Note that this order of bits is opposite of the order in XBM images; see [XBM Images](XBM-Images.html).)

The height is normally the length of `bits`. However, you can specify a different height with non-`nil` `height`. The width is normally 8, but you can specify a different width with non-`nil` `width`. The width must be an integer between 1 and 16.

The argument `align` specifies the positioning of the bitmap relative to the range of rows where it is used; the default is to center the bitmap. The allowed values are `top`, `center`, or `bottom`.

The `align` argument may also be a list `(align periodic)` where `align` is interpreted as described above. If `periodic` is non-`nil`, it specifies that the rows in `bits` should be repeated enough times to reach the specified height.

### Function: **destroy-fringe-bitmap** *bitmap*

This function destroys the fringe bitmap identified by `bitmap`. If `bitmap` identifies a standard fringe bitmap, it actually restores the standard definition of that bitmap, instead of eliminating it entirely.

### Function: **set-fringe-bitmap-face** *bitmap \&optional face*

This sets the face for the fringe bitmap `bitmap` to `face`. If `face` is `nil`, it selects the `fringe` face. The bitmap’s face controls the color to draw it in.

`face` is merged with the `fringe` face, so normally `face` should specify only the foreground color.
