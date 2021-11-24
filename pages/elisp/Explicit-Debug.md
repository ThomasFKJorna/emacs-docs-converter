

#### 18.1.5 Explicit Entry to the Debugger

You can cause the debugger to be called at a certain point in your program by writing the expression `(debug)` at that point. To do this, visit the source file, insert the text ‘`(debug)`’ at the proper place, and type `C-M-x` (`eval-defun`, a Lisp mode key binding). **Warning:** if you do this for temporary debugging purposes, be sure to undo this insertion before you save the file!

The place where you insert ‘`(debug)`’ must be a place where an additional form can be evaluated and its value ignored. (If the value of `(debug)` isn’t ignored, it will alter the execution of the program!) The most common suitable places are inside a `progn` or an implicit `progn` (see [Sequencing](Sequencing.html)).

If you don’t know exactly where in the source code you want to put the debug statement, but you want to display a backtrace when a certain message is displayed, you can set `debug-on-message` to a regular expression matching the desired message.
