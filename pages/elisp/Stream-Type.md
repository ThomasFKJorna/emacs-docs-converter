

#### 2.5.12 Stream Type

A *stream* is an object that can be used as a source or sink for characters—either to supply characters for input or to accept them as output. Many different types can be used this way: markers, buffers, strings, and functions. Most often, input streams (character sources) obtain characters from the keyboard, a buffer, or a file, and output streams (character sinks) send characters to a buffer, such as a `*Help*` buffer, or to the echo area.

The object `nil`, in addition to its other meanings, may be used as a stream. It stands for the value of the variable `standard-input` or `standard-output`. Also, the object `t` as a stream specifies input using the minibuffer (see [Minibuffers](Minibuffers.html)) or output in the echo area (see [The Echo Area](The-Echo-Area.html)).

Streams have no special printed representation or read syntax, and print as whatever primitive type they are.

See [Read and Print](Read-and-Print.html), for a description of functions related to streams, including parsing and printing functions.
