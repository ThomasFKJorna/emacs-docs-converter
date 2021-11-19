

Next: [Network Servers](Network-Servers.html), Previous: [Transaction Queues](Transaction-Queues.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.14 Network Connections

Emacs Lisp programs can open stream (TCP) and datagram (UDP) network connections (see [Datagrams](Datagrams.html)) to other processes on the same machine or other machines. A network connection is handled by Lisp much like a subprocess, and is represented by a process object. However, the process you are communicating with is not a child of the Emacs process, has no process ID, and you can’t kill it or send it signals. All you can do is send and receive data. `delete-process` closes the connection, but does not kill the program at the other end; that program must decide what to do about closure of the connection.

Lisp programs can listen for connections by creating network servers. A network server is also represented by a kind of process object, but unlike a network connection, the network server never transfers data itself. When it receives a connection request, it creates a new network connection to represent the connection just made. (The network connection inherits certain information, including the process plist, from the server.) The network server then goes back to listening for more connection requests.

Network connections and servers are created by calling `make-network-process` with an argument list consisting of keyword/argument pairs, for example `:server t` to create a server process, or `:type 'datagram` to create a datagram connection. See [Low-Level Network](Low_002dLevel-Network.html), for details. You can also use the `open-network-stream` function described below.

To distinguish the different types of processes, the `process-type` function returns the symbol `network` for a network connection or server, `serial` for a serial port connection, `pipe` for a pipe connection, or `real` for a real subprocess.

The `process-status` function returns `open`, `closed`, `connect`, `stop`, or `failed` for network connections. For a network server, the status is always `listen`. Except for `stop`, none of those values is possible for a real subprocess. See [Process Information](Process-Information.html).

You can stop and resume operation of a network process by calling `stop-process` and `continue-process`. For a server process, being stopped means not accepting new connections. (Up to 5 connection requests will be queued for when you resume the server; you can increase this limit, unless it is imposed by the operating system—see the `:server` keyword of `make-network-process`, [Network Processes](Network-Processes.html).) For a network stream connection, being stopped means not processing input (any arriving input waits until you resume the connection). For a datagram connection, some number of packets may be queued but input may be lost. You can use the function `process-command` to determine whether a network connection or server is stopped; a non-`nil` value means yes.

Emacs can create encrypted network connections, using either built-in or external support. The built-in support uses the GnuTLS Transport Layer Security Library; see [the GnuTLS project page](https://www.gnu.org/software/gnutls/). If your Emacs was compiled with GnuTLS support, the function `gnutls-available-p` is defined and returns non-`nil`. For more details, see [Overview](../emacs-gnutls/index.html#Top) in The Emacs-GnuTLS manual. The external support uses the `starttls.el` library, which requires a helper utility such as `gnutls-cli` to be installed on the system. The `open-network-stream` function can transparently handle the details of creating encrypted connections for you, using whatever support is available.

*   Function: **open-network-stream** *name buffer host service \&rest parameters*

    This function opens a TCP connection, with optional encryption, and returns a process object that represents the connection.

    The `name` argument specifies the name for the process object. It is modified as necessary to make it unique.

    The `buffer` argument is the buffer to associate with the connection. Output from the connection is inserted in the buffer, unless you specify your own filter function to handle the output. If `buffer` is `nil`, it means that the connection is not associated with any buffer.

    The arguments `host` and `service` specify where to connect to; `host` is the host name (a string), and `service` is the name of a defined network service (a string) or a port number (an integer like `80` or an integer string like `"80"`).

    The remaining arguments `parameters` are keyword/argument pairs that are mainly relevant to encrypted connections:

    *   `:nowait boolean`

        If non-`nil`, try to make an asynchronous connection.

    *   `:type type`

        The type of connection. Options are:

        *   `plain`

            An ordinary, unencrypted connection.

        *   *   `tls`
            *   `ssl`

            A TLS (Transport Layer Security) connection.

        *   *   `nil`
            *   `network`

            Start with a plain connection, and if parameters ‘`:success`’ and ‘`:capability-command`’ are supplied, try to upgrade to an encrypted connection via STARTTLS. If that fails, retain the unencrypted connection.

        *   `starttls`

            As for `nil`, but if STARTTLS fails drop the connection.

        *   `shell`

            A shell connection.

    *   `:always-query-capabilities boolean`

        If non-`nil`, always ask for the server’s capabilities, even when doing a ‘`plain`’ connection.

    *   `:capability-command capability-command`

        Command string to query the host capabilities.

    *   *   `:end-of-command regexp`
        *   `:end-of-capability regexp`

        Regular expression matching the end of a command, or the end of the command `capability-command`. The latter defaults to the former.

    *   `:starttls-function function`

        Function of one argument (the response to `capability-command`), which returns either `nil`, or the command to activate STARTTLS if supported.

    *   `:success regexp`

        Regular expression matching a successful STARTTLS negotiation.

    *   `:use-starttls-if-possible boolean`

        If non-`nil`, do opportunistic STARTTLS upgrades even if Emacs doesn’t have built-in TLS support.

    *   `:warn-unless-encrypted boolean`

        If non-`nil`, and `:return-value` is also non-`nil`, Emacs will warn if the connection isn’t encrypted. This is useful for protocols like IMAP and the like, where most users would expect the network traffic to be encrypted.

    *   `:client-certificate list-or-t`

        Either a list of the form `(key-file cert-file)`, naming the certificate key file and certificate file itself, or `t`, meaning to query `auth-source` for this information (see [auth-source](../auth/Help-for-users.html#Help-for-users) in Emacs auth-source Library). Only used for TLS or STARTTLS. To enable automatic queries of `auth-source` when `:client-certificate` is not specified customize `network-stream-use-client-certificates` to t.

    *   `:return-list cons-or-nil`

        The return value of this function. If omitted or `nil`, return a process object. Otherwise, a cons of the form `(process-object . plist)`, where `plist` has keywords:

        *   `:greeting string-or-nil`

            If non-`nil`, the greeting string returned by the host.

        *   `:capabilities string-or-nil`

            If non-`nil`, the host’s capability string.

        *   `:type symbol`

            The connection type: ‘`plain`’ or ‘`tls`’.

    *   `:shell-command string-or-nil`

        If the connection `type` is `shell`, this parameter will be interpreted as a format-spec string that will be executed to make the connection. The specs available are ‘`%s`’ for the host name and ‘`%p`’ for the port number. For instance, if you want to first ssh to ‘`gateway`’ before making a plain connection, then this parameter could be something like ‘`ssh gateway nc %s %p`’.

Next: [Network Servers](Network-Servers.html), Previous: [Transaction Queues](Transaction-Queues.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
