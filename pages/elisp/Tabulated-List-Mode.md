

#### 23.2.7 Tabulated List mode

Tabulated List mode is a major mode for displaying tabulated data, i.e., data consisting of *entries*, each entry occupying one row of text with its contents divided into columns. Tabulated List mode provides facilities for pretty-printing rows and columns, and sorting the rows according to the values in each column. It is derived from Special mode (see [Basic Major Modes](Basic-Major-Modes.html)).

Tabulated List mode is intended to be used as a parent mode by a more specialized major mode. Examples include Process Menu mode (see [Process Information](Process-Information.html)) and Package Menu mode (see [Package Menu](https://www.gnu.org/software/emacs/manual/html_node/emacs/Package-Menu.html#Package-Menu) in The GNU Emacs Manual).

Such a derived mode should use `define-derived-mode` in the usual way, specifying `tabulated-list-mode` as the second argument (see [Derived Modes](Derived-Modes.html)). The body of the `define-derived-mode` form should specify the format of the tabulated data, by assigning values to the variables documented below; optionally, it can then call the function `tabulated-list-init-header`, which will populate a header with the names of the columns.

The derived mode should also define a *listing command*. This, not the mode command, is what the user calls (e.g., `M-x list-processes`). The listing command should create or switch to a buffer, turn on the derived mode, specify the tabulated data, and finally call `tabulated-list-print` to populate the buffer.

### User Option: **tabulated-list-gui-sort-indicator-asc**

This variable specifies the character to be used on GUI frames as an indication that the column is sorted in the ascending order.

Whenever you change the sort direction in Tabulated List buffers, this indicator toggles between ascending (“asc”) and descending (“desc”).

### User Option: **tabulated-list-gui-sort-indicator-desc**

Like `tabulated-list-gui-sort-indicator-asc`, but used when the column is sorted in the descending order.

### User Option: **tabulated-list-tty-sort-indicator-asc**

Like `tabulated-list-gui-sort-indicator-asc`, but used for text-mode frames.

### User Option: **tabulated-list-tty-sort-indicator-desc**

Like `tabulated-list-tty-sort-indicator-asc`, but used when the column is sorted in the descending order.

### Variable: **tabulated-list-format**

This buffer-local variable specifies the format of the Tabulated List data. Its value should be a vector. Each element of the vector represents a data column, and should be a list `(name width sort)`, where

*   `name` is the column’s name (a string).
*   `width` is the width to reserve for the column (an integer). This is meaningless for the last column, which runs to the end of each line.
*   `sort` specifies how to sort entries by the column. If `nil`, the column cannot be used for sorting. If `t`, the column is sorted by comparing string values. Otherwise, this should be a predicate function for `sort` (see [Rearrangement](Rearrangement.html)), which accepts two arguments with the same form as the elements of `tabulated-list-entries` (see below).

### Variable: **tabulated-list-entries**

This buffer-local variable specifies the entries displayed in the Tabulated List buffer. Its value should be either a list, or a function.

If the value is a list, each list element corresponds to one entry, and should have the form `(id contents)`, where

*   `id` is either `nil`, or a Lisp object that identifies the entry. If the latter, the cursor stays on the same entry when re-sorting entries. Comparison is done with `equal`.

*   `contents` is a vector with the same number of elements as `tabulated-list-format`. Each vector element is either a string, which is inserted into the buffer as-is, or a list `(label . properties)`, which means to insert a text button by calling `insert-text-button` with `label` and `properties` as arguments (see [Making Buttons](Making-Buttons.html)).

    There should be no newlines in any of these strings.

Otherwise, the value should be a function which returns a list of the above form when called with no arguments.

### Variable: **tabulated-list-revert-hook**

This normal hook is run prior to reverting a Tabulated List buffer. A derived mode can add a function to this hook to recompute `tabulated-list-entries`.

### Variable: **tabulated-list-printer**

The value of this variable is the function called to insert an entry at point, including its terminating newline. The function should accept two arguments, `id` and `contents`, having the same meanings as in `tabulated-list-entries`. The default value is a function which inserts an entry in a straightforward way; a mode which uses Tabulated List mode in a more complex way can specify another function.

### Variable: **tabulated-list-sort-key**

The value of this variable specifies the current sort key for the Tabulated List buffer. If it is `nil`, no sorting is done. Otherwise, it should have the form `(name . flip)`, where `name` is a string matching one of the column names in `tabulated-list-format`, and `flip`, if non-`nil`, means to invert the sort order.

### Function: **tabulated-list-init-header**

This function computes and sets `header-line-format` for the Tabulated List buffer (see [Header Lines](Header-Lines.html)), and assigns a keymap to the header line to allow sorting entries by clicking on column headers.

Modes derived from Tabulated List mode should call this after setting the above variables (in particular, only after setting `tabulated-list-format`).

### Function: **tabulated-list-print** *\&optional remember-pos update*

This function populates the current buffer with entries. It should be called by the listing command. It erases the buffer, sorts the entries specified by `tabulated-list-entries` according to `tabulated-list-sort-key`, then calls the function specified by `tabulated-list-printer` to insert each entry.

If the optional argument `remember-pos` is non-`nil`, this function looks for the `id` element on the current line, if any, and tries to move to that entry after all the entries are (re)inserted.

If the optional argument `update` is non-`nil`, this function will only erase or add entries that have changed since the last print. This is several times faster if most entries haven’t changed since the last time this function was called. The only difference in outcome is that tags placed via `tabulated-list-put-tag` will not be removed from entries that haven’t changed (normally all tags are removed).

### Function: **tabulated-list-delete-entry**

This function deletes the entry at point.

It returns a list `(id cols)`, where `id` is the ID of the deleted entry and `cols` is a vector of its column descriptors. It moves point to the beginning of the current line. It returns `nil` if there is no entry at point.

Note that this function only changes the buffer contents; it does not alter `tabulated-list-entries`.

### Function: **tabulated-list-get-id** *\&optional pos*

This `defsubst` returns the ID object from `tabulated-list-entries` (if that is a list) or from the list returned by `tabulated-list-entries` (if it is a function). If omitted or `nil`, `pos` defaults to point.

### Function: **tabulated-list-get-entry** *\&optional pos*

This `defsubst` returns the entry object from `tabulated-list-entries` (if that is a list) or from the list returned by `tabulated-list-entries` (if it is a function). This will be a vector for the ID at `pos`. If there is no entry at `pos`, then the function returns `nil`.

### Function: **tabulated-list-header-overlay-p** *\&optional POS*

This `defsubst` returns non-nil if there is a fake header at `pos`. A fake header is used if `tabulated-list-use-header-line` is `nil` to put the column names at the beginning of the buffer. If omitted or `nil`, `pos` defaults to `point-min`.

### Function: **tabulated-list-put-tag** *tag \&optional advance*

This function puts `tag` in the padding area of the current line. The padding area can be empty space at the beginning of the line, the width of which is governed by `tabulated-list-padding`. `tag` should be a string, with a length less than or equal to `tabulated-list-padding`. If `advance` is non-nil, this function advances point by one line.

### Function: **tabulated-list-clear-all-tags**

This function clears all tags from the padding area in the current buffer.

### Function: **tabulated-list-set-col** *col desc \&optional change-entry-data*

This function changes the tabulated list entry at point, setting `col` to `desc`. `col` is the column number to change, or the name of the column to change. `desc` is the new column descriptor, which is inserted via `tabulated-list-print-col`.

If `change-entry-data` is non-nil, this function modifies the underlying data (usually the column descriptor in the list `tabulated-list-entries`) by setting the column descriptor of the vector to `desc`.
