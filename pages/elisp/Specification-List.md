

#### 18.2.15.2 Specification List

A *specification list* is required for an Edebug specification if some arguments of a macro call are evaluated while others are not. Some elements in a specification list match one or more arguments, but others modify the processing of all following elements. The latter, called *specification keywords*, are symbols beginning with ‘`&`’ (such as `&optional`).

A specification list may contain sublists, which match arguments that are themselves lists, or it may contain vectors used for grouping. Sublists and groups thus subdivide the specification list into a hierarchy of levels. Specification keywords apply only to the remainder of the sublist or group they are contained in.

When a specification list involves alternatives or repetition, matching it against an actual macro call may require backtracking. For more details, see [Backtracking](Backtracking.html).

Edebug specifications provide the power of regular expression matching, plus some context-free grammar constructs: the matching of sublists with balanced parentheses, recursive processing of forms, and recursion via indirect specifications.

Here’s a table of the possible elements of a specification list, with their meanings (see [Specification Examples](Specification-Examples.html), for the referenced examples):

`sexp`

A single unevaluated Lisp object, which is not instrumented.

`form`

A single evaluated expression, which is instrumented. If your macro wraps the expression with `lambda` before it is evaluated, use `def-form` instead. See `def-form` below.

`place`

A generalized variable. See [Generalized Variables](Generalized-Variables.html).

`body`

Short for `&rest form`. See `&rest` below. If your macro wraps its body of code with `lambda` before it is evaluated, use `def-body` instead. See `def-body` below.

`function-form`

A function form: either a quoted function symbol, a quoted lambda expression, or a form (that should evaluate to a function symbol or lambda expression). This is useful when an argument that’s a lambda expression might be quoted with `quote` rather than `function`, since it instruments the body of the lambda expression either way.

`lambda-expr`

A lambda expression with no quoting.

`&optional`

All following elements in the specification list are optional; as soon as one does not match, Edebug stops matching at this level.

To make just a few elements optional, followed by non-optional elements, use `[&optional specs…]`. To specify that several elements must all match or none, use `&optional [specs…]`. See the `defun` example.

`&rest`

All following elements in the specification list are repeated zero or more times. In the last repetition, however, it is not a problem if the expression runs out before matching all of the elements of the specification list.

To repeat only a few elements, use `[&rest specs…]`. To specify several elements that must all match on every repetition, use `&rest [specs…]`.

`&or`

Each of the following elements in the specification list is an alternative. One of the alternatives must match, or the `&or` specification fails.

Each list element following `&or` is a single alternative. To group two or more list elements as a single alternative, enclose them in `[…]`.

`&not`

Each of the following elements is matched as alternatives as if by using `&or`, but if any of them match, the specification fails. If none of them match, nothing is matched, but the `&not` specification succeeds.

`&define`

Indicates that the specification is for a defining form. Edebug’s definition of a defining form is a form containing one or more code forms which are saved and executed later, after the execution of the defining form.

The defining form itself is not instrumented (that is, Edebug does not stop before and after the defining form), but forms inside it typically will be instrumented. The `&define` keyword should be the first element in a list specification.

`nil`

This is successful when there are no more arguments to match at the current argument list level; otherwise it fails. See sublist specifications and the backquote example.

`gate`

No argument is matched but backtracking through the gate is disabled while matching the remainder of the specifications at this level. This is primarily used to generate more specific syntax error messages. See [Backtracking](Backtracking.html), for more details. Also see the `let` example.

`other-symbol`

Any other symbol in a specification list may be a predicate or an indirect specification.

If the symbol has an Edebug specification, this *indirect specification* should be either a list specification that is used in place of the symbol, or a function that is called to process the arguments. The specification may be defined with `def-edebug-spec` just as for macros. See the `defun` example.

Otherwise, the symbol should be a predicate. The predicate is called with the argument, and if the predicate returns `nil`, the specification fails and the argument is not instrumented.

Some suitable predicates include `symbolp`, `integerp`, `stringp`, `vectorp`, and `atom`.

`[elements…]`

A vector of elements groups the elements into a single *group specification*. Its meaning has nothing to do with vectors.

`"string"`

The argument should be a symbol named `string`. This specification is equivalent to the quoted symbol, `'symbol`, where the name of `symbol` is the `string`, but the string form is preferred.

`(vector elements…)`

The argument should be a vector whose elements must match the `elements` in the specification. See the backquote example.

`(elements…)`

Any other list is a *sublist specification* and the argument must be a list whose elements match the specification `elements`.

A sublist specification may be a dotted list and the corresponding list argument may then be a dotted list. Alternatively, the last CDR of a dotted list specification may be another sublist specification (via a grouping or an indirect specification, e.g., `(spec . [(more specs…)])`) whose elements match the non-dotted list arguments. This is useful in recursive specifications such as in the backquote example. Also see the description of a `nil` specification above for terminating such recursion.

Note that a sublist specification written as `(specs . nil)` is equivalent to `(specs)`, and `(specs . (sublist-elements…))` is equivalent to `(specs sublist-elements…)`.

Here is a list of additional specifications that may appear only after `&define`. See the `defun` example.

`name`

The argument, a symbol, is the name of the defining form.

A defining form is not required to have a name field; and it may have multiple name fields.

### `:name`

This construct does not actually match an argument. The element following `:name` should be a symbol; it is used as an additional name component for the definition. You can use this to add a unique, static component to the name of the definition. It may be used more than once.

`arg`

The argument, a symbol, is the name of an argument of the defining form. However, lambda-list keywords (symbols starting with ‘`&`’) are not allowed.

`lambda-list`

This matches a lambda list—the argument list of a lambda expression.

`def-body`

The argument is the body of code in a definition. This is like `body`, described above, but a definition body must be instrumented with a different Edebug call that looks up information associated with the definition. Use `def-body` for the highest level list of forms within the definition.

`def-form`

The argument is a single, highest-level form in a definition. This is like `def-body`, except it is used to match a single form rather than a list of forms. As a special case, `def-form` also means that tracing information is not output when the form is executed. See the `interactive` example.
