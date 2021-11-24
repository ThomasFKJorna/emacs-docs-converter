

### 5.9 Property Lists

A *property list* (*plist* for short) is a list of paired elements. Each of the pairs associates a property name (usually a symbol) with a property or value. Here is an example of a property list:

```lisp
(pine cones numbers (1 2 3) color "blue")
```

This property list associates `pine` with `cones`, `numbers` with `(1 2 3)`, and `color` with `"blue"`. The property names and values can be any Lisp objects, but the names are usually symbols (as they are in this example).

Property lists are used in several contexts. For instance, the function `put-text-property` takes an argument which is a property list, specifying text properties and associated values which are to be applied to text in a string or buffer. See [Text Properties](Text-Properties.html).

Another prominent use of property lists is for storing symbol properties. Every symbol possesses a list of properties, used to record miscellaneous information about the symbol; these properties are stored in the form of a property list. See [Symbol Properties](Symbol-Properties.html).

|                                               |    |                                                                       |
| :-------------------------------------------- | -- | :-------------------------------------------------------------------- |
| • [Plists and Alists](Plists-and-Alists.html) |    | Comparison of the advantages of property lists and association lists. |
| • [Plist Access](Plist-Access.html)           |    | Accessing property lists stored elsewhere.                            |
