

Previous: [Resources](Resources.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.25 Display Feature Testing

The functions in this section describe the basic capabilities of a particular display. Lisp programs can use them to adapt their behavior to what the display can do. For example, a program that ordinarily uses a popup menu could use the minibuffer if popup menus are not supported.

The optional argument `display` in these functions specifies which display to ask the question about. It can be a display name, a frame (which designates the display that frame is on), or `nil` (which refers to the selected frame’s display, see [Input Focus](Input-Focus.html)).

See [Color Names](Color-Names.html), [Text Terminal Colors](Text-Terminal-Colors.html), for other functions to obtain information about displays.

*   Function: **display-popup-menus-p** *\&optional display*

    This function returns `t` if popup menus are supported on `display`, `nil` if not. Support for popup menus requires that the mouse be available, since the menu is popped up by clicking the mouse on some portion of the Emacs display.

<!---->

*   Function: **display-graphic-p** *\&optional display*

    This function returns `t` if `display` is a graphic display capable of displaying several frames and several different fonts at once. This is true for displays that use a window system such as X, and false for text terminals.

<!---->

*   Function: **display-mouse-p** *\&optional display*

    This function returns `t` if `display` has a mouse available, `nil` if not.

<!---->

*   Function: **display-color-p** *\&optional display*

    This function returns `t` if the screen is a color screen. It used to be called `x-display-color-p`, and that name is still supported as an alias.

<!---->

*   Function: **display-grayscale-p** *\&optional display*

    This function returns `t` if the screen can display shades of gray. (All color displays can do this.)

<!---->

*   Function: **display-supports-face-attributes-p** *attributes \&optional display*

    This function returns non-`nil` if all the face attributes in `attributes` are supported (see [Face Attributes](Face-Attributes.html)).

    The definition of “supported” is somewhat heuristic, but basically means that a face containing all the attributes in `attributes`, when merged with the default face for display, can be represented in a way that’s

    1.  different in appearance than the default face, and
    2.  close in spirit to what the attributes specify, if not exact.

    Point (2) implies that a `:weight black` attribute will be satisfied by any display that can display bold, as will `:foreground "yellow"` as long as some yellowish color can be displayed, but `:slant italic` will *not* be satisfied by the tty display code’s automatic substitution of a dim face for italic.

<!---->

*   Function: **display-selections-p** *\&optional display*

    This function returns `t` if `display` supports selections. Windowed displays normally support selections, but they may also be supported in some other cases.

<!---->

*   Function: **display-images-p** *\&optional display*

    This function returns `t` if `display` can display images. Windowed displays ought in principle to handle images, but some systems lack the support for that. On a display that does not support images, Emacs cannot display a tool bar.

<!---->

*   Function: **display-screens** *\&optional display*

    This function returns the number of screens associated with the display.

<!---->

*   Function: **display-pixel-height** *\&optional display*

    This function returns the height of the screen in pixels. On a character terminal, it gives the height in characters.

    For graphical terminals, note that on multi-monitor setups this refers to the pixel height for all physical monitors associated with `display`. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   Function: **display-pixel-width** *\&optional display*

    This function returns the width of the screen in pixels. On a character terminal, it gives the width in characters.

    For graphical terminals, note that on multi-monitor setups this refers to the pixel width for all physical monitors associated with `display`. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   Function: **display-mm-height** *\&optional display*

    This function returns the height of the screen in millimeters, or `nil` if Emacs cannot get that information.

    For graphical terminals, note that on multi-monitor setups this refers to the height for all physical monitors associated with `display`. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   Function: **display-mm-width** *\&optional display*

    This function returns the width of the screen in millimeters, or `nil` if Emacs cannot get that information.

    For graphical terminals, note that on multi-monitor setups this refers to the width for all physical monitors associated with `display`. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   User Option: **display-mm-dimensions-alist**

    This variable allows the user to specify the dimensions of graphical displays returned by `display-mm-height` and `display-mm-width` in case the system provides incorrect values.

<!---->

*   Function: **display-backing-store** *\&optional display*

    This function returns the backing store capability of the display. Backing store means recording the pixels of windows (and parts of windows) that are not exposed, so that when exposed they can be displayed very quickly.

    Values can be the symbols `always`, `when-mapped`, or `not-useful`. The function can also return `nil` when the question is inapplicable to a certain kind of display.

<!---->

*   Function: **display-save-under** *\&optional display*

    This function returns non-`nil` if the display supports the SaveUnder feature. That feature is used by pop-up windows to save the pixels they obscure, so that they can pop down quickly.

<!---->

*   Function: **display-planes** *\&optional display*

    This function returns the number of planes the display supports. This is typically the number of bits per pixel. For a tty display, it is log to base two of the number of colors supported.

<!---->

*   Function: **display-visual-class** *\&optional display*

    This function returns the visual class for the screen. The value is one of the symbols `static-gray` (a limited, unchangeable number of grays), `gray-scale` (a full range of grays), `static-color` (a limited, unchangeable number of colors), `pseudo-color` (a limited number of colors), `true-color` (a full range of colors), and `direct-color` (a full range of colors).

<!---->

*   Function: **display-color-cells** *\&optional display*

    This function returns the number of color cells the screen supports.

These functions obtain additional information about the window system in use where Emacs shows the specified `display`. (Their names begin with `x-` for historical reasons.)

*   Function: **x-server-version** *\&optional display*

    This function returns the list of version numbers of the GUI window system running on `display`, such as the X server on GNU and Unix systems. The value is a list of three integers: the major and minor version numbers of the protocol, and the distributor-specific release number of the window system software itself. On GNU and Unix systems, these are normally the version of the X protocol and the distributor-specific release number of the X server software. On MS-Windows, this is the version of the Windows OS.

<!---->

*   Function: **x-server-vendor** *\&optional display*

    This function returns the vendor that provided the window system software (as a string). On GNU and Unix systems this really means whoever distributes the X server. On MS-Windows this is the vendor ID string of the Windows OS (Microsoft).

    When the developers of X labeled software distributors as “vendors”, they showed their false assumption that no system could ever be developed and distributed noncommercially.

Previous: [Resources](Resources.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
