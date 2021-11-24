

### 32.7 User-Level Deletion Commands

This section describes higher-level commands for deleting text, commands intended primarily for the user but useful also in Lisp programs.

### Command: **delete-horizontal-space** *\&optional backward-only*

This function deletes all spaces and tabs around point. It returns `nil`.

If `backward-only` is non-`nil`, the function deletes spaces and tabs before point, but not after point.

In the following examples, we call `delete-horizontal-space` four times, once on each line, with point between the second and third characters on the line each time.

```lisp
---------- Buffer: foo ----------
I ∗thought
I ∗     thought
We∗ thought
Yo∗u thought
---------- Buffer: foo ----------
```

```lisp
```

```lisp
(delete-horizontal-space)   ; Four times.
     ⇒ nil

---------- Buffer: foo ----------
Ithought
Ithought
Wethought
You thought
---------- Buffer: foo ----------
```

### Command: **delete-indentation** *\&optional join-following-p beg end*

This function joins the line point is on to the previous line, deleting any whitespace at the join and in some cases replacing it with one space. If `join-following-p` is non-`nil`, `delete-indentation` joins this line to the following line instead. Otherwise, if `beg` and `end` are non-`nil`, this function joins all lines in the region they define.

In an interactive call, `join-following-p` is the prefix argument, and `beg` and `end` are, respectively, the start and end of the region if it is active, else `nil`. The function returns `nil`.

If there is a fill prefix, and the second of the lines being joined starts with the prefix, then `delete-indentation` deletes the fill prefix before joining the lines. See [Margins](Margins.html).

In the example below, point is located on the line starting ‘`events`’, and it makes no difference if there are trailing spaces in the preceding line.

```lisp
---------- Buffer: foo ----------
When in the course of human
∗    events, it becomes necessary
---------- Buffer: foo ----------
```

```lisp

(delete-indentation)
     ⇒ nil
```

```lisp
---------- Buffer: foo ----------
When in the course of human∗ events, it becomes necessary
---------- Buffer: foo ----------
```

After the lines are joined, the function `fixup-whitespace` is responsible for deciding whether to leave a space at the junction.

### Command: **fixup-whitespace**

This function replaces all the horizontal whitespace surrounding point with either one space or no space, according to the context. It returns `nil`.

At the beginning or end of a line, the appropriate amount of space is none. Before a character with close parenthesis syntax, or after a character with open parenthesis or expression-prefix syntax, no space is also appropriate. Otherwise, one space is appropriate. See [Syntax Class Table](Syntax-Class-Table.html).

In the example below, `fixup-whitespace` is called the first time with point before the word ‘`spaces`’ in the first line. For the second invocation, point is directly after the ‘`(`’.

```lisp
---------- Buffer: foo ----------
This has too many     ∗spaces
This has too many spaces at the start of (∗   this list)
---------- Buffer: foo ----------
```

```lisp
```

```lisp
(fixup-whitespace)
     ⇒ nil
(fixup-whitespace)
     ⇒ nil
```

```lisp
```

```lisp
---------- Buffer: foo ----------
This has too many spaces
This has too many spaces at the start of (this list)
---------- Buffer: foo ----------
```

### Command: **just-one-space** *\&optional n*

This command replaces any spaces and tabs around point with a single space, or `n` spaces if `n` is specified. It returns `nil`.

### Command: **delete-blank-lines**

This function deletes blank lines surrounding point. If point is on a blank line with one or more blank lines before or after it, then all but one of them are deleted. If point is on an isolated blank line, then it is deleted. If point is on a nonblank line, the command deletes all blank lines immediately following it.

A blank line is defined as a line containing only tabs and spaces.

`delete-blank-lines` returns `nil`.

### Command: **delete-trailing-whitespace** *\&optional start end*

Delete trailing whitespace in the region defined by `start` and `end`.

This command deletes whitespace characters after the last non-whitespace character in each line in the region.

If this command acts on the entire buffer (i.e., if called interactively with the mark inactive, or called from Lisp with `end` `nil`), it also deletes all trailing lines at the end of the buffer if the variable `delete-trailing-lines` is non-`nil`.
