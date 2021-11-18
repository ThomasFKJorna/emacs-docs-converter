<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Byte Packing](Byte-Packing.html), Previous: [Misc Network](Misc-Network.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.19 Communicating with Serial Ports

Emacs can communicate with serial ports. For interactive use, `M-x serial-term` opens a terminal window. In a Lisp program, `make-serial-process` creates a process object.

The serial port can be configured at run-time, without having to close and re-open it. The function `serial-process-configure` lets you change the speed, bytesize, and other parameters. In a terminal window created by `serial-term`, you can click on the mode line for configuration.

A serial connection is represented by a process object, which can be used in a similar way to a subprocess or network process. You can send and receive data, and configure the serial port. A serial process object has no process ID, however, and you can’t send signals to it, and the status codes are different from other types of processes. `delete-process` on the process object or `kill-buffer` on the process buffer close the connection, but this does not affect the device connected to the serial port.

The function `process-type` returns the symbol `serial` for a process object representing a serial port connection.

Serial ports are available on GNU/Linux, Unix, and MS Windows systems.

*   Command: **serial-term** *port speed \&optional line-mode*

    Start a terminal-emulator for a serial port in a new buffer. `port` is the name of the serial port to connect to. For example, this could be `/dev/ttyS0` on Unix. On MS Windows, this could be `COM1`, or `\\.\COM10` (double the backslashes in Lisp strings).

    `speed` is the speed of the serial port in bits per second. 9600 is a common value. The buffer is in Term mode; see [Term Mode](https://www.gnu.org/software/emacs/manual/html_node/emacs/Term-Mode.html#Term-Mode) in The GNU Emacs Manual, for the commands to use in that buffer. You can change the speed and the configuration in the mode line menu. If `line-mode` is non-`nil`, `term-line-mode` is used; otherwise `term-raw-mode` is used.

<!---->

*   Function: **make-serial-process** *\&rest args*

    This function creates a process and a buffer. Arguments are specified as keyword/argument pairs. Here’s the list of the meaningful keywords, with the first two (`port` and `speed`) being mandatory:

    *   `:port port`

        This is the name of the serial port. On Unix and GNU systems, this is a file name such as `/dev/ttyS0`. On Windows, this could be `COM1`, or `\\.\COM10` for ports higher than `COM9` (double the backslashes in Lisp strings).

    *   `:speed speed`

        The speed of the serial port in bits per second. This function calls `serial-process-configure` to handle the speed; see the following documentation of that function for more details.

    *   `:name name`

        The name of the process. If `name` is not given, `port` will serve as the process name as well.

    *   `:buffer buffer`

        The buffer to associate with the process. The value can be either a buffer or a string that names a buffer. Process output goes at the end of that buffer, unless you specify an output stream or filter function to handle the output. If `buffer` is not given, the process buffer’s name is taken from the value of the `:name` keyword.

    *   `:coding coding`

        If `coding` is a symbol, it specifies the coding system used for both reading and writing for this process. If `coding` is a cons `(decoding . encoding)`, `decoding` is used for reading, and `encoding` is used for writing. If not specified, the default is to determine the coding systems from the data itself.

    *   `:noquery query-flag`

        Initialize the process query flag to `query-flag`. See [Query Before Exit](Query-Before-Exit.html). The flags defaults to `nil` if unspecified.

    *   `:stop bool`

        Start process in the stopped state if `bool` is non-`nil`. In the stopped state, a serial process does not accept incoming data, but you can send outgoing data. The stopped state is cleared by `continue-process` and set by `stop-process`.

    *   `:filter filter`

        Install `filter` as the process filter.

    *   `:sentinel sentinel`

        Install `sentinel` as the process sentinel.

    *   `:plist plist`

        Install `plist` as the initial plist of the process.

    *   *   `:bytesize`
        *   `:parity`
        *   `:stopbits`
        *   `:flowcontrol`

        These are handled by `serial-process-configure`, which is called by `make-serial-process`.

    The original argument list, possibly modified by later configuration, is available via the function `process-contact`.

    Here is an example:

        (make-serial-process :port "/dev/ttyS0" :speed 9600)

<!---->

*   Function: **serial-process-configure** *\&rest args*

    This function configures a serial port connection. Arguments are specified as keyword/argument pairs. Attributes that are not given are re-initialized from the process’s current configuration (available via the function `process-contact`), or set to reasonable default values. The following arguments are defined:

    *   *   `:process process`
        *   `:name name`
        *   `:buffer buffer`
        *   `:port port`

        Any of these arguments can be given to identify the process that is to be configured. If none of these arguments is given, the current buffer’s process is used.

    *   `:speed speed`

        The speed of the serial port in bits per second, a.k.a. *baud rate*. The value can be any number, but most serial ports work only at a few defined values between 1200 and 115200, with 9600 being the most common value. If `speed` is `nil`, the function ignores all other arguments and does not configure the port. This may be useful for special serial ports such as Bluetooth-to-serial converters, which can only be configured through ‘`AT`’ commands sent through the connection. The value of `nil` for `speed` is valid only for connections that were already opened by a previous call to `make-serial-process` or `serial-term`.

    *   `:bytesize bytesize`

        The number of bits per byte, which can be 7 or 8. If `bytesize` is not given or `nil`, it defaults to 8.

    *   `:parity parity`

        The value can be `nil` (don’t use parity), the symbol `odd` (use odd parity), or the symbol `even` (use even parity). If `parity` is not given, it defaults to no parity.

    *   `:stopbits stopbits`

        The number of stopbits used to terminate a transmission of each byte. `stopbits` can be 1 or 2. If `stopbits` is not given or `nil`, it defaults to 1.

    *   `:flowcontrol flowcontrol`

        The type of flow control to use for this connection, which is either `nil` (don’t use flow control), the symbol `hw` (use RTS/CTS hardware flow control), or the symbol `sw` (use XON/XOFF software flow control). If `flowcontrol` is not given, it defaults to no flow control.

    Internally, `make-serial-process` calls `serial-process-configure` for the initial configuration of the serial port.

Next: [Byte Packing](Byte-Packing.html), Previous: [Misc Network](Misc-Network.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
