

Next: [Simple Packages](Simple-Packages.html), Up: [Packaging](Packaging.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 41.1 Packaging Basics

A package is either a *simple package* or a *multi-file package*. A simple package is stored in a package archive as a single Emacs Lisp file, while a multi-file package is stored as a tar file (containing multiple Lisp files, and possibly non-Lisp files such as a manual).

In ordinary usage, the difference between simple packages and multi-file packages is relatively unimportant; the Package Menu interface makes no distinction between them. However, the procedure for creating them differs, as explained in the following sections.

Each package (whether simple or multi-file) has certain *attributes*:

*   Name

    A short word (e.g., ‘`auctex`’). This is usually also the symbol prefix used in the program (see [Coding Conventions](Coding-Conventions.html)).

*   Version

    A version number, in a form that the function `version-to-list` understands (e.g., ‘`11.86`’). Each release of a package should be accompanied by an increase in the version number so that it will be recognized as an upgrade by users querying the package archive.

*   Brief description

    This is shown when the package is listed in the Package Menu. It should occupy a single line, ideally in 36 characters or less.

*   Long description

    This is shown in the buffer created by `C-h P` (`describe-package`), following the package’s brief description and installation status. It normally spans multiple lines, and should fully describe the package’s capabilities and how to begin using it once it is installed.

*   Dependencies

    A list of other packages (possibly including minimal acceptable version numbers) on which this package depends. The list may be empty, meaning this package has no dependencies. Otherwise, installing this package also automatically installs its dependencies, recursively; if any dependency cannot be found, the package cannot be installed.

Installing a package, either via the command `package-install-file`, or via the Package Menu, creates a subdirectory of `package-user-dir` named `name-version`, where `name` is the package’s name and `version` its version (e.g., `~/.emacs.d/elpa/auctex-11.86/`). We call this the package’s *content directory*. It is where Emacs puts the package’s contents (the single Lisp file for a simple package, or the files extracted from a multi-file package).

Emacs then searches every Lisp file in the content directory for autoload magic comments (see [Autoload](Autoload.html)). These autoload definitions are saved to a file named `name-autoloads.el` in the content directory. They are typically used to autoload the principal user commands defined in the package, but they can also perform other tasks, such as adding an element to `auto-mode-alist` (see [Auto Major Mode](Auto-Major-Mode.html)). Note that a package typically does *not* autoload every function and variable defined within it—only the handful of commands typically called to begin using the package. Emacs then byte-compiles every Lisp file in the package.

After installation, the installed package is *loaded*: Emacs adds the package’s content directory to `load-path`, and evaluates the autoload definitions in `name-autoloads.el`.

Whenever Emacs starts up, it automatically calls the function `package-activate-all` to make installed packages available to the current session. This is done after loading the early init file, but before loading the regular init file (see [Startup Summary](Startup-Summary.html)). Packages are not automatically made available if the user option `package-enable-at-startup` is set to `nil` in the early init file.

*   Function: **package-activate-all**

    This function makes the packages available to the current session. The user option `package-load-list` specifies which packages to make available; by default, all installed packages are made available. See [Package Installation](https://www.gnu.org/software/emacs/manual/html_node/emacs/Package-Installation.html#Package-Installation) in The GNU Emacs Manual.

    In most cases, you should not need to call `package-activate-all`, as this is done automatically during startup. Simply make sure to put any code that should run before `package-activate-all` in the early init file, and any code that should run after it in the primary init file (see [Init File](https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-File.html#Init-File) in The GNU Emacs Manual).

<!---->

*   Command: **package-initialize** *\&optional no-activate*

    This function initializes Emacs’ internal record of which packages are installed, and then calls `package-activate-all`.

    The optional argument `no-activate`, if non-`nil`, causes Emacs to update its record of installed packages without actually making them available.

Next: [Simple Packages](Simple-Packages.html), Up: [Packaging](Packaging.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
