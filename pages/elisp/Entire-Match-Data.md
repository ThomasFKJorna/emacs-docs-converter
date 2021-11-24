

#### 34.6.3 Accessing the Entire Match Data

The functions `match-data` and `set-match-data` read or write the entire match data, all at once.

### Function: **match-data** *\&optional integers reuse reseat*

This function returns a list of positions (markers or integers) that record all the information on the text that the last search matched. Element zero is the position of the beginning of the match for the whole expression; element one is the position of the end of the match for the expression. The next two elements are the positions of the beginning and end of the match for the first subexpression, and so on. In general, element number 2`n` corresponds to `(match-beginning n)`; and element number 2`n` + 1 corresponds to `(match-end n)`.

Normally all the elements are markers or `nil`, but if `integers` is non-`nil`, that means to use integers instead of markers. (In that case, the buffer itself is appended as an additional element at the end of the list, to facilitate complete restoration of the match data.) If the last match was done on a string with `string-match`, then integers are always used, since markers can’t point into a string.

If `reuse` is non-`nil`, it should be a list. In that case, `match-data` stores the match data in `reuse`. That is, `reuse` is destructively modified. `reuse` does not need to have the right length. If it is not long enough to contain the match data, it is extended. If it is too long, the length of `reuse` stays the same, but the elements that were not used are set to `nil`. The purpose of this feature is to reduce the need for garbage collection.

If `reseat` is non-`nil`, all markers on the `reuse` list are reseated to point to nowhere.

As always, there must be no possibility of intervening searches between the call to a search function and the call to `match-data` that is intended to access the match data for that search.

```lisp
(match-data)
     ⇒  (#<marker at 9 in foo>
          #<marker at 17 in foo>
          #<marker at 13 in foo>
          #<marker at 17 in foo>)
```

### Function: **set-match-data** *match-list \&optional reseat*

This function sets the match data from the elements of `match-list`, which should be a list that was the value of a previous call to `match-data`. (More precisely, anything that has the same format will work.)

If `match-list` refers to a buffer that doesn’t exist, you don’t get an error; that sets the match data in a meaningless but harmless way.

If `reseat` is non-`nil`, all markers on the `match-list` list are reseated to point to nowhere.

`store-match-data` is a semi-obsolete alias for `set-match-data`.
