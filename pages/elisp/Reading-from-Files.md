

### 25.3 Reading from Files

To copy the contents of a file into a buffer, use the function `insert-file-contents`. (Don’t use the command `insert-file` in a Lisp program, as that sets the mark.)

### Function: **insert-file-contents** *filename \&optional visit beg end replace*

This function inserts the contents of file `filename` into the current buffer after point. It returns a list of the absolute file name and the length of the data inserted. An error is signaled if `filename` is not the name of a file that can be read.

This function checks the file contents against the defined file formats, and converts the file contents if appropriate and also calls the functions in the list `after-insert-file-functions`. See [Format Conversion](Format-Conversion.html). Normally, one of the functions in the `after-insert-file-functions` list determines the coding system (see [Coding Systems](Coding-Systems.html)) used for decoding the file’s contents, including end-of-line conversion. However, if the file contains null bytes, it is by default visited without any code conversions. See [inhibit-nul-byte-detection](Lisp-and-Coding-Systems.html).

If `visit` is non-`nil`, this function additionally marks the buffer as unmodified and sets up various fields in the buffer so that it is visiting the file `filename`: these include the buffer’s visited file name and its last save file modtime. This feature is used by `find-file-noselect` and you probably should not use it yourself.

If `beg` and `end` are non-`nil`, they should be numbers that are byte offsets specifying the portion of the file to insert. In this case, `visit` must be `nil`. For example,

```lisp
(insert-file-contents filename nil 0 500)
```

inserts the first 500 characters of a file.

If the argument `replace` is non-`nil`, it means to replace the contents of the buffer (actually, just the accessible portion) with the contents of the file. This is better than simply deleting the buffer contents and inserting the whole file, because (1) it preserves some marker positions and (2) it puts less data in the undo list.

It is possible to read a special file (such as a FIFO or an I/O device) with `insert-file-contents`, as long as `replace` and `visit` are `nil`.

### Function: **insert-file-contents-literally** *filename \&optional visit beg end replace*

This function works like `insert-file-contents` except that it does not run `after-insert-file-functions`, and does not do format decoding, character code conversion, automatic uncompression, and so on.

If you want to pass a file name to another process so that another program can read the file, use the function `file-local-copy`; see [Magic File Names](Magic-File-Names.html).
