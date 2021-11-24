

### 7.2 Backward Compatibility

Code compiled with older versions of `cl-defstruct` that doesn’t use records may run into problems when used in a new Emacs. To alleviate this, Emacs detects when an old `cl-defstruct` is used, and enables a mode in which `type-of` handles old struct objects as if they were records.

### Function: **cl-old-struct-compat-mode** *arg*

If `arg` is positive, enable backward compatibility with old-style structs.
