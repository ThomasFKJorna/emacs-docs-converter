

Next: [Size of Displayed Text](Size-of-Displayed-Text.html), Previous: [Temporary Displays](Temporary-Displays.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.9 Overlays

You can use *overlays* to alter the appearance of a buffer’s text on the screen, for the sake of presentation features. An overlay is an object that belongs to a particular buffer, and has a specified beginning and end. It also has properties that you can examine and set; these affect the display of the text within the overlay.

The visual effect of an overlay is the same as of the corresponding text property (see [Text Properties](Text-Properties.html)). However, due to a different implementation, overlays generally don’t scale well (many operations take a time that is proportional to the number of overlays in the buffer). If you need to affect the visual appearance of many portions in the buffer, we recommend using text properties.

An overlay uses markers to record its beginning and end; thus, editing the text of the buffer adjusts the beginning and end of each overlay so that it stays with the text. When you create the overlay, you can specify whether text inserted at the beginning should be inside the overlay or outside, and likewise for the end of the overlay.

|                                                 |    |                                                                           |
| :---------------------------------------------- | -- | :------------------------------------------------------------------------ |
| • [Managing Overlays](Managing-Overlays.html)   |    | Creating and moving overlays.                                             |
| • [Overlay Properties](Overlay-Properties.html) |    | How to read and set properties. What properties do to the screen display. |
| • [Finding Overlays](Finding-Overlays.html)     |    | Searching for overlays.                                                   |
