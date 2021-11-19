

Next: [Match Data](Match-Data.html), Previous: [Regexp Search](Regexp-Search.html), Up: [Searching and Matching](Searching-and-Matching.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 34.5 POSIX Regular Expression Searching

The usual regular expression functions do backtracking when necessary to handle the ‘`\|`’ and repetition constructs, but they continue this only until they find *some* match. Then they succeed and report the first match found.

This section describes alternative search functions which perform the full backtracking specified by the POSIX standard for regular expression matching. They continue backtracking until they have tried all possibilities and found all matches, so they can report the longest match, as required by POSIX. This is much slower, so use these functions only when you really need the longest match.

The POSIX search and match functions do not properly support the non-greedy repetition operators (see [non-greedy](Regexp-Special.html)). This is because POSIX backtracking conflicts with the semantics of non-greedy repetition.

*   Command: **posix-search-forward** *regexp \&optional limit noerror count*

    This is like `re-search-forward` except that it performs the full backtracking specified by the POSIX standard for regular expression matching.

<!---->

*   Command: **posix-search-backward** *regexp \&optional limit noerror count*

    This is like `re-search-backward` except that it performs the full backtracking specified by the POSIX standard for regular expression matching.

<!---->

*   Function: **posix-looking-at** *regexp*

    This is like `looking-at` except that it performs the full backtracking specified by the POSIX standard for regular expression matching.

<!---->

*   Function: **posix-string-match** *regexp string \&optional start*

    This is like `string-match` except that it performs the full backtracking specified by the POSIX standard for regular expression matching.
