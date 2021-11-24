

### 29.4 Frame Parameters

A frame has many parameters that control its appearance and behavior. Just what parameters a frame has depends on what display mechanism it uses.

Frame parameters exist mostly for the sake of graphical displays. Most frame parameters have no effect when applied to a frame on a text terminal; only the `height`, `width`, `name`, `title`, `menu-bar-lines`, `buffer-list` and `buffer-predicate` parameters do something special. If the terminal supports colors, the parameters `foreground-color`, `background-color`, `background-mode` and `display-type` are also meaningful. If the terminal supports frame transparency, the parameter `alpha` is also meaningful.

By default, frame parameters are saved and restored by the desktop library functions (see [Desktop Save Mode](Desktop-Save-Mode.html)) when the variable `desktop-restore-frames` is non-`nil`. It’s the responsibility of applications that their parameters are included in `frameset-persistent-filter-alist` to avoid that they get meaningless or even harmful values in restored sessions.

|                                                           |    |                                                    |
| :-------------------------------------------------------- | -- | :------------------------------------------------- |
| • [Parameter Access](Parameter-Access.html)               |    | How to change a frame’s parameters.                |
| • [Initial Parameters](Initial-Parameters.html)           |    | Specifying frame parameters when you make a frame. |
| • [Window Frame Parameters](Window-Frame-Parameters.html) |    | List of frame parameters for window systems.       |
| • [Geometry](Geometry.html)                               |    | Parsing geometry specifications.                   |
