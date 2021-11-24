

#### 16.5.2 When to Use Autoload

Do not add an autoload comment unless it is really necessary. Autoloading code means it is always globally visible. Once an item is autoloaded, there is no compatible way to transition back to it not being autoloaded (after people become accustomed to being able to use it without an explicit load).

The most common items to autoload are the interactive entry points to a library. For example, if `python.el` is a library defining a major-mode for editing Python code, autoload the definition of the `python-mode` function, so that people can simply use `M-x python-mode` to load the library.

Variables usually don’t need to be autoloaded. An exception is if the variable on its own is generally useful without the whole defining library being loaded. (An example of this might be something like `find-exec-terminator`.)

Don’t autoload a user option just so that a user can set it.

Never add an autoload *comment* to silence a compiler warning in another file. In the file that produces the warning, use `(defvar foo)` to silence an undefined variable warning, and `declare-function` (see [Declaring Functions](Declaring-Functions.html)) to silence an undefined function warning; or require the relevant library; or use an explicit autoload *statement*.
