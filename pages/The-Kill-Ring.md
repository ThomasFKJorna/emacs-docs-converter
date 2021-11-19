

Next: [Undo](Undo.html), Previous: [User-Level Deletion](User_002dLevel-Deletion.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.8 The Kill Ring

*Kill functions* delete text like the deletion functions, but save it so that the user can reinsert it by *yanking*. Most of these functions have ‘`kill-`’ in their name. By contrast, the functions whose names start with ‘`delete-`’ normally do not save text for yanking (though they can still be undone); these are deletion functions.

Most of the kill commands are primarily for interactive use, and are not described here. What we do describe are the functions provided for use in writing such commands. You can use these functions to write commands for killing text. When you need to delete text for internal purposes within a Lisp function, you should normally use deletion functions, so as not to disturb the kill ring contents. See [Deletion](Deletion.html).

Killed text is saved for later yanking in the *kill ring*. This is a list that holds a number of recent kills, not just the last text kill. We call this a “ring” because yanking treats it as having elements in a cyclic order. The list is kept in the variable `kill-ring`, and can be operated on with the usual functions for lists; there are also specialized functions, described in this section, that treat it as a ring.

Some people think this use of the word “kill” is unfortunate, since it refers to operations that specifically *do not* destroy the entities killed. This is in sharp contrast to ordinary life, in which death is permanent and killed entities do not come back to life. Therefore, other metaphors have been proposed. For example, the term “cut ring” makes sense to people who, in pre-computer days, used scissors and paste to cut up and rearrange manuscripts. However, it would be difficult to change the terminology now.

|                                                         |    |                                               |
| :------------------------------------------------------ | -- | :-------------------------------------------- |
| • [Kill Ring Concepts](Kill-Ring-Concepts.html)         |    | What text looks like in the kill ring.        |
| • [Kill Functions](Kill-Functions.html)                 |    | Functions that kill text.                     |
| • [Yanking](Yanking.html)                               |    | How yanking is done.                          |
| • [Yank Commands](Yank-Commands.html)                   |    | Commands that access the kill ring.           |
| • [Low-Level Kill Ring](Low_002dLevel-Kill-Ring.html)   |    | Functions and variables for kill ring access. |
| • [Internals of Kill Ring](Internals-of-Kill-Ring.html) |    | Variables that hold kill ring data.           |

Next: [Undo](Undo.html), Previous: [User-Level Deletion](User_002dLevel-Deletion.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
