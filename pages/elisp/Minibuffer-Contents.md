

### 20.12 Minibuffer Contents

These functions access the minibuffer prompt and contents.

### Function: **minibuffer-prompt**

This function returns the prompt string of the currently active minibuffer. If no minibuffer is active, it returns `nil`.

### Function: **minibuffer-prompt-end**

This function returns the current position of the end of the minibuffer prompt, if a minibuffer is current. Otherwise, it returns the minimum valid buffer position.

### Function: **minibuffer-prompt-width**

This function returns the current display-width of the minibuffer prompt, if a minibuffer is current. Otherwise, it returns zero.

### Function: **minibuffer-contents**

This function returns the editable contents of the minibuffer (that is, everything except the prompt) as a string, if a minibuffer is current. Otherwise, it returns the entire contents of the current buffer.

### Function: **minibuffer-contents-no-properties**

This is like `minibuffer-contents`, except that it does not copy text properties, just the characters themselves. See [Text Properties](Text-Properties.html).

### Command: **delete-minibuffer-contents**

This command erases the editable contents of the minibuffer (that is, everything except the prompt), if a minibuffer is current. Otherwise, it erases the entire current buffer.
