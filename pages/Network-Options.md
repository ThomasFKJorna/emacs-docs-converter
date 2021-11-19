

Next: [Network Feature Testing](Network-Feature-Testing.html), Previous: [Network Processes](Network-Processes.html), Up: [Low-Level Network](Low_002dLevel-Network.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 38.17.2 Network Options

The following network options can be specified when you create a network process. Except for `:reuseaddr`, you can also set or modify these options later, using `set-network-process-option`.

For a server process, the options specified with `make-network-process` are not inherited by the client connections, so you will need to set the necessary options for each child connection as it is created.

*   :bindtodevice `device-name`

    If `device-name` is a non-empty string identifying a network interface name (see `network-interface-list`), only handle packets received on that interface. If `device-name` is `nil` (the default), handle packets received on any interface.

    Using this option may require special privileges on some systems.

*   :broadcast `broadcast-flag`

    If `broadcast-flag` is non-`nil` for a datagram process, the process will receive datagram packet sent to a broadcast address, and be able to send packets to a broadcast address. This is ignored for a stream connection.

*   :dontroute `dontroute-flag`

    If `dontroute-flag` is non-`nil`, the process can only send to hosts on the same network as the local host.

*   :keepalive `keepalive-flag`

    If `keepalive-flag` is non-`nil` for a stream connection, enable exchange of low-level keep-alive messages.

*   :linger `linger-arg`

    If `linger-arg` is non-`nil`, wait for successful transmission of all queued packets on the connection before it is deleted (see `delete-process`). If `linger-arg` is an integer, it specifies the maximum time in seconds to wait for queued packets to be sent before closing the connection. The default is `nil`, which means to discard unsent queued packets when the process is deleted.

*   :oobinline `oobinline-flag`

    If `oobinline-flag` is non-`nil` for a stream connection, receive out-of-band data in the normal data stream. Otherwise, ignore out-of-band data.

*   :priority `priority`

    Set the priority for packets sent on this connection to the integer `priority`. The interpretation of this number is protocol specific; such as setting the TOS (type of service) field on IP packets sent on this connection. It may also have system dependent effects, such as selecting a specific output queue on the network interface.

*   :reuseaddr `reuseaddr-flag`

    If `reuseaddr-flag` is non-`nil` (the default) for a stream server process, allow this server to reuse a specific port number (see `:service`), unless another process on this host is already listening on that port. If `reuseaddr-flag` is `nil`, there may be a period of time after the last use of that port (by any process on the host) where it is not possible to make a new server on that port.

<!---->

*   Function: **set-network-process-option** *process option value \&optional no-error*

    This function sets or modifies a network option for network process `process`. The accepted options and values are as for `make-network-process`. If `no-error` is non-`nil`, this function returns `nil` instead of signaling an error if `option` is not a supported option. If the function successfully completes, it returns `t`.

    The current setting of an option is available via the `process-contact` function.

Next: [Network Feature Testing](Network-Feature-Testing.html), Previous: [Network Processes](Network-Processes.html), Up: [Low-Level Network](Low_002dLevel-Network.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
