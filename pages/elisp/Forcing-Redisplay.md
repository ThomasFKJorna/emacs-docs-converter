

### 39.2 Forcing Redisplay

Emacs normally tries to redisplay the screen whenever it waits for input. With the following function, you can request an immediate attempt to redisplay, in the middle of Lisp code, without actually waiting for input.

### Function: **redisplay** *\&optional force*

This function tries immediately to redisplay. The optional argument `force`, if non-`nil`, forces the redisplay to be performed, instead of being preempted if input is pending.

The function returns `t` if it actually tried to redisplay, and `nil` otherwise. A value of `t` does not mean that redisplay proceeded to completion; it could have been preempted by newly arriving input.

Although `redisplay` tries immediately to redisplay, it does not change how Emacs decides which parts of its frame(s) to redisplay. By contrast, the following function adds certain windows to the pending redisplay work (as if their contents had completely changed), but does not immediately try to perform redisplay.

### Function: **force-window-update** *\&optional object*

This function forces some or all windows to be updated the next time Emacs does a redisplay. If `object` is a window, that window is to be updated. If `object` is a buffer or buffer name, all windows displaying that buffer are to be updated. If `object` is `nil` (or omitted), all windows are to be updated.

This function does not do a redisplay immediately; Emacs does that as it waits for input, or when the function `redisplay` is called.

### Variable: **pre-redisplay-function**

A function run just before redisplay. It is called with one argument, the set of windows to be redisplayed. The set can be `nil`, meaning only the selected window, or `t`, meaning all the windows.

### Variable: **pre-redisplay-functions**

This hook is run just before redisplay. It is called once in each window that is about to be redisplayed, with `current-buffer` set to the buffer displayed in that window.
