<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Surprising Local Vars](Surprising-Local-Vars.html), Previous: [Wrong Time](Wrong-Time.html), Up: [Problems with Macros](Problems-with-Macros.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 14.5.2 Evaluating Macro Arguments Repeatedly

When defining a macro you must pay attention to the number of times the arguments will be evaluated when the expansion is executed. The following macro (used to facilitate iteration) illustrates the problem. This macro allows us to write a for-loop construct.

    (defmacro for (var from init to final do &rest body)
      "Execute a simple \"for\" loop.
    For example, (for i from 1 to 10 do (print i))."
      (list 'let (list (list var init))
            (cons 'while
                  (cons (list '<= var final)
                        (append body (list (list 'inc var)))))))

```
```

    (for i from 1 to 3 do
       (setq square (* i i))
       (princ (format "\n%d %d" i square)))
    →

<!---->

    (let ((i 1))
      (while (<= i 3)
        (setq square (* i i))
        (princ (format "\n%d %d" i square))
        (inc i)))

```

     -|1       1
     -|2       4
     -|3       9
⇒ nil
```

The arguments `from`, `to`, and `do` in this macro are syntactic sugar; they are entirely ignored. The idea is that you will write noise words (such as `from`, `to`, and `do`) in those positions in the macro call.

Here’s an equivalent definition simplified through use of backquote:

    (defmacro for (var from init to final do &rest body)
      "Execute a simple \"for\" loop.
    For example, (for i from 1 to 10 do (print i))."
      `(let ((,var ,init))
         (while (<= ,var ,final)
           ,@body
           (inc ,var))))

Both forms of this definition (with backquote and without) suffer from the defect that `final` is evaluated on every iteration. If `final` is a constant, this is not a problem. If it is a more complex form, say `(long-complex-calculation x)`, this can slow down the execution significantly. If `final` has side effects, executing it more than once is probably incorrect.

A well-designed macro definition takes steps to avoid this problem by producing an expansion that evaluates the argument expressions exactly once unless repeated evaluation is part of the intended purpose of the macro. Here is a correct expansion for the `for` macro:

    (let ((i 1)
          (max 3))
      (while (<= i max)
        (setq square (* i i))
        (princ (format "%d      %d" i square))
        (inc i)))

Here is a macro definition that creates this expansion:

    (defmacro for (var from init to final do &rest body)
      "Execute a simple for loop: (for i from 1 to 10 do (print i))."
      `(let ((,var ,init)
             (max ,final))
         (while (<= ,var max)
           ,@body
           (inc ,var))))

Unfortunately, this fix introduces another problem, described in the following section.

Next: [Surprising Local Vars](Surprising-Local-Vars.html), Previous: [Wrong Time](Wrong-Time.html), Up: [Problems with Macros](Problems-with-Macros.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
