

#### 39.19.3 Making Buttons

Buttons are associated with a region of text, using an overlay or text properties to hold button-specific information, all of which are initialized from the button’s type (which defaults to the built-in button type `button`). Like all Emacs text, the appearance of the button is governed by the `face` property; by default (via the `face` property inherited from the `button` button-type) this is a simple underline, like a typical web-page link.

For convenience, there are two sorts of button-creation functions, those that add button properties to an existing region of a buffer, called `make-...button`, and those that also insert the button text, called `insert-...button`.

The button-creation functions all take the `&rest` argument `properties`, which should be a sequence of `property value` pairs, specifying properties to add to the button; see [Button Properties](Button-Properties.html). In addition, the keyword argument `:type` may be used to specify a button-type from which to inherit other properties; see [Button Types](Button-Types.html). Any properties not explicitly specified during creation will be inherited from the button’s type (if the type defines such a property).

The following functions add a button using an overlay (see [Overlays](Overlays.html)) to hold the button properties:

### Function: **make-button** *beg end \&rest properties*

This makes a button from `beg` to `end` in the current buffer, and returns it.

### Function: **insert-button** *label \&rest properties*

This insert a button with the label `label` at point, and returns it.

The following functions are similar, but using text properties (see [Text Properties](Text-Properties.html)) to hold the button properties. Such buttons do not add markers to the buffer, so editing in the buffer does not slow down if there is an extremely large numbers of buttons. However, if there is an existing face text property on the text (e.g., a face assigned by Font Lock mode), the button face may not be visible. Both of these functions return the starting position of the new button.

### Function: **make-text-button** *beg end \&rest properties*

This makes a button from `beg` to `end` in the current buffer, using text properties.

### Function: **insert-text-button** *label \&rest properties*

This inserts a button with the label `label` at point, using text properties.
