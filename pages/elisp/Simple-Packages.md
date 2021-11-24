

### 41.2 Simple Packages

A simple package consists of a single Emacs Lisp source file. The file must conform to the Emacs Lisp library header conventions (see [Library Headers](Library-Headers.html)). The package’s attributes are taken from the various headers, as illustrated by the following example:

```lisp
;;; superfrobnicator.el --- Frobnicate and bifurcate flanges

;; Copyright (C) 2011 Free Software Foundation, Inc.
```

```lisp

;; Author: J. R. Hacker <jrh@example.com>
;; Version: 1.3
;; Package-Requires: ((flange "1.0"))
;; Keywords: multimedia, hypermedia
;; URL: https://example.com/jrhacker/superfrobnicate

…

;;; Commentary:

;; This package provides a minor mode to frobnicate and/or
;; bifurcate any flanges you desire.  To activate it, just type
…

;;;###autoload
(define-minor-mode superfrobnicator-mode
…
```

The name of the package is the same as the base name of the file, as written on the first line. Here, it is ‘`superfrobnicator`’.

The brief description is also taken from the first line. Here, it is ‘`Frobnicate and bifurcate flanges`’.

The version number comes from the ‘`Package-Version`’ header, if it exists, or from the ‘`Version`’ header otherwise. One or the other *must* be present. Here, the version number is 1.3.

If the file has a ‘`;;; Commentary:`’ section, this section is used as the long description. (When displaying the description, Emacs omits the ‘`;;; Commentary:`’ line, as well as the leading comment characters in the commentary itself.)

If the file has a ‘`Package-Requires`’ header, that is used as the package dependencies. In the above example, the package depends on the ‘`flange`’ package, version 1.0 or higher. See [Library Headers](Library-Headers.html), for a description of the ‘`Package-Requires`’ header. If the header is omitted, the package has no dependencies.

The ‘`Keywords`’ and ‘`URL`’ headers are optional, but recommended. The command `describe-package` uses these to add links to its output. The ‘`Keywords`’ header should contain at least one standard keyword from the `finder-known-keywords` list.

The file ought to also contain one or more autoload magic comments, as explained in [Packaging Basics](Packaging-Basics.html). In the above example, a magic comment autoloads `superfrobnicator-mode`.

See [Package Archives](Package-Archives.html), for an explanation of how to add a single-file package to a package archive.
