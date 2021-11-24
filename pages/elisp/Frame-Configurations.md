

### 29.13 Frame Configurations

A *frame configuration* records the current arrangement of frames, all their properties, and the window configuration of each one. (See [Window Configurations](Window-Configurations.html).)

### Function: **current-frame-configuration**

This function returns a frame configuration list that describes the current arrangement of frames and their contents.

### Function: **set-frame-configuration** *configuration \&optional nodelete*

This function restores the state of frames described in `configuration`. However, this function does not restore deleted frames.

Ordinarily, this function deletes all existing frames not listed in `configuration`. But if `nodelete` is non-`nil`, the unwanted frames are iconified instead.
