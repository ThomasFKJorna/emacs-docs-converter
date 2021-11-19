

Next: [Image Cache](Image-Cache.html), Previous: [Showing Images](Showing-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.17.10 Multi-Frame Images

Some image files can contain more than one image. We say that there are multiple “frames” in the image. At present, Emacs supports multiple frames for GIF, TIFF, and certain ImageMagick formats such as DJVM.

The frames can be used either to represent multiple pages (this is usually the case with multi-frame TIFF files, for example), or to create animation (usually the case with multi-frame GIF files).

A multi-frame image has a property `:index`, whose value is an integer (counting from 0) that specifies which frame is being displayed.

*   Function: **image-multi-frame-p** *image*

    This function returns non-`nil` if `image` contains more than one frame. The actual return value is a cons `(nimages . delay)`, where `nimages` is the number of frames and `delay` is the delay in seconds between them, or `nil` if the image does not specify a delay. Images that are intended to be animated usually specify a frame delay, whereas ones that are intended to be treated as multiple pages do not.

<!---->

*   Function: **image-current-frame** *image*

    This function returns the index of the current frame number for `image`, counting from 0.

<!---->

*   Function: **image-show-frame** *image n \&optional nocheck*

    This function switches `image` to frame number `n`. It replaces a frame number outside the valid range with that of the end of the range, unless `nocheck` is non-`nil`. If `image` does not contain a frame with the specified number, the image displays as a hollow box.

<!---->

*   Function: **image-animate** *image \&optional index limit*

    This function animates `image`. The optional integer `index` specifies the frame from which to start (default 0). The optional argument `limit` controls the length of the animation. If omitted or `nil`, the image animates once only; if `t` it loops forever; if a number animation stops after that many seconds.

Animation operates by means of a timer. Note that Emacs imposes a minimum frame delay of 0.01 (`image-minimum-frame-delay`) seconds. If the image itself does not specify a delay, Emacs uses `image-default-frame-delay`.

*   Function: **image-animate-timer** *image*

    This function returns the timer responsible for animating `image`, if there is one.

Next: [Image Cache](Image-Cache.html), Previous: [Showing Images](Showing-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
