

### 25.9 File Names

Files are generally referred to by their names, in Emacs as elsewhere. File names in Emacs are represented as strings. The functions that operate on a file all expect a file name argument.

In addition to operating on files themselves, Emacs Lisp programs often need to operate on file names; i.e., to take them apart and to use part of a name to construct related file names. This section describes how to manipulate file names.

The functions in this section do not actually access files, so they can operate on file names that do not refer to an existing file or directory.

On MS-DOS and MS-Windows, these functions (like the function that actually operate on files) accept MS-DOS or MS-Windows file-name syntax, where backslashes separate the components, as well as POSIX syntax; but they always return POSIX syntax. This enables Lisp programs to specify file names in POSIX syntax and work properly on all systems without change.[15](#FOOT15)

|                                                     |    |                                                                                         |
| :-------------------------------------------------- | -- | :-------------------------------------------------------------------------------------- |
| • [File Name Components](File-Name-Components.html) |    | The directory part of a file name, and the rest.                                        |
| • [Relative File Names](Relative-File-Names.html)   |    | Some file names are relative to a current directory.                                    |
| • [Directory Names](Directory-Names.html)           |    | A directory’s name as a directory is different from its name as a file.                 |
| • [File Name Expansion](File-Name-Expansion.html)   |    | Converting relative file names to absolute ones.                                        |
| • [Unique File Names](Unique-File-Names.html)       |    | Generating names for temporary files.                                                   |
| • [File Name Completion](File-Name-Completion.html) |    | Finding the completions for a given file name.                                          |
| • [Standard File Names](Standard-File-Names.html)   |    | If your package uses a fixed file name, how to handle various operating systems simply. |

***

#### Footnotes

##### [(15)](#DOCF15)

In MS-Windows versions of Emacs compiled for the Cygwin environment, you can use the functions `cygwin-convert-file-name-to-windows` and `cygwin-convert-file-name-from-windows` to convert between the two file-name syntaxes.
