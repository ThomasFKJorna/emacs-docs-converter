

#### 38.17.3 Testing Availability of Network Features

To test for the availability of a given network feature, use `featurep` like this:

```lisp
(featurep 'make-network-process '(keyword value))
```

The result of this form is `t` if it works to specify `keyword` with value `value` in `make-network-process`. Here are some of the `keyword`—`value` pairs you can test in this way.

### `(:nowait t)`

Non-`nil` if non-blocking connect is supported.

### `(:type datagram)`

Non-`nil` if datagrams are supported.

### `(:family local)`

Non-`nil` if local (a.k.a. “UNIX domain”) sockets are supported.

### `(:family ipv6)`

Non-`nil` if IPv6 is supported.

### `(:service t)`

Non-`nil` if the system can select the port for a server.

To test for the availability of a given network option, use `featurep` like this:

```lisp
(featurep 'make-network-process 'keyword)
```

The accepted `keyword` values are `:bindtodevice`, etc. For the complete list, see [Network Options](Network-Options.html). This form returns non-`nil` if that particular network option is supported by `make-network-process` (or `set-network-process-option`).
