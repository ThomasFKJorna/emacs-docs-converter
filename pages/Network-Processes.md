

Next: [Network Options](Network-Options.html), Up: [Low-Level Network](Low_002dLevel-Network.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 38.17.1 `make-network-process`

The basic function for creating network connections and network servers is `make-network-process`. It can do either of those jobs, depending on the arguments you give it.

*   Function: **make-network-process** *\&rest args*

    This function creates a network connection or server and returns the process object that represents it. The arguments `args` are a list of keyword/argument pairs. Omitting a keyword is always equivalent to specifying it with value `nil`, except for `:coding`, `:filter-multibyte`, and `:reuseaddr`. Here are the meaningful keywords (those corresponding to network options are listed in the following section):

    *   :name `name`

        Use the string `name` as the process name. It is modified if necessary to make it unique.

    *   :type `type`

        Specify the communication type. A value of `nil` specifies a stream connection (the default); `datagram` specifies a datagram connection; `seqpacket` specifies a sequenced packet stream connection. Both connections and servers can be of these types.

    *   :server `server-flag`

        If `server-flag` is non-`nil`, create a server. Otherwise, create a connection. For a stream type server, `server-flag` may be an integer, which then specifies the length of the queue of pending connections to the server. The default queue length is 5.

    *   :host `host`

        Specify the host to connect to. `host` should be a host name or Internet address, as a string, or the symbol `local` to specify the local host. If you specify `host` for a server, it must specify a valid address for the local host, and only clients connecting to that address will be accepted. When using `local`, by default IPv4 will be used, specify a `family` of `ipv6` to override this. To listen on all interfaces, specify an address of ‘`"0.0.0.0"`’ for IPv4 or ‘`"::"`’ for IPv6. Note that on some operating systems, listening on ‘`"::"`’ will also listen on IPv4, so attempting to then listen separately on IPv4 will result in `EADDRINUSE` errors (‘`"Address already in use"`’).

    *   :service `service`

        `service` specifies a port number to connect to; or, for a server, the port number to listen on. It should be a service name like ‘`"https"`’ that translates to a port number, or an integer like ‘`443`’ or an integer string like ‘`"443"`’ that specifies the port number directly. For a server, it can also be `t`, which means to let the system select an unused port number.

    *   :family `family`

        `family` specifies the address (and protocol) family for communication. `nil` means determine the proper address family automatically for the given `host` and `service`. `local` specifies a Unix socket, in which case `host` is ignored. `ipv4` and `ipv6` specify to use IPv4 and IPv6, respectively.

    *   :use-external-socket `use-external-socket`

        If `use-external-socket` is non-`nil` use any sockets passed to Emacs on invocation instead of allocating one. This is used by the Emacs server code to allow on-demand socket activation. If Emacs wasn’t passed a socket, this option is silently ignored.

    *   :local `local-address`

        For a server process, `local-address` is the address to listen on. It overrides `family`, `host` and `service`, so you might as well not specify them.

    *   :remote `remote-address`

        For a connection, `remote-address` is the address to connect to. It overrides `family`, `host` and `service`, so you might as well not specify them.

        For a datagram server, `remote-address` specifies the initial setting of the remote datagram address.

        The format of `local-address` or `remote-address` depends on the address family:

        *   \- An IPv4 address is represented as a five-element vector of four 8-bit integers and one 16-bit integer `[a b c d p]` corresponding to numeric IPv4 address `a`.`b`.`c`.`d` and port number `p`.
        *   \- An IPv6 address is represented as a nine-element vector of 16-bit integers `[a b c d e f g h p]` corresponding to numeric IPv6 address `a`:`b`:`c`:`d`:`e`:`f`:`g`:`h` and port number `p`.
        *   \- A local address is represented as a string, which specifies the address in the local address space.
        *   \- An unsupported-family address is represented by a cons `(f . av)`, where `f` is the family number and `av` is a vector specifying the socket address using one element per address data byte. Do not rely on this format in portable code, as it may depend on implementation defined constants, data sizes, and data structure alignment.

    *   :nowait `bool`

        If `bool` is non-`nil` for a stream connection, return without waiting for the connection to complete. When the connection succeeds or fails, Emacs will call the sentinel function, with a second argument matching `"open"` (if successful) or `"failed"`. The default is to block, so that `make-network-process` does not return until the connection has succeeded or failed.

        If you’re setting up an asynchronous TLS connection, you have to also provide the `:tls-parameters` parameter (see below).

        Depending on the capabilities of Emacs, how asynchronous `:nowait` is may vary. The three elements that may (or may not) be done asynchronously are domain name resolution, socket setup, and (for TLS connections) TLS negotiation.

        Many functions that interact with process objects, (for instance, `process-datagram-address`) rely on them at least having a socket before they can return a useful value. These functions will block until the socket has achieved the desired status. The recommended way of interacting with asynchronous sockets is to place a sentinel on the process, and not try to interact with it before it has changed status to ‘`"run"`’. That way, none of these functions will block.

    *   :tls-parameters

        When opening a TLS connection, this should be where the first element is the TLS type (which should either be `gnutls-x509pki` or `gnutls-anon`, and the remaining elements should form a keyword list acceptable for `gnutls-boot`. (This keyword list can be obtained from the `gnutls-boot-parameters` function.) The TLS connection will then be negotiated after completing the connection to the host.

    *   :stop `stopped`

        If `stopped` is non-`nil`, start the network connection or server in the stopped state.

    *   :buffer `buffer`

        Use `buffer` as the process buffer.

    *   :coding `coding`

        Use `coding` as the coding system for this process. To specify different coding systems for decoding data from the connection and for encoding data sent to it, specify `(decoding . encoding)` for `coding`.

        If you don’t specify this keyword at all, the default is to determine the coding systems from the data.

    *   :noquery `query-flag`

        Initialize the process query flag to `query-flag`. See [Query Before Exit](Query-Before-Exit.html).

    *   :filter `filter`

        Initialize the process filter to `filter`.

    *   :filter-multibyte `multibyte`

        If `multibyte` is non-`nil`, strings given to the process filter are multibyte, otherwise they are unibyte. The default is `t`.

    *   :sentinel `sentinel`

        Initialize the process sentinel to `sentinel`.

    *   :log `log`

        Initialize the log function of a server process to `log`. The log function is called each time the server accepts a network connection from a client. The arguments passed to the log function are `server`, `connection`, and `message`; where `server` is the server process, `connection` is the new process for the connection, and `message` is a string describing what has happened.

    *   :plist `plist`

        Initialize the process plist to `plist`.

    The original argument list, modified with the actual connection information, is available via the `process-contact` function.

Next: [Network Options](Network-Options.html), Up: [Low-Level Network](Low_002dLevel-Network.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
