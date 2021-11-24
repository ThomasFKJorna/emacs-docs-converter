

### 17.7 Byte-Code Function Objects

Byte-compiled functions have a special data type: they are *byte-code function objects*. Whenever such an object appears as a function to be called, Emacs uses the byte-code interpreter to execute the byte-code.

Internally, a byte-code function object is much like a vector; its elements can be accessed using `aref`. Its printed representation is like that for a vector, with an additional ‘`#`’ before the opening ‘`[`’. It must have at least four elements; there is no maximum number, but only the first six elements have any normal use. They are:

`argdesc`

The descriptor of the arguments. This can either be a list of arguments, as described in [Argument List](Argument-List.html), or an integer encoding the required number of arguments. In the latter case, the value of the descriptor specifies the minimum number of arguments in the bits zero to 6, and the maximum number of arguments in bits 8 to 14. If the argument list uses `&rest`, then bit 7 is set; otherwise it’s cleared.

If `argdesc` is a list, the arguments will be dynamically bound before executing the byte code. If `argdesc` is an integer, the arguments will be instead pushed onto the stack of the byte-code interpreter, before executing the code.

`byte-code`

The string containing the byte-code instructions.

`constants`

The vector of Lisp objects referenced by the byte code. These include symbols used as function names and variable names.

`stacksize`

The maximum stack size this function needs.

`docstring`

The documentation string (if any); otherwise, `nil`. The value may be a number or a list, in case the documentation string is stored in a file. Use the function `documentation` to get the real documentation string (see [Accessing Documentation](Accessing-Documentation.html)).

`interactive`

The interactive spec (if any). This can be a string or a Lisp expression. It is `nil` for a function that isn’t interactive.

Here’s an example of a byte-code function object, in printed representation. It is the definition of the command `backward-sexp`.

```lisp
#[256
  "\211\204^G^@\300\262^A\301^A[!\207"
  [1 forward-sexp]
  3
  1793299
  "^p"]
```

The primitive way to create a byte-code object is with `make-byte-code`:

### Function: **make-byte-code** *\&rest elements*

This function constructs and returns a byte-code function object with `elements` as its elements.

You should not try to come up with the elements for a byte-code function yourself, because if they are inconsistent, Emacs may crash when you call the function. Always leave it to the byte compiler to create these objects; it makes the elements consistent (we hope).
