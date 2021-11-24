

### 18.4 Test Coverage

You can do coverage testing for a file of Lisp code by loading the `testcover` library and using the command `M-x testcover-start RET file RET` to instrument the code. Then test your code by calling it one or more times. Then use the command `M-x testcover-mark-all` to display colored highlights on the code to show where coverage is insufficient. The command `M-x testcover-next-mark` will move point forward to the next highlighted spot.

Normally, a red highlight indicates the form was never completely evaluated; a brown highlight means it always evaluated to the same value (meaning there has been little testing of what is done with the result). However, the red highlight is skipped for forms that can’t possibly complete their evaluation, such as `error`. The brown highlight is skipped for forms that are expected to always evaluate to the same value, such as `(setq x 14)`.

For difficult cases, you can add do-nothing macros to your code to give advice to the test coverage tool.

### Macro: **1value** *form*

Evaluate `form` and return its value, but inform coverage testing that `form`’s value should always be the same.

### Macro: **noreturn** *form*

Evaluate `form`, informing coverage testing that `form` should never return. If it ever does return, you get a run-time error.

Edebug also has a coverage testing feature (see [Coverage Testing](Coverage-Testing.html)). These features partly duplicate each other, and it would be cleaner to combine them.
