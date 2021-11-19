

Next: [Saving Buffers](Saving-Buffers.html), Up: [Files](Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 25.1 Visiting Files

Visiting a file means reading a file into a buffer. Once this is done, we say that the buffer is *visiting* that file, and call the file *the visited file* of the buffer.

A file and a buffer are two different things. A file is information recorded permanently in the computer (unless you delete it). A buffer, on the other hand, is information inside of Emacs that will vanish at the end of the editing session (or when you kill the buffer). When a buffer is visiting a file, it contains information copied from the file. The copy in the buffer is what you modify with editing commands. Changes to the buffer do not change the file; to make the changes permanent, you must *save* the buffer, which means copying the altered buffer contents back into the file.

Despite the distinction between files and buffers, people often refer to a file when they mean a buffer and vice-versa. Indeed, we say, “I am editing a file”, rather than, “I am editing a buffer that I will soon save as a file of the same name”. Humans do not usually need to make the distinction explicit. When dealing with a computer program, however, it is good to keep the distinction in mind.

|                                                           |    |                                             |
| :-------------------------------------------------------- | -- | :------------------------------------------ |
| • [Visiting Functions](Visiting-Functions.html)           |    | The usual interface functions for visiting. |
| • [Subroutines of Visiting](Subroutines-of-Visiting.html) |    | Lower-level subroutines that they use.      |
