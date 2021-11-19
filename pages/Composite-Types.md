

Next: [Splicing into Lists](Splicing-into-Lists.html), Previous: [Simple Types](Simple-Types.html), Up: [Customization Types](Customization-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 15.4.2 Composite Types

When none of the simple types is appropriate, you can use composite types, which build new types from other types or from specified data. The specified types or data are called the *arguments* of the composite type. The composite type normally looks like this:

```lisp
(constructor arguments…)
```

but you can also add keyword-value pairs before the arguments, like this:

```lisp
(constructor {keyword value}… arguments…)
```

Here is a table of constructors and how to use them to write composite types:

*   `(cons car-type cdr-type)`

    The value must be a cons cell, its CAR must fit `car-type`, and its CDR must fit `cdr-type`. For example, `(cons string symbol)` is a customization type which matches values such as `("foo" . foo)`.

    In the customization buffer, the CAR and CDR are displayed and edited separately, each according to their specified type.

*   `(list element-types…)`

    The value must be a list with exactly as many elements as the `element-types` given; and each element must fit the corresponding `element-type`.

    For example, `(list integer string function)` describes a list of three elements; the first element must be an integer, the second a string, and the third a function.

    In the customization buffer, each element is displayed and edited separately, according to the type specified for it.

*   `(group element-types…)`

    This works like `list` except for the formatting of text in the Custom buffer. `list` labels each element value with its tag; `group` does not.

*   `(vector element-types…)`

    Like `list` except that the value must be a vector instead of a list. The elements work the same as in `list`.

*   `(alist :key-type key-type :value-type value-type)`

    The value must be a list of cons-cells, the CAR of each cell representing a key of customization type `key-type`, and the CDR of the same cell representing a value of customization type `value-type`. The user can add and delete key/value pairs, and edit both the key and the value of each pair.

    If omitted, `key-type` and `value-type` default to `sexp`.

    The user can add any key matching the specified key type, but you can give some keys a preferential treatment by specifying them with the `:options` (see [Variable Definitions](Variable-Definitions.html)). The specified keys will always be shown in the customize buffer (together with a suitable value), with a checkbox to include or exclude or disable the key/value pair from the alist. The user will not be able to edit the keys specified by the `:options` keyword argument.

    The argument to the `:options` keywords should be a list of specifications for reasonable keys in the alist. Ordinarily, they are simply atoms, which stand for themselves. For example:

    ```lisp
    :options '("foo" "bar" "baz")
    ```

    specifies that there are three known keys, namely `"foo"`, `"bar"` and `"baz"`, which will always be shown first.

    You may want to restrict the value type for specific keys, for example, the value associated with the `"bar"` key can only be an integer. You can specify this by using a list instead of an atom in the list. The first element will specify the key, like before, while the second element will specify the value type. For example:

    ```lisp
    :options '("foo" ("bar" integer) "baz")
    ```

    Finally, you may want to change how the key is presented. By default, the key is simply shown as a `const`, since the user cannot change the special keys specified with the `:options` keyword. However, you may want to use a more specialized type for presenting the key, like `function-item` if you know it is a symbol with a function binding. This is done by using a customization type specification instead of a symbol for the key.

    ```lisp
    :options '("foo"
               ((function-item some-function) integer)
               "baz")
    ```

    Many alists use lists with two elements, instead of cons cells. For example,

    ```lisp
    (defcustom list-alist
      '(("foo" 1) ("bar" 2) ("baz" 3))
      "Each element is a list of the form (KEY VALUE).")
    ```

    instead of

    ```lisp
    (defcustom cons-alist
      '(("foo" . 1) ("bar" . 2) ("baz" . 3))
      "Each element is a cons-cell (KEY . VALUE).")
    ```

    Because of the way lists are implemented on top of cons cells, you can treat `list-alist` in the example above as a cons cell alist, where the value type is a list with a single element containing the real value.

    ```lisp
    (defcustom list-alist '(("foo" 1) ("bar" 2) ("baz" 3))
      "Each element is a list of the form (KEY VALUE)."
      :type '(alist :value-type (group integer)))
    ```

    The `group` widget is used here instead of `list` only because the formatting is better suited for the purpose.

    Similarly, you can have alists with more values associated with each key, using variations of this trick:

    ```lisp
    (defcustom person-data '(("brian"  50 t)
                             ("dorith" 55 nil)
                             ("ken"    52 t))
      "Alist of basic info about people.
    Each element has the form (NAME AGE MALE-FLAG)."
      :type '(alist :value-type (group integer boolean)))
    ```

*   `(plist :key-type key-type :value-type value-type)`

    This customization type is similar to `alist` (see above), except that (i) the information is stored as a property list, (see [Property Lists](Property-Lists.html)), and (ii) `key-type`, if omitted, defaults to `symbol` rather than `sexp`.

*   `(choice alternative-types…)`

    The value must fit one of `alternative-types`. For example, `(choice integer string)` allows either an integer or a string.

    In the customization buffer, the user selects an alternative using a menu, and can then edit the value in the usual way for that alternative.

    Normally the strings in this menu are determined automatically from the choices; however, you can specify different strings for the menu by including the `:tag` keyword in the alternatives. For example, if an integer stands for a number of spaces, while a string is text to use verbatim, you might write the customization type this way,

    ```lisp
    (choice (integer :tag "Number of spaces")
            (string :tag "Literal text"))
    ```

    so that the menu offers ‘`Number of spaces`’ and ‘`Literal text`’.

    In any alternative for which `nil` is not a valid value, other than a `const`, you should specify a valid default for that alternative using the `:value` keyword. See [Type Keywords](Type-Keywords.html).

    If some values are covered by more than one of the alternatives, customize will choose the first alternative that the value fits. This means you should always list the most specific types first, and the most general last. Here’s an example of proper usage:

    ```lisp
    (choice (const :tag "Off" nil)
            symbol (sexp :tag "Other"))
    ```

    This way, the special value `nil` is not treated like other symbols, and symbols are not treated like other Lisp expressions.

*   `(radio element-types…)`

    This is similar to `choice`, except that the choices are displayed using radio buttons rather than a menu. This has the advantage of displaying documentation for the choices when applicable and so is often a good choice for a choice between constant functions (`function-item` customization types).

*   `(const value)`

    The value must be `value`—nothing else is allowed.

    The main use of `const` is inside of `choice`. For example, `(choice integer (const nil))` allows either an integer or `nil`.

    `:tag` is often used with `const`, inside of `choice`. For example,

    ```lisp
    (choice (const :tag "Yes" t)
            (const :tag "No" nil)
            (const :tag "Ask" foo))
    ```

    describes a variable for which `t` means yes, `nil` means no, and `foo` means “ask”.

*   `(other value)`

    This alternative can match any Lisp value, but if the user chooses this alternative, that selects the value `value`.

    The main use of `other` is as the last element of `choice`. For example,

    ```lisp
    (choice (const :tag "Yes" t)
            (const :tag "No" nil)
            (other :tag "Ask" foo))
    ```

    describes a variable for which `t` means yes, `nil` means no, and anything else means “ask”. If the user chooses ‘`Ask`’ from the menu of alternatives, that specifies the value `foo`; but any other value (not `t`, `nil` or `foo`) displays as ‘`Ask`’, just like `foo`.

*   `(function-item function)`

    Like `const`, but used for values which are functions. This displays the documentation string as well as the function name. The documentation string is either the one you specify with `:doc`, or `function`’s own documentation string.

*   `(variable-item variable)`

    Like `const`, but used for values which are variable names. This displays the documentation string as well as the variable name. The documentation string is either the one you specify with `:doc`, or `variable`’s own documentation string.

*   `(set types…)`

    The value must be a list, and each element of the list must match one of the `types` specified.

    This appears in the customization buffer as a checklist, so that each of `types` may have either one corresponding element or none. It is not possible to specify two different elements that match the same one of `types`. For example, `(set integer symbol)` allows one integer and/or one symbol in the list; it does not allow multiple integers or multiple symbols. As a result, it is rare to use nonspecific types such as `integer` in a `set`.

    Most often, the `types` in a `set` are `const` types, as shown here:

    ```lisp
    (set (const :bold) (const :italic))
    ```

    Sometimes they describe possible elements in an alist:

    ```lisp
    (set (cons :tag "Height" (const height) integer)
         (cons :tag "Width" (const width) integer))
    ```

    That lets the user specify a height value optionally and a width value optionally.

*   `(repeat element-type)`

    The value must be a list and each element of the list must fit the type `element-type`. This appears in the customization buffer as a list of elements, with ‘`[INS]`’ and ‘`[DEL]`’ buttons for adding more elements or removing elements.

*   `(restricted-sexp :match-alternatives criteria)`

    This is the most general composite type construct. The value may be any Lisp object that satisfies one of `criteria`. `criteria` should be a list, and each element should be one of these possibilities:

    *   A predicate—that is, a function of one argument that returns either `nil` or non-`nil` according to the argument. Using a predicate in the list says that objects for which the predicate returns non-`nil` are acceptable.
    *   A quoted constant—that is, `'object`. This sort of element in the list says that `object` itself is an acceptable value.

    For example,

    ```lisp
    (restricted-sexp :match-alternatives
                     (integerp 't 'nil))
    ```

    allows integers, `t` and `nil` as legitimate values.

    The customization buffer shows all legitimate values using their read syntax, and the user edits them textually.

Here is a table of the keywords you can use in keyword-value pairs in a composite type:

*   `:tag tag`

    Use `tag` as the name of this alternative, for user communication purposes. This is useful for a type that appears inside of a `choice`.

*   `:match-alternatives criteria`

    Use `criteria` to match possible values. This is used only in `restricted-sexp`.

*   `:args argument-list`

    Use the elements of `argument-list` as the arguments of the type construct. For instance, `(const :args (foo))` is equivalent to `(const foo)`. You rarely need to write `:args` explicitly, because normally the arguments are recognized automatically as whatever follows the last keyword-value pair.

Next: [Splicing into Lists](Splicing-into-Lists.html), Previous: [Simple Types](Simple-Types.html), Up: [Customization Types](Customization-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
