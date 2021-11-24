

### 2.3 Comments

A *comment* is text that is written in a program only for the sake of humans that read the program, and that has no effect on the meaning of the program. In Lisp, an unescaped semicolon (‘`;`’) starts a comment if it is not within a string or character constant. The comment continues to the end of line. The Lisp reader discards comments; they do not become part of the Lisp objects which represent the program within the Lisp system.

The ‘`#@count`’ construct, which skips the next `count` characters, is useful for program-generated comments containing binary data. The Emacs Lisp byte compiler uses this in its output files (see [Byte Compilation](Byte-Compilation.html)). It isn’t meant for source files, however.

See [Comment Tips](Comment-Tips.html), for conventions for formatting comments.
