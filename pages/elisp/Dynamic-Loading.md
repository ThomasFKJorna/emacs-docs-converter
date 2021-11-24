

### 17.4 Dynamic Loading of Individual Functions

When you compile a file, you can optionally enable the *dynamic function loading* feature (also known as *lazy loading*). With dynamic function loading, loading the file doesn’t fully read the function definitions in the file. Instead, each function definition contains a place-holder which refers to the file. The first time each function is called, it reads the full definition from the file, to replace the place-holder.

The advantage of dynamic function loading is that loading the file should become faster. This is a good thing for a file which contains many separate user-callable functions, if using one of them does not imply you will probably also use the rest. A specialized mode which provides many keyboard commands often has that usage pattern: a user may invoke the mode, but use only a few of the commands it provides.

The dynamic loading feature has certain disadvantages:

If you delete or move the compiled file after loading it, Emacs can no longer load the remaining function definitions not already loaded.

If you alter the compiled file (such as by compiling a new version), then trying to load any function not already loaded will usually yield nonsense results.

These problems will never happen in normal circumstances with installed Emacs files. But they are quite likely to happen with Lisp files that you are changing. The easiest way to prevent these problems is to reload the new compiled file immediately after each recompilation.

*Experience shows that using dynamic function loading provides benefits that are hardly measurable, so this feature is deprecated since Emacs 27.1.*

The byte compiler uses the dynamic function loading feature if the variable `byte-compile-dynamic` is non-`nil` at compilation time. Do not set this variable globally, since dynamic loading is desirable only for certain files. Instead, enable the feature for specific source files with file-local variable bindings. For example, you could do it by writing this text in the source file’s first line:

```lisp
-*-byte-compile-dynamic: t;-*-
```

### Variable: **byte-compile-dynamic**

If this is non-`nil`, the byte compiler generates compiled files that are set up for dynamic function loading.

### Function: **fetch-bytecode** *function*

If `function` is a byte-code function object, this immediately finishes loading the byte code of `function` from its byte-compiled file, if it is not fully loaded already. Otherwise, it does nothing. It always returns `function`.
