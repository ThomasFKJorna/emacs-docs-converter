<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Function Cells](Function-Cells.html), Previous: [Anonymous Functions](Anonymous-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.8 Generic Functions

Functions defined using `defun` have a hard-coded set of assumptions about the types and expected values of their arguments. For example, a function that was designed to handle values of its argument that are either numbers or lists of numbers will fail or signal an error if called with a value of any other type, such as a vector or a string. This happens because the implementation of the function is not prepared to deal with types other than those assumed during the design.

By contrast, object-oriented programs use *polymorphic functions*: a set of specialized functions having the same name, each one of which was written for a certain specific set of argument types. Which of the functions is actually called is decided at run time based on the types of the actual arguments.

Emacs provides support for polymorphism. Like other Lisp environments, notably Common Lisp and its Common Lisp Object System (CLOS), this support is based on *generic functions*. The Emacs generic functions closely follow CLOS, including use of similar names, so if you have experience with CLOS, the rest of this section will sound very familiar.

A generic function specifies an abstract operation, by defining its name and list of arguments, but (usually) no implementation. The actual implementation for several specific classes of arguments is provided by *methods*, which should be defined separately. Each method that implements a generic function has the same name as the generic function, but the method’s definition indicates what kinds of arguments it can handle by *specializing* the arguments defined by the generic function. These *argument specializers* can be more or less specific; for example, a `string` type is more specific than a more general type, such as `sequence`.

Note that, unlike in message-based OO languages, such as C`++` and Simula, methods that implement generic functions don’t belong to a class, they belong to the generic function they implement.

When a generic function is invoked, it selects the applicable methods by comparing the actual arguments passed by the caller with the argument specializers of each method. A method is applicable if the actual arguments of the call are compatible with the method’s specializers. If more than one method is applicable, they are combined using certain rules, described below, and the combination then handles the call.

*   Macro: **cl-defgeneric** *name arguments \[documentation] \[options-and-methods…] \&rest body*

    This macro defines a generic function with the specified `name` and `arguments`. If `body` is present, it provides the default implementation. If `documentation` is present (it should always be), it specifies the documentation string for the generic function, in the form `(:documentation docstring)`. The optional `options-and-methods` can be one of the following forms:

    *   `(declare declarations)`

        A declare form, as described in [Declare Form](Declare-Form.html).

    *   `(:argument-precedence-order &rest args)`

        This form affects the sorting order for combining applicable methods. Normally, when two methods are compared during combination, method arguments are examined left to right, and the first method whose argument specializer is more specific will come before the other one. The order defined by this form overrides that, and the arguments are examined according to their order in this form, and not left to right.

    *   `(:method [qualifiers…] args &rest body)`

        This form defines a method like `cl-defmethod` does.

<!---->

*   Macro: **cl-defmethod** *name \[qualifier] arguments \[\&context (expr spec)…] \&rest \[docstring] body*

    This macro defines a particular implementation for the generic function called `name`. The implementation code is given by `body`. If present, `docstring` is the documentation string for the method. The `arguments` list, which must be identical in all the methods that implement a generic function, and must match the argument list of that function, provides argument specializers of the form `(arg spec)`, where `arg` is the argument name as specified in the `cl-defgeneric` call, and `spec` is one of the following specializer forms:

    *   `type`

        This specializer requires the argument to be of the given `type`, one of the types from the type hierarchy described below.

    *   `(eql object)`

        This specializer requires the argument be `eql` to the given `object`.

    *   `(head object)`

        The argument must be a cons cell whose `car` is `eql` to `object`.

    *   `struct-type`

        The argument must be an instance of a class named `struct-type` defined with `cl-defstruct` (see [Structures](https://www.gnu.org/software/emacs/manual/html_node/cl/Structures.html#Structures) in Common Lisp Extensions for GNU Emacs Lisp), or of one of its child classes.

    Method definitions can make use of a new argument-list keyword, `&context`, which introduces extra specializers that test the environment at the time the method is run. This keyword should appear after the list of required arguments, but before any `&rest` or `&optional` keywords. The `&context` specializers look much like regular argument specializers—(`expr` `spec`)—except that `expr` is an expression to be evaluated in the current context, and the `spec` is a value to compare against. For example, `&context (overwrite-mode (eql t))` will make the method applicable only when `overwrite-mode` is turned on. The `&context` keyword can be followed by any number of context specializers. Because the context specializers are not part of the generic function’s argument signature, they may be omitted in methods that don’t require them.

    The type specializer, `(arg type)`, can specify one of the *system types* in the following list. When a parent type is specified, an argument whose type is any of its more specific child types, as well as grand-children, grand-grand-children, etc. will also be compatible.

    *   `integer`

        Parent type: `number`.

    *   *   `number`
        *   `null`

        Parent type: `symbol`

    *   *   `symbol`
        *   `string`

        Parent type: `array`.

    *   `array`

        Parent type: `sequence`.

    *   `cons`

        Parent type: `list`.

    *   `list`

        Parent type: `sequence`.

    *   *   `marker`
        *   `overlay`
        *   `float`

        Parent type: `number`.

    *   *   `window-configuration`
        *   `process`
        *   `window`
        *   `subr`
        *   `compiled-function`
        *   `buffer`
        *   `char-table`

        Parent type: `array`.

    *   `bool-vector`

        Parent type: `array`.

    *   `vector`

        Parent type: `array`.

    *   *   `frame`
        *   `hash-table`
        *   `font-spec`
        *   `font-entity`
        *   `font-object`

    The optional `qualifier` allows combining several applicable methods. If it is not present, the defined method is a *primary* method, responsible for providing the primary implementation of the generic function for the specialized arguments. You can also define *auxiliary methods*, by using one of the following values as `qualifier`:

    *   `:before`

        This auxiliary method will run before the primary method. More accurately, all the `:before` methods will run before the primary, in the most-specific-first order.

    *   `:after`

        This auxiliary method will run after the primary method. More accurately, all such methods will run after the primary, in the most-specific-last order.

    *   `:around`

        This auxiliary method will run *instead* of the primary method. The most specific of such methods will be run before any other method. Such methods normally use `cl-call-next-method`, described below, to invoke the other auxiliary or primary methods.

    *   `:extra string`

        This allows you to add more methods, distinguished by `string`, for the same specializers and qualifiers.

    Functions defined using `cl-defmethod` cannot be made interactive, i.e. commands (see [Defining Commands](Defining-Commands.html)), by adding the `interactive` form to them. If you need a polymorphic command, we recommend defining a normal command that calls a polymorphic function defined via `cl-defgeneric` and `cl-defmethod`.

Each time a generic function is called, it builds the *effective method* which will handle this invocation by combining the applicable methods defined for the function. The process of finding the applicable methods and producing the effective method is called *dispatch*. The applicable methods are those all of whose specializers are compatible with the actual arguments of the call. Since all of the arguments must be compatible with the specializers, they all determine whether a method is applicable. Methods that explicitly specialize more than one argument are called *multiple-dispatch methods*.

The applicable methods are sorted into the order in which they will be combined. The method whose left-most argument specializer is the most specific one will come first in the order. (Specifying `:argument-precedence-order` as part of `cl-defmethod` overrides that, as described above.) If the method body calls `cl-call-next-method`, the next most-specific method will run. If there are applicable `:around` methods, the most-specific of them will run first; it should call `cl-call-next-method` to run any of the less specific `:around` methods. Next, the `:before` methods run in the order of their specificity, followed by the primary method, and lastly the `:after` methods in the reverse order of their specificity.

*   Function: **cl-call-next-method** *\&rest args*

    When invoked from within the lexical body of a primary or an `:around` auxiliary method, call the next applicable method for the same generic function. Normally, it is called with no arguments, which means to call the next applicable method with the same arguments that the calling method was invoked. Otherwise, the specified arguments are used instead.

<!---->

*   Function: **cl-next-method-p**

    This function, when called from within the lexical body of a primary or an `:around` auxiliary method, returns non-`nil` if there is a next method to call.

Next: [Function Cells](Function-Cells.html), Previous: [Anonymous Functions](Anonymous-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
