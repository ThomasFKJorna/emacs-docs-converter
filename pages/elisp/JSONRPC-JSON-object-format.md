

#### 32.30.3 JSONRPC JSON object format

JSONRPC JSON objects are exchanged as Lisp plists (see [Property Lists](Property-Lists.html)): JSON-compatible plists are handed to the dispatcher functions and, likewise, JSON-compatible plists should be given to `jsonrpc-notify`, `jsonrpc-request`, and `jsonrpc-async-request`.

To facilitate handling plists, this library makes liberal use of `cl-lib` library (see [cl-lib](https://www.gnu.org/software/emacs/manual/html_node/cl/index.html#Top) in Common Lisp Extensions for GNU Emacs Lisp) and suggests (but doesn’t force) its clients to do the same. A macro `jsonrpc-lambda` can be used to create a lambda for destructuring a JSON-object like in this example:

```lisp
(jsonrpc-async-request
 myproc :frobnicate `(:foo "trix")
 :success-fn (jsonrpc-lambda (&key bar baz &allow-other-keys)
               (message "Server replied back with %s and %s!"
                        bar baz))
 :error-fn (jsonrpc-lambda (&key code message _data)
             (message "Sadly, server reports %s: %s"
                      code message)))
```
