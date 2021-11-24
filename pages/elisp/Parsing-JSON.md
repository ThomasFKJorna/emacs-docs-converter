

### 32.29 Parsing and generating JSON values

When Emacs is compiled with JSON (*JavaScript Object Notation*) support, it provides several functions to convert between Lisp objects and JSON values. Any JSON value can be converted to a Lisp object, but not vice versa. Specifically:

### JSON uses three keywords: `true`, `null`, `false`. `true` is represented by the symbol `t`. By default, the remaining two are represented, respectively, by the symbols `:null` and `:false`.

JSON only has floating-point numbers. They can represent both Lisp integers and Lisp floating-point numbers.

JSON strings are always Unicode strings encoded in UTF-8. Lisp strings can contain non-Unicode characters.

JSON has only one sequence type, the array. JSON arrays are represented using Lisp vectors.

JSON has only one map type, the object. JSON objects are represented using Lisp hashtables, alists or plists. When an alist or plist contains several elements with the same key, Emacs uses only the first element for serialization, in accordance with the behavior of `assq`.

Note that `nil`, being both a valid alist and a valid plist, represents `{}`, the empty JSON object; not `null`, `false`, or an empty array, all of which are different JSON values.

If some Lisp object can’t be represented in JSON, the serialization functions will signal an error of type `wrong-type-argument`. The parsing functions can also signal the following errors:

`json-end-of-file`

Signaled when encountering a premature end of the input text.

`json-trailing-content`

Signaled when encountering unexpected input after the first JSON object parsed.

`json-parse-error`

Signaled when encountering invalid JSON syntax.

Only top-level values (arrays and objects) can be serialized to JSON. The subobjects within these top-level values can be of any type. Likewise, the parsing functions will only return vectors, hashtables, alists, and plists.

### Function: **json-serialize** *object \&rest args*

This function returns a new Lisp string which contains the JSON representation of `object`. The argument `args` is a list of keyword/argument pairs. The following keywords are accepted:

*   `:null-object`

    The value decides which Lisp object to use to represent the JSON keyword `null`. It defaults to the symbol `:null`.

*   `:false-object`

    The value decides which Lisp object to use to represent the JSON keyword `false`. It defaults to the symbol `:false`.

### Function: **json-insert** *object \&rest args*

This function inserts the JSON representation of `object` into the current buffer before point. The argument `args` are interpreted as in `json-parse-string`.

### Function: **json-parse-string** *string \&rest args*

This function parses the JSON value in `string`, which must be a Lisp string. If `string` doesn’t contain a valid JSON object, this function signals the `json-parse-error` error.

The argument `args` is a list of keyword/argument pairs. The following keywords are accepted:

*   `:object-type`

    The value decides which Lisp object to use for representing the key-value mappings of a JSON object. It can be either `hash-table`, the default, to make hashtables with strings as keys; `alist` to use alists with symbols as keys; or `plist` to use plists with keyword symbols as keys.

*   `:array-type`

    The value decides which Lisp object to use for representing a JSON array. It can be either `array`, the default, to use Lisp arrays; or `list` to use lists.

*   `:null-object`

    The value decides which Lisp object to use to represent the JSON keyword `null`. It defaults to the symbol `:null`.

*   `:false-object`

    The value decides which Lisp object to use to represent the JSON keyword `false`. It defaults to the symbol `:false`.

### Function: **json-parse-buffer** *\&rest args*

This function reads the next JSON value from the current buffer, starting at point. It moves point to the position immediately after the value if contains a valid JSON object; otherwise it signals the `json-parse-error` error and doesn’t move point. The arguments `args` are interpreted as in `json-parse-string`.
