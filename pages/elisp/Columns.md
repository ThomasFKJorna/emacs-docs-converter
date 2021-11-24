

### 32.16 Counting Columns

The column functions convert between a character position (counting characters from the beginning of the buffer) and a column position (counting screen characters from the beginning of a line).

These functions count each character according to the number of columns it occupies on the screen. This means control characters count as occupying 2 or 4 columns, depending upon the value of `ctl-arrow`, and tabs count as occupying a number of columns that depends on the value of `tab-width` and on the column where the tab begins. See [Usual Display](Usual-Display.html).

Column number computations ignore the width of the window and the amount of horizontal scrolling. Consequently, a column value can be arbitrarily high. The first (or leftmost) column is numbered 0. They also ignore overlays and text properties, aside from invisibility.

### Function: **current-column**

This function returns the horizontal position of point, measured in columns, counting from 0 at the left margin. The column position is the sum of the widths of all the displayed representations of the characters between the start of the current line and point.

### Command: **move-to-column** *column \&optional force*

This function moves point to `column` in the current line. The calculation of `column` takes into account the widths of the displayed representations of the characters between the start of the line and point.

When called interactively, `column` is the value of prefix numeric argument. If `column` is not an integer, an error is signaled.

If it is impossible to move to column `column` because that is in the middle of a multicolumn character such as a tab, point moves to the end of that character. However, if `force` is non-`nil`, and `column` is in the middle of a tab, then `move-to-column` either converts the tab into spaces (when `indent-tabs-mode` is `nil`), or inserts enough spaces before it (otherwise), so that point can move precisely to column `column`. Other multicolumn characters can cause anomalies despite `force`, since there is no way to split them.

The argument `force` also has an effect if the line isn’t long enough to reach column `column`; if it is `t`, that means to add whitespace at the end of the line to reach that column.

The return value is the column number actually moved to.
