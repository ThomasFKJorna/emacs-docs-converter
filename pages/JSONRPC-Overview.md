

Next: [Process-based JSONRPC connections](Process_002dbased-JSONRPC-connections.html), Up: [JSONRPC](JSONRPC.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.30.1 Overview

Quoting from the [spec](https://www.jsonrpc.org/), JSONRPC "is transport agnostic in that the concepts can be used within the same process, over sockets, over http, or in many various message passing environments."

To model this agnosticism, the `jsonrpc` library uses objects of a `jsonrpc-connection` class, which represent a connection to a remote JSON endpoint (for details on Emacs’s object system, see [EIEIO](https://www.gnu.org/software/emacs/manual/html_node/eieio/index.html#Top) in EIEIO). In modern object-oriented parlance, this class is “abstract”, i.e. the actual class of a useful connection object is always a subclass of `jsonrpc-connection`. Nevertheless, we can define two distinct APIs around the `jsonrpc-connection` class:

1.  A user interface for building JSONRPC applications

    In this scenario, the JSONRPC application selects a concrete subclass of `jsonrpc-connection`, and proceeds to create objects of that subclass using `make-instance`. To initiate a contact to the remote endpoint, the JSONRPC application passes this object to the functions `jsonrpc-notify`, `jsonrpc-request`, and/or `jsonrpc-async-request`. For handling remotely initiated contacts, which generally come in asynchronously, the instantiation should include `:request-dispatcher` and `:notification-dispatcher` initargs, which are both functions of 3 arguments: the connection object; a symbol naming the JSONRPC method invoked remotely; and a JSONRPC `params` object.

    The function passed as `:request-dispatcher` is responsible for handling the remote endpoint’s requests, which expect a reply from the local endpoint (in this case, the program you’re building). Inside that function, you may either return locally (a normal return) or non-locally (an error return). A local return value must be a Lisp object that can be serialized as JSON (see [Parsing JSON](Parsing-JSON.html)). This determines a success response, and the object is forwarded to the server as the JSONRPC `result` object. A non-local return, achieved by calling the function `jsonrpc-error`, causes an error response to be sent to the server. The details of the accompanying JSONRPC `error` are filled out with whatever was passed to `jsonrpc-error`. A non-local return triggered by an unexpected error of any other type also causes an error response to be sent (unless you have set `debug-on-error`, in which case this calls the Lisp debugger, see [Error Debugging](Error-Debugging.html)).

2.  A inheritance interface for building JSONRPC transport implementations

    In this scenario, `jsonrpc-connection` is subclassed to implement a different underlying transport strategy (for details on how to subclass, see [(eieio)Inheritance](https://www.gnu.org/software/emacs/manual/html_node/eieio/Inheritance.html#Inheritance).). Users of the application-building interface can then instantiate objects of this concrete class (using the `make-instance` function) and connect to JSONRPC endpoints using that strategy.

    This API has mandatory and optional parts.

    To allow its users to initiate JSONRPC contacts (notifications or requests) or reply to endpoint requests, the subclass must have an implementation of the `jsonrpc-connection-send` method.

    Likewise, for handling the three types of remote contacts (requests, notifications, and responses to local requests), the transport implementation must arrange for the function `jsonrpc-connection-receive` to be called after noticing a new JSONRPC message on the wire (whatever that "wire" may be).

    Finally, and optionally, the `jsonrpc-connection` subclass should implement the `jsonrpc-shutdown` and `jsonrpc-running-p` methods if these concepts apply to the transport. If they do, then any system resources (e.g. processes, timers, etc.) used to listen for messages on the wire should be released in `jsonrpc-shutdown`, i.e. they should only be needed while `jsonrpc-running-p` is non-nil.

Next: [Process-based JSONRPC connections](Process_002dbased-JSONRPC-connections.html), Up: [JSONRPC](JSONRPC.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
