

#### 21.7.1 Keyboard Events

There are two kinds of input you can get from the keyboard: ordinary keys, and function keys. Ordinary keys correspond to (possibly modified) characters; the events they generate are represented in Lisp as characters. The event type of a *character event* is the character itself (an integer), which might have some modifier bits set; see [Classifying Events](Classifying-Events.html).

An input character event consists of a *basic code* between 0 and 524287, plus any or all of these *modifier bits*:

meta

The 2\*\*27 bit in the character code indicates a character typed with the meta key held down.

control

The 2\*\*26 bit in the character code indicates a non-ASCII control character.

ASCII control characters such as `C-a` have special basic codes of their own, so Emacs needs no special bit to indicate them. Thus, the code for `C-a` is just 1.

But if you type a control combination not in ASCII, such as `%` with the control key, the numeric value you get is the code for `%` plus 2\*\*26 (assuming the terminal supports non-ASCII control characters), i.e. with the 27th bit set.

shift

The 2\*\*25 bit (the 26th bit) in the character event code indicates an ASCII control character typed with the shift key held down.

For letters, the basic code itself indicates upper versus lower case; for digits and punctuation, the shift key selects an entirely different character with a different basic code. In order to keep within the ASCII character set whenever possible, Emacs avoids using the 2\*\*25 bit for those character events.

However, ASCII provides no way to distinguish `C-A` from `C-a`, so Emacs uses the 2\*\*25 bit in `C-A` and not in `C-a`.

hyper

The 2\*\*24 bit in the character event code indicates a character typed with the hyper key held down.

super

The 2\*\*23 bit in the character event code indicates a character typed with the super key held down.

alt

The 2\*\*22 bit in the character event code indicates a character typed with the alt key held down. (The key labeled `Alt` on most keyboards is actually treated as the meta key, not this.)

It is best to avoid mentioning specific bit numbers in your program. To test the modifier bits of a character, use the function `event-modifiers` (see [Classifying Events](Classifying-Events.html)). When making key bindings, you can use the read syntax for characters with modifier bits (‘`\C-`’, ‘`\M-`’, and so on). For making key bindings with `define-key`, you can use lists such as `(control hyper ?x)` to specify the characters (see [Changing Key Bindings](Changing-Key-Bindings.html)). The function `event-convert-list` converts such a list into an event type (see [Classifying Events](Classifying-Events.html)).
