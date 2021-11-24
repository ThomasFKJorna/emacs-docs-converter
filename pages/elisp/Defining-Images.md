

#### 39.17.8 Defining Images

The functions `create-image`, `defimage` and `find-image` provide convenient ways to create image descriptors.

### Function: **create-image** *file-or-data \&optional type data-p \&rest props*

This function creates and returns an image descriptor which uses the data in `file-or-data`. `file-or-data` can be a file name or a string containing the image data; `data-p` should be `nil` for the former case, non-`nil` for the latter case.

The optional argument `type` is a symbol specifying the image type. If `type` is omitted or `nil`, `create-image` tries to determine the image type from the file’s first few bytes, or else from the file’s name.

The remaining arguments, `props`, specify additional image properties—for example,

```lisp
(create-image "foo.xpm" 'xpm nil :heuristic-mask t)
```

The function returns `nil` if images of this type are not supported. Otherwise it returns an image descriptor.

### Macro: **defimage** *symbol specs \&optional doc*

This macro defines `symbol` as an image name. The arguments `specs` is a list which specifies how to display the image. The third argument, `doc`, is an optional documentation string.

Each argument in `specs` has the form of a property list, and each one should specify at least the `:type` property and either the `:file` or the `:data` property. The value of `:type` should be a symbol specifying the image type, the value of `:file` is the file to load the image from, and the value of `:data` is a string containing the actual image data. Here is an example:

```lisp
(defimage test-image
  ((:type xpm :file "~/test1.xpm")
   (:type xbm :file "~/test1.xbm")))
```

`defimage` tests each argument, one by one, to see if it is usable—that is, if the type is supported and the file exists. The first usable argument is used to make an image descriptor which is stored in `symbol`.

If none of the alternatives will work, then `symbol` is defined as `nil`.

### Function: **image-property** *image property*

Return the value of `property` in `image`. Properties can be set by using `setf`. Setting a property to `nil` will remove the property from the image.

### Function: **find-image** *specs*

This function provides a convenient way to find an image satisfying one of a list of image specifications `specs`.

Each specification in `specs` is a property list with contents depending on image type. All specifications must at least contain the properties `:type type` and either `:file file` or `:data data`, where `type` is a symbol specifying the image type, e.g., `xbm`, `file` is the file to load the image from, and `data` is a string containing the actual image data. The first specification in the list whose `type` is supported, and `file` exists, is used to construct the image specification to be returned. If no specification is satisfied, `nil` is returned.

The image is looked for in `image-load-path`.

### User Option: **image-load-path**

This variable’s value is a list of locations in which to search for image files. If an element is a string or a variable symbol whose value is a string, the string is taken to be the name of a directory to search. If an element is a variable symbol whose value is a list, that is taken to be a list of directories to search.

The default is to search in the `images` subdirectory of the directory specified by `data-directory`, then the directory specified by `data-directory`, and finally in the directories in `load-path`. Subdirectories are not automatically included in the search, so if you put an image file in a subdirectory, you have to supply the subdirectory explicitly. For example, to find the image `images/foo/bar.xpm` within `data-directory`, you should specify the image as follows:

```lisp
(defimage foo-image '((:type xpm :file "foo/bar.xpm")))
```

### Function: **image-load-path-for-library** *library image \&optional path no-error*

This function returns a suitable search path for images used by the Lisp package `library`.

The function searches for `image` first using `image-load-path`, excluding `data-directory/images`, and then in `load-path`, followed by a path suitable for `library`, which includes `../../etc/images` and `../etc/images` relative to the library file itself, and finally in `data-directory/images`.

Then this function returns a list of directories which contains first the directory in which `image` was found, followed by the value of `load-path`. If `path` is given, it is used instead of `load-path`.

If `no-error` is non-`nil` and a suitable path can’t be found, don’t signal an error. Instead, return a list of directories as before, except that `nil` appears in place of the image directory.

Here is an example of using `image-load-path-for-library`:

```lisp
(defvar image-load-path) ; shush compiler
(let* ((load-path (image-load-path-for-library
                    "mh-e" "mh-logo.xpm"))
       (image-load-path (cons (car load-path)
                              image-load-path)))
  (mh-tool-bar-folder-buttons-init))
```

Images are automatically scaled when created based on the `image-scaling-factor` variable. The value is either a floating point number (where numbers higher than 1 means to increase the size and lower means to shrink the size), or the symbol `auto`, which will compute a scaling factor based on the font pixel size.
