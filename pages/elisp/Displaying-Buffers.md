

### 28.13 Displaying a Buffer in a Suitable Window

This section describes lower-level functions Emacs uses to find or create a window for displaying a specified buffer. The common workhorse of these functions is `display-buffer` which eventually handles all incoming requests for buffer display (see [Choosing Window](Choosing-Window.html)).

`display-buffer` delegates the task of finding a suitable window to so-called action functions (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)). First, `display-buffer` compiles a so-called action alist—a special association list that action functions can use to fine-tune their behavior. Then it passes that alist on to each action function it calls (see [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)).

The behavior of `display-buffer` is highly customizable. To understand how customizations are used in practice, you may wish to study examples illustrating the order of precedence which `display-buffer` uses to call action functions (see [Precedence of Action Functions](Precedence-of-Action-Functions.html)). To avoid conflicts between Lisp programs calling `display-buffer` and user customizations of its behavior, it may make sense to follow a number of guidelines which are sketched in the final part of this section (see [The Zen of Buffer Display](The-Zen-of-Buffer-Display.html)).

|                                                                           |    |                                                         |
| :------------------------------------------------------------------------ | -- | :------------------------------------------------------ |
| • [Choosing Window](Choosing-Window.html)                                 |    | How to choose a window for displaying a buffer.         |
| • [Buffer Display Action Functions](Buffer-Display-Action-Functions.html) |    | Support functions for buffer display.                   |
| • [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)       |    | Alists for fine-tuning buffer display.                  |
| • [Choosing Window Options](Choosing-Window-Options.html)                 |    | Extra options affecting how buffers are displayed.      |
| • [Precedence of Action Functions](Precedence-of-Action-Functions.html)   |    | Examples to explain the precedence of action functions. |
| • [The Zen of Buffer Display](The-Zen-of-Buffer-Display.html)             |    | How to avoid that buffers get lost in between windows.  |
