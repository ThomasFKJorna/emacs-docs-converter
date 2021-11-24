

### 13.16 Determining whether a Function is Safe to Call

Some major modes, such as SES, call functions that are stored in user files. (See [(ses)Top](../ses/index.html#Top), for more information on SES.) User files sometimes have poor pedigrees—you can get a spreadsheet from someone you’ve just met, or you can get one through email from someone you’ve never met. So it is risky to call a function whose source code is stored in a user file until you have determined that it is safe.

### Function: **unsafep** *form \&optional unsafep-vars*

Returns `nil` if `form` is a *safe* Lisp expression, or returns a list that describes why it might be unsafe. The argument `unsafep-vars` is a list of symbols known to have temporary bindings at this point; it is mainly used for internal recursive calls. The current buffer is an implicit argument, which provides a list of buffer-local bindings.

Being quick and simple, `unsafep` does a very light analysis and rejects many Lisp expressions that are actually safe. There are no known cases where `unsafep` returns `nil` for an unsafe expression. However, a safe Lisp expression can return a string with a `display` property, containing an associated Lisp expression to be executed after the string is inserted into a buffer. This associated expression can be a virus. In order to be safe, you must delete properties from all strings calculated by user code before inserting them into buffers.
