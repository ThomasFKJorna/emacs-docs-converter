

### 33.10 Coding Systems

When Emacs reads or writes a file, and when Emacs sends text to a subprocess or receives text from a subprocess, it normally performs character code conversion and end-of-line conversion as specified by a particular *coding system*.

How to define a coding system is an arcane matter, and is not documented here.

|                                                                     |    |                                                                    |
| :------------------------------------------------------------------ | -- | :----------------------------------------------------------------- |
| • [Coding System Basics](Coding-System-Basics.html)                 |    | Basic concepts.                                                    |
| • [Encoding and I/O](Encoding-and-I_002fO.html)                     |    | How file I/O functions handle coding systems.                      |
| • [Lisp and Coding Systems](Lisp-and-Coding-Systems.html)           |    | Functions to operate on coding system names.                       |
| • [User-Chosen Coding Systems](User_002dChosen-Coding-Systems.html) |    | Asking the user to choose a coding system.                         |
| • [Default Coding Systems](Default-Coding-Systems.html)             |    | Controlling the default choices.                                   |
| • [Specifying Coding Systems](Specifying-Coding-Systems.html)       |    | Requesting a particular coding system for a single file operation. |
| • [Explicit Encoding](Explicit-Encoding.html)                       |    | Encoding or decoding text without doing I/O.                       |
| • [Terminal I/O Encoding](Terminal-I_002fO-Encoding.html)           |    | Use of encoding for terminal I/O.                                  |
