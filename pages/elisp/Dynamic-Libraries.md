

### 40.21 Dynamically Loaded Libraries

A *dynamically loaded library* is a library that is loaded on demand, when its facilities are first needed. Emacs supports such on-demand loading of support libraries for some of its features.

### Variable: **dynamic-library-alist**

This is an alist of dynamic libraries and external library files implementing them.

Each element is a list of the form `(library files…)`, where the `car` is a symbol representing a supported external library, and the rest are strings giving alternate filenames for that library.

Emacs tries to load the library from the files in the order they appear in the list; if none is found, the Emacs session won’t have access to that library, and the features it provides will be unavailable.

Image support on some platforms uses this facility. Here’s an example of setting this variable for supporting images on MS-Windows:

```lisp
(setq dynamic-library-alist
      '((xpm "libxpm.dll" "xpm4.dll" "libXpm-nox4.dll")
        (png "libpng12d.dll" "libpng12.dll" "libpng.dll"
             "libpng13d.dll" "libpng13.dll")
        (jpeg "jpeg62.dll" "libjpeg.dll" "jpeg-62.dll"
              "jpeg.dll")
        (tiff "libtiff3.dll" "libtiff.dll")
        (gif "giflib4.dll" "libungif4.dll" "libungif.dll")
        (svg "librsvg-2-2.dll")
        (gdk-pixbuf "libgdk_pixbuf-2.0-0.dll")
        (glib "libglib-2.0-0.dll")
        (gobject "libgobject-2.0-0.dll")))
```

Note that image types `pbm` and `xbm` do not need entries in this variable because they do not depend on external libraries and are always available in Emacs.

Also note that this variable is not meant to be a generic facility for accessing external libraries; only those already known by Emacs can be loaded through it.

This variable is ignored if the given `library` is statically linked into Emacs.
