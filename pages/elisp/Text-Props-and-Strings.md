

#### 2.4.8.4 Text Properties in Strings

A string can hold properties for the characters it contains, in addition to the characters themselves. This enables programs that copy text between strings and buffers to copy the text’s properties with no special effort. See [Text Properties](Text-Properties.html), for an explanation of what text properties mean. Strings with text properties use a special read and print syntax:

```lisp
#("characters" property-data...)
```

where `property-data` consists of zero or more elements, in groups of three as follows:

```lisp
beg end plist
```

The elements `beg` and `end` are integers, and together specify a range of indices in the string; `plist` is the property list for that range. For example,

```lisp
#("foo bar" 0 3 (face bold) 3 4 nil 4 7 (face italic))
```

represents a string whose textual contents are ‘`foo bar`’, in which the first three characters have a `face` property with value `bold`, and the last three have a `face` property with value `italic`. (The fourth character has no text properties, so its property list is `nil`. It is not actually necessary to mention ranges with `nil` as the property list, since any characters not mentioned in any range will default to having no properties.)
