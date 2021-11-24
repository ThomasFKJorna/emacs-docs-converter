

### 15.5 Applying Customizations

The following functions are responsible for installing the user’s customization settings for variables and faces, respectively. When the user invokes ‘`Save for future sessions`’ in the Customize interface, that takes effect by writing a `custom-set-variables` and/or a `custom-set-faces` form into the custom file, to be evaluated the next time Emacs starts.

### Function: **custom-set-variables** *\&rest args*

This function installs the variable customizations specified by `args`. Each argument in `args` should have the form

```lisp
(var expression [now [request [comment]]])
```

`var` is a variable name (a symbol), and `expression` is an expression which evaluates to the desired customized value.

If the `defcustom` form for `var` has been evaluated prior to this `custom-set-variables` call, `expression` is immediately evaluated, and the variable’s value is set to the result. Otherwise, `expression` is stored into the variable’s `saved-value` property, to be evaluated when the relevant `defcustom` is called (usually when the library defining that variable is loaded into Emacs).

The `now`, `request`, and `comment` entries are for internal use only, and may be omitted. `now`, if non-`nil`, means to set the variable’s value now, even if the variable’s `defcustom` form has not been evaluated. `request` is a list of features to be loaded immediately (see [Named Features](Named-Features.html)). `comment` is a string describing the customization.

### Function: **custom-set-faces** *\&rest args*

This function installs the face customizations specified by `args`. Each argument in `args` should have the form

```lisp
(face spec [now [comment]])
```

`face` is a face name (a symbol), and `spec` is the customized face specification for that face (see [Defining Faces](Defining-Faces.html)).

The `now` and `comment` entries are for internal use only, and may be omitted. `now`, if non-`nil`, means to install the face specification now, even if the `defface` form has not been evaluated. `comment` is a string describing the customization.
