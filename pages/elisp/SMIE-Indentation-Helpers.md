

#### 23.7.1.7 Helper Functions for Indentation Rules

SMIE provides various functions designed specifically for use in the indentation rules function (several of those functions break if used in another context). These functions all start with the prefix `smie-rule-`.

### Function: **smie-rule-bolp**

Return non-`nil` if the current token is the first on the line.

### Function: **smie-rule-hanging-p**

Return non-`nil` if the current token is *hanging*. A token is *hanging* if it is the last token on the line and if it is preceded by other tokens: a lone token on a line is not hanging.

### Function: **smie-rule-next-p** *\&rest tokens*

Return non-`nil` if the next token is among `tokens`.

### Function: **smie-rule-prev-p** *\&rest tokens*

Return non-`nil` if the previous token is among `tokens`.

### Function: **smie-rule-parent-p** *\&rest parents*

Return non-`nil` if the current token’s parent is among `parents`.

### Function: **smie-rule-sibling-p**

Return non-`nil` if the current token’s parent is actually a sibling. This is the case for example when the parent of a `","` is just the previous `","`.

### Function: **smie-rule-parent** *\&optional offset*

Return the proper offset to align the current token with the parent. If non-`nil`, `offset` should be an integer giving an additional offset to apply.

### Function: **smie-rule-separator** *method*

Indent current token as a *separator*.

By *separator*, we mean here a token whose sole purpose is to separate various elements within some enclosing syntactic construct, and which does not have any semantic significance in itself (i.e., it would typically not exist as a node in an abstract syntax tree).

Such a token is expected to have an associative syntax and be closely tied to its syntactic parent. Typical examples are `","` in lists of arguments (enclosed inside parentheses), or `";"` in sequences of instructions (enclosed in a `{...}` or `begin...end` block).

`method` should be the method name that was passed to `smie-rules-function`.
