

#### 2.5.1 Buffer Type

A *buffer* is an object that holds text that can be edited (see [Buffers](Buffers.html)). Most buffers hold the contents of a disk file (see [Files](Files.html)) so they can be edited, but some are used for other purposes. Most buffers are also meant to be seen by the user, and therefore displayed, at some time, in a window (see [Windows](Windows.html)). But a buffer need not be displayed in any window. Each buffer has a designated position called *point* (see [Positions](Positions.html)); most editing commands act on the contents of the current buffer in the neighborhood of point. At any time, one buffer is the *current buffer*.

The contents of a buffer are much like a string, but buffers are not used like strings in Emacs Lisp, and the available operations are different. For example, you can insert text efficiently into an existing buffer, altering the buffer’s contents, whereas inserting text into a string requires concatenating substrings, and the result is an entirely new string object.

Many of the standard Emacs functions manipulate or test the characters in the current buffer; a whole chapter in this manual is devoted to describing these functions (see [Text](Text.html)).

Several other data structures are associated with each buffer:

a local syntax table (see [Syntax Tables](Syntax-Tables.html));

a local keymap (see [Keymaps](Keymaps.html)); and,

a list of buffer-local variable bindings (see [Buffer-Local Variables](Buffer_002dLocal-Variables.html)).

overlays (see [Overlays](Overlays.html)).

text properties for the text in the buffer (see [Text Properties](Text-Properties.html)).

The local keymap and variable list contain entries that individually override global bindings or values. These are used to customize the behavior of programs in different buffers, without actually changing the programs.

A buffer may be *indirect*, which means it shares the text of another buffer, but presents it differently. See [Indirect Buffers](Indirect-Buffers.html).

Buffers have no read syntax. They print in hash notation, showing the buffer name.

```lisp
(current-buffer)
     ⇒ #<buffer objects.texi>
```
