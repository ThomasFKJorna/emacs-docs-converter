

#### 32.30.2 Process-based JSONRPC connections

For convenience, the `jsonrpc` library comes with a built-in `jsonrpc-process-connection` transport implementation that can talk to local subprocesses (using the standard input and standard output); or TCP hosts (using sockets); or any other remote endpoint that Emacs’s process object can represent (see [Processes](Processes.html)).

Using this transport, the JSONRPC messages are encoded on the wire as plain text and prefaced by some basic HTTP-style enveloping headers, such as “Content-Length”.

For an example of an application using this transport scheme on top of JSONRPC, see the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/specification).

Along with the mandatory `:request-dispatcher` and `:notification-dispatcher` initargs, users of the `jsonrpc-process-connection` class should pass the following initargs as keyword-value pairs to `make-instance`:

### `:process`

Value must be a live process object or a function of no arguments producing one such object. If passed a process object, the object is expected to contain a pre-established connection; otherwise, the function is called immediately after the object is made.

### `:on-shutdown`

Value must be a function of a single argument, the `jsonrpc-process-connection` object. The function is called after the underlying process object has been deleted (either deliberately by `jsonrpc-shutdown`, or unexpectedly, because of some external cause).
