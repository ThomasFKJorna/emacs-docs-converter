

### 21.6 Adjusting Point After Commands

Emacs cannot display the cursor when point is in the middle of a sequence of text that has the `display` or `composition` property, or is invisible. Therefore, after a command finishes and returns to the command loop, if point is within such a sequence, the command loop normally moves point to the edge of the sequence, making this sequence effectively intangible.

A command can inhibit this feature by setting the variable `disable-point-adjustment`:

### Variable: **disable-point-adjustment**

If this variable is non-`nil` when a command returns to the command loop, then the command loop does not check for those text properties, and does not move point out of sequences that have them.

The command loop sets this variable to `nil` before each command, so if a command sets it, the effect applies only to that command.

### Variable: **global-disable-point-adjustment**

If you set this variable to a non-`nil` value, the feature of moving point out of these sequences is completely turned off.
