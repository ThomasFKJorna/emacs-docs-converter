

#### 33.10.8 Terminal I/O Encoding

Emacs can use coding systems to decode keyboard input and encode terminal output. This is useful for terminals that transmit or display text using a particular encoding, such as Latin-1. Emacs does not set `last-coding-system-used` when encoding or decoding terminal I/O.

### Function: **keyboard-coding-system** *\&optional terminal*

This function returns the coding system used for decoding keyboard input from `terminal`. A value of `no-conversion` means no decoding is done. If `terminal` is omitted or `nil`, it means the selected frame’s terminal. See [Multiple Terminals](Multiple-Terminals.html).

### Command: **set-keyboard-coding-system** *coding-system \&optional terminal*

This command specifies `coding-system` as the coding system to use for decoding keyboard input from `terminal`. If `coding-system` is `nil`, that means not to decode keyboard input. If `terminal` is a frame, it means that frame’s terminal; if it is `nil`, that means the currently selected frame’s terminal. See [Multiple Terminals](Multiple-Terminals.html).

### Function: **terminal-coding-system** *\&optional terminal*

This function returns the coding system that is in use for encoding terminal output from `terminal`. A value of `no-conversion` means no encoding is done. If `terminal` is a frame, it means that frame’s terminal; if it is `nil`, that means the currently selected frame’s terminal.

### Command: **set-terminal-coding-system** *coding-system \&optional terminal*

This command specifies `coding-system` as the coding system to use for encoding terminal output from `terminal`. If `coding-system` is `nil`, that means not to encode terminal output. If `terminal` is a frame, it means that frame’s terminal; if it is `nil`, that means the currently selected frame’s terminal.
