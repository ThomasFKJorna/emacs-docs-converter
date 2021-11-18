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

Up: [Parsing HTML/XML](Parsing-HTML_002fXML.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.28.1 Document Object Model

The DOM returned by `libxml-parse-html-region` (and the other XML parsing functions) is a tree structure where each node has a node name (called a *tag*), and optional key/value *attribute* list, and then a list of *child nodes*. The child nodes are either strings or DOM objects.

    (body ((width . "101"))
     (div ((class . "thing"))
      "Foo"
      (div nil
       "Yes")))

*   Function: **dom-node** *tag \&optional attributes \&rest children*

    This function creates a DOM node of type `tag`. If given, `attributes` should be a key/value pair list. If given, `children` should be DOM nodes.

The following functions can be used to work with this structure. Each function takes a DOM node, or a list of nodes. In the latter case, only the first node in the list is used.

Simple accessors:

*   `dom-tag node`

    Return the *tag* (also called “node name”) of the node.

*   `dom-attr node attribute`

    Return the value of `attribute` in the node. A common usage would be:

        (dom-attr img 'href)
        => "https://fsf.org/logo.png"

*   `dom-children node`

    Return all the children of the node.

*   `dom-non-text-children node`

    Return all the non-string children of the node.

*   `dom-attributes node`

    Return the key/value pair list of attributes of the node.

*   `dom-text node`

    Return all the textual elements of the node as a concatenated string.

*   `dom-texts node`

    Return all the textual elements of the node, as well as the textual elements of all the children of the node, recursively, as a concatenated string. This function also takes an optional separator to be inserted between the textual elements.

*   `dom-parent dom node`

    Return the parent of `node` in `dom`.

*   `dom-remove dom node`

    Remove `node` from `dom`.

The following are functions for altering the DOM.

*   `dom-set-attribute node attribute value`

    Set the `attribute` of the node to `value`.

*   `dom-append-child node child`

    Append `child` as the last child of `node`.

*   `dom-add-child-before node child before`

    Add `child` to `node`’s child list before the `before` node. If `before` is `nil`, make `child` the first child.

*   `dom-set-attributes node attributes`

    Replace all the attributes of the node with a new key/value list.

The following are functions for searching for elements in the DOM. They all return lists of matching nodes.

*   `dom-by-tag dom tag`

    Return all nodes in `dom` that are of type `tag`. A typical use would be:

        (dom-by-tag dom 'td)
        => '((td ...) (td ...) (td ...))

*   `dom-by-class dom match`

    Return all nodes in `dom` that have class names that match `match`, which is a regular expression.

*   `dom-by-style dom style`

    Return all nodes in `dom` that have styles that match `match`, which is a regular expression.

*   `dom-by-id dom style`

    Return all nodes in `dom` that have IDs that match `match`, which is a regular expression.

*   `dom-search dom predicate`

    Return all nodes in `dom` where `predicate` returns a non-`nil` value. `predicate` is called with the node to be tested as its parameter.

*   `dom-strings dom`

    Return all strings in `dom`.

Utility functions:

*   `dom-pp dom &optional remove-empty`

    Pretty-print `dom` at point. If `remove-empty`, don’t print textual nodes that just contain white-space.

Up: [Parsing HTML/XML](Parsing-HTML_002fXML.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
