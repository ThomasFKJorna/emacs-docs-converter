

### 38.13 Transaction Queues

You can use a *transaction queue* to communicate with a subprocess using transactions. First use `tq-create` to create a transaction queue communicating with a specified process. Then you can call `tq-enqueue` to send a transaction.

### Function: **tq-create** *process*

This function creates and returns a transaction queue communicating with `process`. The argument `process` should be a subprocess capable of sending and receiving streams of bytes. It may be a child process, or it may be a TCP connection to a server, possibly on another machine.

### Function: **tq-enqueue** *queue question regexp closure fn \&optional delay-question*

This function sends a transaction to queue `queue`. Specifying the queue has the effect of specifying the subprocess to talk to.

The argument `question` is the outgoing message that starts the transaction. The argument `fn` is the function to call when the corresponding answer comes back; it is called with two arguments: `closure`, and the answer received.

The argument `regexp` is a regular expression that should match text at the end of the entire answer, but nothing before; that’s how `tq-enqueue` determines where the answer ends.

If the argument `delay-question` is non-`nil`, delay sending this question until the process has finished replying to any previous questions. This produces more reliable results with some processes.

### Function: **tq-close** *queue*

Shut down transaction queue `queue`, waiting for all pending transactions to complete, and then terminate the connection or child process.

Transaction queues are implemented by means of a filter function. See [Filter Functions](Filter-Functions.html).
