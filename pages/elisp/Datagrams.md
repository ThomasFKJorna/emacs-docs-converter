

### 38.16 Datagrams

A *datagram* connection communicates with individual packets rather than streams of data. Each call to `process-send` sends one datagram packet (see [Input to Processes](Input-to-Processes.html)), and each datagram received results in one call to the filter function.

The datagram connection doesn’t have to talk with the same remote peer all the time. It has a *remote peer address* which specifies where to send datagrams to. Each time an incoming datagram is passed to the filter function, the peer address is set to the address that datagram came from; that way, if the filter function sends a datagram, it will go back to that place. You can specify the remote peer address when you create the datagram connection using the `:remote` keyword. You can change it later on by calling `set-process-datagram-address`.

### Function: **process-datagram-address** *process*

If `process` is a datagram connection or server, this function returns its remote peer address.

### Function: **set-process-datagram-address** *process address*

If `process` is a datagram connection or server, this function sets its remote peer address to `address`.
