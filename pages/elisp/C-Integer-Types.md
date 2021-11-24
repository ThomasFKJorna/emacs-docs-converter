

### E.10 C Integer Types

Here are some guidelines for use of integer types in the Emacs C source code. These guidelines sometimes give competing advice; common sense is advised.

Avoid arbitrary limits. For example, avoid `int len = strlen (s);` unless the length of `s` is required for other reasons to fit in `int` range.

### Do not assume that signed integer arithmetic wraps around on overflow. This is no longer true of Emacs porting targets: signed integer overflow has undefined behavior in practice, and can dump core or even cause earlier or later code to behave illogically. Unsigned overflow does wrap around reliably, modulo a power of two.

Prefer signed types to unsigned, as code gets confusing when signed and unsigned types are combined. Many other guidelines assume that types are signed; in the rarer cases where unsigned types are needed, similar advice may apply to the unsigned counterparts (e.g., `size_t` instead of `ptrdiff_t`, or `uintptr_t` instead of `intptr_t`).

Prefer `int` for Emacs character codes, in the range 0 .. 0x3FFFFF. More generally, prefer `int` for integers known to be in `int` range, e.g., screen column counts.

Prefer `ptrdiff_t` for sizes, i.e., for integers bounded by the maximum size of any individual C object or by the maximum number of elements in any C array. This is part of Emacs’s general preference for signed types. Using `ptrdiff_t` limits objects to `PTRDIFF_MAX` bytes, but larger objects would cause trouble anyway since they would break pointer subtraction, so this does not impose an arbitrary limit.

Avoid `ssize_t` except when communicating to low-level APIs that have `ssize_t`-related limitations. Although it’s equivalent to `ptrdiff_t` on typical platforms, `ssize_t` is occasionally narrower, so using it for size-related calculations could overflow. Also, `ptrdiff_t` is more ubiquitous and better-standardized, has standard `printf` formats, and is the basis for Emacs’s internal size-overflow checking. When using `ssize_t`, please note that POSIX requires support only for values in the range -1 .. `SSIZE_MAX`.

Normally, prefer `intptr_t` for internal representations of pointers, or for integers bounded only by the number of objects that can exist at any given time or by the total number of bytes that can be allocated. However, prefer `uintptr_t` to represent pointer arithmetic that could cross page boundaries. For example, on a machine with a 32-bit address space an array could cross the 0x7fffffff/0x80000000 boundary, which would cause an integer overflow when adding 1 to `(intptr_t) 0x7fffffff`.

Prefer the Emacs-defined type `EMACS_INT` for representing values converted to or from Emacs Lisp fixnums, as fixnum arithmetic is based on `EMACS_INT`.

When representing a system value (such as a file size or a count of seconds since the Epoch), prefer the corresponding system type (e.g., `off_t`, `time_t`). Do not assume that a system type is signed, unless this assumption is known to be safe. For example, although `off_t` is always signed, `time_t` need not be.

Prefer `intmax_t` for representing values that might be any signed integer value. A `printf`-family function can print such a value via a format like `"%"PRIdMAX`.

Prefer `bool`, `false` and `true` for booleans. Using `bool` can make programs easier to read and a bit faster than using `int`. Although it is also OK to use `int`, `0` and `1`, this older style is gradually being phased out. When using `bool`, respect the limitations of the replacement implementation of `bool`, as documented in the source file `lib/stdbool.in.h`. In particular, boolean bitfields should be of type `bool_bf`, not `bool`, so that they work correctly even when compiling Objective C with standard GCC.

In bitfields, prefer `unsigned int` or `signed int` to `int`, as `int` is less portable: it might be signed, and might not be. Single-bit bit fields should be `unsigned int` or `bool_bf` so that their values are 0 or 1.
