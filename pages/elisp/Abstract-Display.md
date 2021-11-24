

### 39.20 Abstract Display

The Ewoc package constructs buffer text that represents a structure of Lisp objects, and updates the text to follow changes in that structure. This is like the “view” component in the “model–view–controller” design paradigm. Ewoc means “Emacs’s Widget for Object Collections”.

An *ewoc* is a structure that organizes information required to construct buffer text that represents certain Lisp data. The buffer text of the ewoc has three parts, in order: first, fixed *header* text; next, textual descriptions of a series of data elements (Lisp objects that you specify); and last, fixed *footer* text. Specifically, an ewoc contains information on:

The buffer which its text is generated in.

The text’s start position in the buffer.

The header and footer strings.

A doubly-linked chain of *nodes*, each of which contains:

*   A *data element*, a single Lisp object.
*   Links to the preceding and following nodes in the chain.

A *pretty-printer* function which is responsible for inserting the textual representation of a data element value into the current buffer.

Typically, you define an ewoc with `ewoc-create`, and then pass the resulting ewoc structure to other functions in the Ewoc package to build nodes within it, and display it in the buffer. Once it is displayed in the buffer, other functions determine the correspondence between buffer positions and nodes, move point from one node’s textual representation to another, and so forth. See [Abstract Display Functions](Abstract-Display-Functions.html).

A node *encapsulates* a data element much the way a variable holds a value. Normally, encapsulation occurs as a part of adding a node to the ewoc. You can retrieve the data element value and place a new value in its place, like so:

```lisp
(ewoc-data node)
⇒ value

(ewoc-set-data node new-value)
⇒ new-value
```

You can also use, as the data element value, a Lisp object (list or vector) that is a container for the real value, or an index into some other structure. The example (see [Abstract Display Example](Abstract-Display-Example.html)) uses the latter approach.

When the data changes, you will want to update the text in the buffer. You can update all nodes by calling `ewoc-refresh`, or just specific nodes using `ewoc-invalidate`, or all nodes satisfying a predicate using `ewoc-map`. Alternatively, you can delete invalid nodes using `ewoc-delete` or `ewoc-filter`, and add new nodes in their place. Deleting a node from an ewoc deletes its associated textual description from buffer, as well.

|                                                                 |    |                                |
| :-------------------------------------------------------------- | -- | :----------------------------- |
| • [Abstract Display Functions](Abstract-Display-Functions.html) |    | Functions in the Ewoc package. |
| • [Abstract Display Example](Abstract-Display-Example.html)     |    | Example of using Ewoc.         |
