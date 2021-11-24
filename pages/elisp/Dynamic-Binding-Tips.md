

#### 12.10.2 Proper Use of Dynamic Binding

Dynamic binding is a powerful feature, as it allows programs to refer to variables that are not defined within their local textual scope. However, if used without restraint, this can also make programs hard to understand. There are two clean ways to use this technique:

If a variable has no global definition, use it as a local variable only within a binding construct, such as the body of the `let` form where the variable was bound. If this convention is followed consistently throughout a program, the value of the variable will not affect, nor be affected by, any uses of the same variable symbol elsewhere in the program.

Otherwise, define the variable with `defvar`, `defconst` (see [Defining Variables](Defining-Variables.html)), or `defcustom` (see [Variable Definitions](Variable-Definitions.html)). Usually, the definition should be at top-level in an Emacs Lisp file. As far as possible, it should include a documentation string which explains the meaning and purpose of the variable. You should also choose the variable’s name to avoid name conflicts (see [Coding Conventions](Coding-Conventions.html)).

Then you can bind the variable anywhere in a program, knowing reliably what the effect will be. Wherever you encounter the variable, it will be easy to refer back to the definition, e.g., via the `C-h v` command (provided the variable definition has been loaded into Emacs). See [Name Help](https://www.gnu.org/software/emacs/manual/html_node/emacs/Name-Help.html#Name-Help) in The GNU Emacs Manual.

For example, it is common to use local bindings for customizable variables like `case-fold-search`:

```lisp
(defun search-for-abc ()
  "Search for the string \"abc\", ignoring case differences."
  (let ((case-fold-search t))
    (re-search-forward "abc")))
```
