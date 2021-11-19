

Next: [Drag and Drop](Drag-and-Drop.html), Previous: [Pointer Shape](Pointer-Shape.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.20 Window System Selections

In window systems, such as X, data can be transferred between different applications by means of *selections*. X defines an arbitrary number of *selection types*, each of which can store its own data; however, only three are commonly used: the *clipboard*, *primary selection*, and *secondary selection*. Other window systems support only the clipboard. See [Cut and Paste](https://www.gnu.org/software/emacs/manual/html_node/emacs/Cut-and-Paste.html#Cut-and-Paste) in The GNU Emacs Manual, for Emacs commands that make use of these selections. This section documents the low-level functions for reading and setting window-system selections.

*   Command: **gui-set-selection** *type data*

    This function sets a window-system selection. It takes two arguments: a selection type `type`, and the value to assign to it, `data`.

    `type` should be a symbol; it is usually one of `PRIMARY`, `SECONDARY` or `CLIPBOARD`. These are symbols with upper-case names, in accord with X Window System conventions. If `type` is `nil`, that stands for `PRIMARY`.

    If `data` is `nil`, it means to clear out the selection. Otherwise, `data` may be a string, a symbol, an integer (or a cons of two integers or list of two integers), an overlay, or a cons of two markers pointing to the same buffer. An overlay or a pair of markers stands for text in the overlay or between the markers. The argument `data` may also be a vector of valid non-vector selection values.

    This function returns `data`.

<!---->

*   Function: **gui-get-selection** *\&optional type data-type*

    This function accesses selections set up by Emacs or by other programs. It takes two optional arguments, `type` and `data-type`. The default for `type`, the selection type, is `PRIMARY`.

    The `data-type` argument specifies the form of data conversion to use, to convert the raw data obtained from another program into Lisp data. Meaningful values include `TEXT`, `STRING`, `UTF8_STRING`, `TARGETS`, `LENGTH`, `DELETE`, `FILE_NAME`, `CHARACTER_POSITION`, `NAME`, `LINE_NUMBER`, `COLUMN_NUMBER`, `OWNER_OS`, `HOST_NAME`, `USER`, `CLASS`, `ATOM`, and `INTEGER`. (These are symbols with upper-case names in accord with X conventions.) The default for `data-type` is `STRING`. Window systems other than X usually support only a small subset of these types, in addition to `STRING`.

<!---->

*   User Option: **selection-coding-system**

    This variable specifies the coding system to use when reading and writing selections or the clipboard. See [Coding Systems](Coding-Systems.html). The default is `compound-text-with-extensions`, which converts to the text representation that X11 normally uses.

When Emacs runs on MS-Windows, it does not implement X selections in general, but it does support the clipboard. `gui-get-selection` and `gui-set-selection` on MS-Windows support the text data type only; if the clipboard holds other types of data, Emacs treats the clipboard as empty. The supported data type is `STRING`.

For backward compatibility, there are obsolete aliases `x-get-selection` and `x-set-selection`, which were the names of `gui-get-selection` and `gui-set-selection` before Emacs 25.1.

Next: [Drag and Drop](Drag-and-Drop.html), Previous: [Pointer Shape](Pointer-Shape.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
