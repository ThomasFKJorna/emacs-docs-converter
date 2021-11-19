

Previous: [Multi-Frame Images](Multi_002dFrame-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.17.11 Image Cache

Emacs caches images so that it can display them again more efficiently. When Emacs displays an image, it searches the image cache for an existing image specification `equal` to the desired specification. If a match is found, the image is displayed from the cache. Otherwise, Emacs loads the image normally.

*   Function: **image-flush** *spec \&optional frame*

    This function removes the image with specification `spec` from the image cache of frame `frame`. Image specifications are compared using `equal`. If `frame` is `nil`, it defaults to the selected frame. If `frame` is `t`, the image is flushed on all existing frames.

    In Emacs’s current implementation, each graphical terminal possesses an image cache, which is shared by all the frames on that terminal (see [Multiple Terminals](Multiple-Terminals.html)). Thus, refreshing an image in one frame also refreshes it in all other frames on the same terminal.

One use for `image-flush` is to tell Emacs about a change in an image file. If an image specification contains a `:file` property, the image is cached based on the file’s contents when the image is first displayed. Even if the file subsequently changes, Emacs continues displaying the old version of the image. Calling `image-flush` flushes the image from the cache, forcing Emacs to re-read the file the next time it needs to display that image.

Another use for `image-flush` is for memory conservation. If your Lisp program creates a large number of temporary images over a period much shorter than `image-cache-eviction-delay` (see below), you can opt to flush unused images yourself, instead of waiting for Emacs to do it automatically.

*   Function: **clear-image-cache** *\&optional filter*

    This function clears an image cache, removing all the images stored in it. If `filter` is omitted or `nil`, it clears the cache for the selected frame. If `filter` is a frame, it clears the cache for that frame. If `filter` is `t`, all image caches are cleared. Otherwise, `filter` is taken to be a file name, and all images associated with that file name are removed from all image caches.

If an image in the image cache has not been displayed for a specified period of time, Emacs removes it from the cache and frees the associated memory.

*   Variable: **image-cache-eviction-delay**

    This variable specifies the number of seconds an image can remain in the cache without being displayed. When an image is not displayed for this length of time, Emacs removes it from the image cache.

    Under some circumstances, if the number of images in the cache grows too large, the actual eviction delay may be shorter than this.

    If the value is `nil`, Emacs does not remove images from the cache except when you explicitly clear it. This mode can be useful for debugging.

Previous: [Multi-Frame Images](Multi_002dFrame-Images.html), Up: [Images](Images.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
