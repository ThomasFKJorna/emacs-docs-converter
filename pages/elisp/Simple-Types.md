

#### 15.4.1 Simple Types

This section describes all the simple customization types. For several of these customization types, the customization widget provides inline completion with `C-M-i` or `M-TAB`.

`sexp`

The value may be any Lisp object that can be printed and read back. You can use `sexp` as a fall-back for any option, if you don’t want to take the time to work out a more specific type to use.

`integer`

The value must be an integer.

`number`

The value must be a number (floating point or integer).

`float`

The value must be floating point.

`string`

The value must be a string. The customization buffer shows the string without delimiting ‘`"`’ characters or ‘`\`’ quotes.

`regexp`

Like `string` except that the string must be a valid regular expression.

`character`

The value must be a character code. A character code is actually an integer, but this type shows the value by inserting the character in the buffer, rather than by showing the number.

`file`

The value must be a file name. The widget provides completion.

### `(file :must-match t)`

The value must be a file name for an existing file. The widget provides completion.

`directory`

The value must be a directory. The widget provides completion.

`hook`

The value must be a list of functions. This customization type is used for hook variables. You can use the `:options` keyword in a hook variable’s `defcustom` to specify a list of functions recommended for use in the hook; See [Variable Definitions](Variable-Definitions.html).

`symbol`

The value must be a symbol. It appears in the customization buffer as the symbol name. The widget provides completion.

`function`

The value must be either a lambda expression or a function name. The widget provides completion for function names.

`variable`

The value must be a variable name. The widget provides completion.

`face`

The value must be a symbol which is a face name. The widget provides completion.

`boolean`

The value is boolean—either `nil` or `t`. Note that by using `choice` and `const` together (see the next section), you can specify that the value must be `nil` or `t`, but also specify the text to describe each value in a way that fits the specific meaning of the alternative.

`key-sequence`

The value is a key sequence. The customization buffer shows the key sequence using the same syntax as the `kbd` function. See [Key Sequences](Key-Sequences.html).

`coding-system`

The value must be a coding-system name, and you can do completion with `M-TAB`.

`color`

The value must be a valid color name. The widget provides completion for color names, as well as a sample and a button for selecting a color name from a list of color names shown in a `*Colors*` buffer.
