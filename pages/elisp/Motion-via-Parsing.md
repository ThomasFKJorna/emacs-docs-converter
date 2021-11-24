

#### 35.6.1 Motion Commands Based on Parsing

This section describes simple point-motion functions that operate based on parsing expressions.

### Function: **scan-lists** *from count depth*

This function scans forward `count` balanced parenthetical groupings from position `from`. It returns the position where the scan stops. If `count` is negative, the scan moves backwards.

If `depth` is nonzero, treat the starting position as being `depth` parentheses deep. The scanner moves forward or backward through the buffer until the depth changes to zero `count` times. Hence, a positive value for `depth` has the effect of moving out `depth` levels of parenthesis from the starting position, while a negative `depth` has the effect of moving deeper by `-depth` levels of parenthesis.

Scanning ignores comments if `parse-sexp-ignore-comments` is non-`nil`.

If the scan reaches the beginning or end of the accessible part of the buffer before it has scanned over `count` parenthetical groupings, the return value is `nil` if the depth at that point is zero; if the depth is non-zero, a `scan-error` error is signaled.

### Function: **scan-sexps** *from count*

This function scans forward `count` sexps from position `from`. It returns the position where the scan stops. If `count` is negative, the scan moves backwards.

Scanning ignores comments if `parse-sexp-ignore-comments` is non-`nil`.

If the scan reaches the beginning or end of (the accessible part of) the buffer while in the middle of a parenthetical grouping, an error is signaled. If it reaches the beginning or end between groupings but before count is used up, `nil` is returned.

### Function: **forward-comment** *count*

This function moves point forward across `count` complete comments (that is, including the starting delimiter and the terminating delimiter if any), plus any whitespace encountered on the way. It moves backward if `count` is negative. If it encounters anything other than a comment or whitespace, it stops, leaving point at the place where it stopped. This includes (for instance) finding the end of a comment when moving forward and expecting the beginning of one. The function also stops immediately after moving over the specified number of complete comments. If `count` comments are found as expected, with nothing except whitespace between them, it returns `t`; otherwise it returns `nil`.

This function cannot tell whether the comments it traverses are embedded within a string. If they look like comments, it treats them as comments.

To move forward over all comments and whitespace following point, use `(forward-comment (buffer-size))`. `(buffer-size)` is a good argument to use, because the number of comments in the buffer cannot exceed that many.
