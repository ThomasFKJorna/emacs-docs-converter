

#### 39.17.4 XPM Images

To use XPM format, specify `xpm` as the image type. The additional image property `:color-symbols` is also meaningful with the `xpm` image type:

### `:color-symbols symbols`

The value, `symbols`, should be an alist whose elements have the form `(name . color)`. In each element, `name` is the name of a color as it appears in the image file, and `color` specifies the actual color to use for displaying that name.
