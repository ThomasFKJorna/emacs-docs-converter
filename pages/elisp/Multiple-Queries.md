

### 20.8 Asking Multiple-Choice Questions

This section describes facilities for asking the user more complex questions or several similar questions.

When you have a series of similar questions to ask, such as “Do you want to save this buffer?” for each buffer in turn, you should use `map-y-or-n-p` to ask the collection of questions, rather than asking each question individually. This gives the user certain convenient facilities such as the ability to answer the whole series at once.

### Function: **map-y-or-n-p** *prompter actor list \&optional help action-alist no-cursor-in-echo-area*

This function asks the user a series of questions, reading a single-character answer in the echo area for each one.

The value of `list` specifies the objects to ask questions about. It should be either a list of objects or a generator function. If it is a function, it should expect no arguments, and should return either the next object to ask about, or `nil`, meaning to stop asking questions.

The argument `prompter` specifies how to ask each question. If `prompter` is a string, the question text is computed like this:

```lisp
(format prompter object)
```

where `object` is the next object to ask about (as obtained from `list`).

If not a string, `prompter` should be a function of one argument (the next object to ask about) and should return the question text. If the value is a string, that is the question to ask the user. The function can also return `t`, meaning do act on this object (and don’t ask the user), or `nil`, meaning ignore this object (and don’t ask the user).

The argument `actor` says how to act on the answers that the user gives. It should be a function of one argument, and it is called with each object that the user says yes for. Its argument is always an object obtained from `list`.

If the argument `help` is given, it should be a list of this form:

```lisp
(singular plural action)
```

where `singular` is a string containing a singular noun that describes the objects conceptually being acted on, `plural` is the corresponding plural noun, and `action` is a transitive verb describing what `actor` does.

If you don’t specify `help`, the default is `("object" "objects" "act on")`.

Each time a question is asked, the user may enter `y`, `Y`, or `SPC` to act on that object; `n`, `N`, or `DEL` to skip that object; `!` to act on all following objects; `ESC` or `q` to exit (skip all following objects); `.` (period) to act on the current object and then exit; or `C-h` to get help. These are the same answers that `query-replace` accepts. The keymap `query-replace-map` defines their meaning for `map-y-or-n-p` as well as for `query-replace`; see [Search and Replace](Search-and-Replace.html).

You can use `action-alist` to specify additional possible answers and what they mean. It is an alist of elements of the form `(char function help)`, each of which defines one additional answer. In this element, `char` is a character (the answer); `function` is a function of one argument (an object from `list`); `help` is a string.

When the user responds with `char`, `map-y-or-n-p` calls `function`. If it returns non-`nil`, the object is considered acted upon, and `map-y-or-n-p` advances to the next object in `list`. If it returns `nil`, the prompt is repeated for the same object.

Normally, `map-y-or-n-p` binds `cursor-in-echo-area` while prompting. But if `no-cursor-in-echo-area` is non-`nil`, it does not do that.

If `map-y-or-n-p` is called in a command that was invoked using the mouse—more precisely, if `last-nonmenu-event` (see [Command Loop Info](Command-Loop-Info.html)) is either `nil` or a list—then it uses a dialog box or pop-up menu to ask the question. In this case, it does not use keyboard input or the echo area. You can force use either of the mouse or of keyboard input by binding `last-nonmenu-event` to a suitable value around the call.

The return value of `map-y-or-n-p` is the number of objects acted on.

If you need to ask the user a question that might have more than just 2 answers, use `read-answer`.

### Function: **read-answer** *question answers*

This function prompts the user with text in `question`, which should end in the ‘`SPC`’ character. The function includes in the prompt the possible responses in `answers` by appending them to the end of `question`. The possible responses are provided in `answers` as an alist whose elements are of the following form:

```lisp
(long-answer short-answer help-message)
```

where `long-answer` is the complete text of the user response, a string; `short-answer` is a short form of the same response, a single character or a function key; and `help-message` is the text that describes the meaning of the answer. If the variable `read-answer-short` is non-`nil`, the prompt will show the short variants of the possible answers and the user is expected to type the single characters/keys shown in the prompt; otherwise the prompt will show the long variants of the answers, and the user is expected to type the full text of one of the answers and end by pressing `RET`. If `use-dialog-box` is non-`nil`, and this function was invoked by mouse events, the question and the answers will be displayed in a GUI dialog box.

The function returns the text of the `long-answer` selected by the user, regardless of whether long or short answers were shown in the prompt and typed by the user.

Here is an example of using this function:

```lisp
(let ((read-answer-short t))
  (read-answer "Foo "
     '(("yes"  ?y "perform the action")
       ("no"   ?n "skip to the next")
       ("all"  ?! "perform for the rest without more questions")
       ("help" ?h "show help")
       ("quit" ?q "exit"))))
```

### Function: **read-char-from-minibuffer** *prompt \&optional chars history*

This function uses the minibuffer to read and return a single character. Optionally, it ignores any input that is not a member of `chars`, a list of accepted characters. The `history` argument specifies the history list symbol to use; if it is omitted or `nil`, this function doesn’t use the history.
