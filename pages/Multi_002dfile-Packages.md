

Next: [Package Archives](Package-Archives.html), Previous: [Simple Packages](Simple-Packages.html), Up: [Packaging](Packaging.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 41.3 Multi-file Packages

A multi-file package is less convenient to create than a single-file package, but it offers more features: it can include multiple Emacs Lisp files, an Info manual, and other file types (such as images).

Prior to installation, a multi-file package is stored in a package archive as a tar file. The tar file must be named `name-version.tar`, where `name` is the package name and `version` is the version number. Its contents, once extracted, must all appear in a directory named `name-version`, the *content directory* (see [Packaging Basics](Packaging-Basics.html)). Files may also extract into subdirectories of the content directory.

One of the files in the content directory must be named `name-pkg.el`. It must contain a single Lisp form, consisting of a call to the function `define-package`, described below. This defines the package’s attributes: version, brief description, and requirements.

For example, if we distribute version 1.3 of the superfrobnicator as a multi-file package, the tar file would be `superfrobnicator-1.3.tar`. Its contents would extract into the directory `superfrobnicator-1.3`, and one of these would be the file `superfrobnicator-pkg.el`.

*   Function: **define-package** *name version \&optional docstring requirements*

    This function defines a package. `name` is the package name, a string. `version` is the version, as a string of a form that can be understood by the function `version-to-list`. `docstring` is the brief description.

    `requirements` is a list of required packages and their versions. Each element in this list should have the form `(dep-name dep-version)`, where `dep-name` is a symbol whose name is the dependency’s package name, and `dep-version` is the dependency’s version (a string).

If the content directory contains a file named `README`, this file is used as the long description (overriding any ‘`;;; Commentary:`’ section).

If the content directory contains a file named `dir`, this is assumed to be an Info directory file made with `install-info`. See [Invoking install-info](https://www.gnu.org/software/texinfo/manual/texinfo/html_node/Invoking-install_002dinfo.html#Invoking-install_002dinfo) in Texinfo. The relevant Info files should also be present in the content directory. In this case, Emacs will automatically add the content directory to `Info-directory-list` when the package is activated.

Do not include any `.elc` files in the package. Those are created when the package is installed. Note that there is no way to control the order in which files are byte-compiled.

Do not include any file named `name-autoloads.el`. This file is reserved for the package’s autoload definitions (see [Packaging Basics](Packaging-Basics.html)). It is created automatically when the package is installed, by searching all the Lisp files in the package for autoload magic comments.

If the multi-file package contains auxiliary data files (such as images), the package’s Lisp code can refer to these files via the variable `load-file-name` (see [Loading](Loading.html)). Here is an example:

```lisp
(defconst superfrobnicator-base (file-name-directory load-file-name))

(defun superfrobnicator-fetch-image (file)
  (expand-file-name file superfrobnicator-base))
```

Next: [Package Archives](Package-Archives.html), Previous: [Simple Packages](Simple-Packages.html), Up: [Packaging](Packaging.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
