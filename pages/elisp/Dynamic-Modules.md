

### 16.11 Emacs Dynamic Modules

A *dynamic Emacs module* is a shared library that provides additional functionality for use in Emacs Lisp programs, just like a package written in Emacs Lisp would.

Functions that load Emacs Lisp packages can also load dynamic modules. They recognize dynamic modules by looking at their file-name extension, a.k.a. “suffix”. This suffix is platform-dependent.

### Variable: **module-file-suffix**

This variable holds the system-dependent value of the file-name extension of the module files. Its value is `.so` on POSIX hosts and `.dll` on MS-Windows.

Every dynamic module should export a C-callable function named `emacs_module_init`, which Emacs will call as part of the call to `load` or `require` which loads the module. It should also export a symbol named `plugin_is_GPL_compatible` to indicate that its code is released under the GPL or compatible license; Emacs will signal an error if your program tries to load modules that don’t export such a symbol.

If a module needs to call Emacs functions, it should do so through the API (Application Programming Interface) defined and documented in the header file `emacs-module.h` that is part of the Emacs distribution. See [Writing Dynamic Modules](Writing-Dynamic-Modules.html), for details of using that API when writing your own modules.

Modules can create `user-ptr` Lisp objects that embed pointers to C struct’s defined by the module. This is useful for keeping around complex data structures created by a module, to be passed back to the module’s functions. User-ptr objects can also have associated *finalizers* – functions to be run when the object is GC’ed; this is useful for freeing any resources allocated for the underlying data structure, such as memory, open file descriptors, etc. See [Module Values](Module-Values.html).

### Function: **user-ptrp** *object*

This function returns `t` if its argument is a `user-ptr` object.

### Function: **module-load** *file*

Emacs calls this low-level primitive to load a module from the specified `file` and perform the necessary initialization of the module. This is the primitive which makes sure the module exports the `plugin_is_GPL_compatible` symbol, calls the module’s `emacs_module_init` function, and signals an error if that function returns an error indication, or if the use typed `C-g` during the initialization. If the initialization succeeds, `module-load` returns `t`. Note that `file` must already have the proper file-name extension, as this function doesn’t try looking for files with known extensions, unlike `load`.

Unlike `load`, `module-load` doesn’t record the module in `load-history`, doesn’t print any messages, and doesn’t protect against recursive loads. Most users should therefore use `load`, `load-file`, `load-library`, or `require` instead of `module-load`.

Loadable modules in Emacs are enabled by using the `--with-modules` option at configure time.

***
