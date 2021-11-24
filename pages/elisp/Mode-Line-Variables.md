

#### 23.4.4 Variables Used in the Mode Line

This section describes variables incorporated by the standard value of `mode-line-format` into the text of the mode line. There is nothing inherently special about these variables; any other variables could have the same effects on the mode line if the value of `mode-line-format` is changed to use them. However, various parts of Emacs set these variables on the understanding that they will control parts of the mode line; therefore, practically speaking, it is essential for the mode line to use them. Also see [Optional Mode Line](https://www.gnu.org/software/emacs/manual/html_node/emacs/Optional-Mode-Line.html#Optional-Mode-Line) in The GNU Emacs Manual.

### Variable: **mode-line-mule-info**

This variable holds the value of the mode line construct that displays information about the language environment, buffer coding system, and current input method. See [Non-ASCII Characters](Non_002dASCII-Characters.html).

### Variable: **mode-line-modified**

This variable holds the value of the mode line construct that displays whether the current buffer is modified. Its default value displays ‘`**`’ if the buffer is modified, ‘`--`’ if the buffer is not modified, ‘`%%`’ if the buffer is read only, and ‘`%*`’ if the buffer is read only and modified.

Changing this variable does not force an update of the mode line.

### Variable: **mode-line-frame-identification**

This variable identifies the current frame. Its default value displays `" "` if you are using a window system which can show multiple frames, or `"-%F "` on an ordinary terminal which shows only one frame at a time.

### Variable: **mode-line-buffer-identification**

This variable identifies the buffer being displayed in the window. Its default value displays the buffer name, padded with spaces to at least 12 columns.

### Variable: **mode-line-position**

This variable indicates the position in the buffer. Its default value displays the buffer percentage and, optionally, the buffer size, the line number and the column number.

### User Option: **mode-line-percent-position**

This option is used in `mode-line-position`. Its value specifies both the buffer percentage to display (one of `nil`, `"%o"`, `"%p"`, `"%P"` or `"%q"`, see [%-Constructs](_0025_002dConstructs.html)) and a width to space-fill or truncate to. You are recommended to set this option with the `customize-variable` facility.

### Variable: **vc-mode**

The variable `vc-mode`, buffer-local in each buffer, records whether the buffer’s visited file is maintained with version control, and, if so, which kind. Its value is a string that appears in the mode line, or `nil` for no version control.

### Variable: **mode-line-modes**

This variable displays the buffer’s major and minor modes. Its default value also displays the recursive editing level, information on the process status, and whether narrowing is in effect.

### Variable: **mode-line-remote**

This variable is used to show whether `default-directory` for the current buffer is remote.

### Variable: **mode-line-client**

This variable is used to identify `emacsclient` frames.

The following three variables are used in `mode-line-modes`:

### Variable: **mode-name**

This buffer-local variable holds the “pretty” name of the current buffer’s major mode. Each major mode should set this variable so that the mode name will appear in the mode line. The value does not have to be a string, but can use any of the data types valid in a mode-line construct (see [Mode Line Data](Mode-Line-Data.html)). To compute the string that will identify the mode name in the mode line, use `format-mode-line` (see [Emulating Mode Line](Emulating-Mode-Line.html)).

### Variable: **mode-line-process**

This buffer-local variable contains the mode line information on process status in modes used for communicating with subprocesses. It is displayed immediately following the major mode name, with no intervening space. For example, its value in the `*shell*` buffer is `(":%s")`, which allows the shell to display its status along with the major mode as: ‘`(Shell:run)`’. Normally this variable is `nil`.

### Variable: **mode-line-front-space**

This variable is displayed at the front of the mode line. By default, this construct is displayed right at the beginning of the mode line, except that if there is a memory-full message, it is displayed first.

### Variable: **mode-line-end-spaces**

This variable is displayed at the end of the mode line.

### Variable: **mode-line-misc-info**

Mode line construct for miscellaneous information. By default, this shows the information specified by `global-mode-string`.

### Variable: **minor-mode-alist**

This variable holds an association list whose elements specify how the mode line should indicate that a minor mode is active. Each element of the `minor-mode-alist` should be a two-element list:

```lisp
(minor-mode-variable mode-line-string)
```

More generally, `mode-line-string` can be any mode line construct. It appears in the mode line when the value of `minor-mode-variable` is non-`nil`, and not otherwise. These strings should begin with spaces so that they don’t run together. Conventionally, the `minor-mode-variable` for a specific mode is set to a non-`nil` value when that minor mode is activated.

`minor-mode-alist` itself is not buffer-local. Each variable mentioned in the alist should be buffer-local if its minor mode can be enabled separately in each buffer.

### Variable: **global-mode-string**

This variable holds a mode line construct that, by default, appears in the mode line just after the `which-function-mode` minor mode if set, else after `mode-line-modes`. The command `display-time` sets `global-mode-string` to refer to the variable `display-time-string`, which holds a string containing the time and load information.

The ‘`%M`’ construct substitutes the value of `global-mode-string`, but that is obsolete, since the variable is included in the mode line from `mode-line-format`.

Here is a simplified version of the default value of `mode-line-format`. The real default value also specifies addition of text properties.

```lisp
("-"
 mode-line-mule-info
 mode-line-modified
 mode-line-frame-identification
 mode-line-buffer-identification
```

```lisp
 "   "
 mode-line-position
 (vc-mode vc-mode)
 "   "
```

```lisp
 mode-line-modes
 (which-function-mode ("" which-func-format "--"))
 (global-mode-string ("--" global-mode-string))
 "-%-")
```
