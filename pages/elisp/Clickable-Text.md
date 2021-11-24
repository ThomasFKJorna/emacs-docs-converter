

#### 32.19.8 Defining Clickable Text

*Clickable text* is text that can be clicked, with either the mouse or via a keyboard command, to produce some result. Many major modes use clickable text to implement textual hyper-links, or *links* for short.

The easiest way to insert and manipulate links is to use the `button` package. See [Buttons](Buttons.html). In this section, we will explain how to manually set up clickable text in a buffer, using text properties. For simplicity, we will refer to the clickable text as a *link*.

Implementing a link involves three separate steps: (1) indicating clickability when the mouse moves over the link; (2) making `RET` or `mouse-2` on that link do something; and (3) setting up a `follow-link` condition so that the link obeys `mouse-1-click-follows-link`.

To indicate clickability, add the `mouse-face` text property to the text of the link; then Emacs will highlight the link when the mouse moves over it. In addition, you should define a tooltip or echo area message, using the `help-echo` text property. See [Special Properties](Special-Properties.html). For instance, here is how Dired indicates that file names are clickable:

```lisp
 (if (dired-move-to-filename)
     (add-text-properties
       (point)
       (save-excursion
         (dired-move-to-end-of-filename)
         (point))
       '(mouse-face highlight
         help-echo "mouse-2: visit this file in other window")))
```

To make the link clickable, bind `RET` and `mouse-2` to commands that perform the desired action. Each command should check to see whether it was called on a link, and act accordingly. For instance, Dired’s major mode keymap binds `mouse-2` to the following command:

```lisp
(defun dired-mouse-find-file-other-window (event)
  "In Dired, visit the file or directory name you click on."
  (interactive "e")
  (let ((window (posn-window (event-end event)))
        (pos (posn-point (event-end event)))
        file)
    (if (not (windowp window))
        (error "No file chosen"))
    (with-current-buffer (window-buffer window)
      (goto-char pos)
      (setq file (dired-get-file-for-visit)))
    (if (file-directory-p file)
        (or (and (cdr dired-subdir-alist)
                 (dired-goto-subdir file))
            (progn
              (select-window window)
              (dired-other-window file)))
      (select-window window)
      (find-file-other-window (file-name-sans-versions file t)))))
```

This command uses the functions `posn-window` and `posn-point` to determine where the click occurred, and `dired-get-file-for-visit` to determine which file to visit.

Instead of binding the mouse command in a major mode keymap, you can bind it within the link text, using the `keymap` text property (see [Special Properties](Special-Properties.html)). For instance:

```lisp
(let ((map (make-sparse-keymap)))
  (define-key map [mouse-2] 'operate-this-button)
  (put-text-property link-start link-end 'keymap map))
```

With this method, you can easily define different commands for different links. Furthermore, the global definition of `RET` and `mouse-2` remain available for the rest of the text in the buffer.

The basic Emacs command for clicking on links is `mouse-2`. However, for compatibility with other graphical applications, Emacs also recognizes `mouse-1` clicks on links, provided the user clicks on the link quickly without moving the mouse. This behavior is controlled by the user option `mouse-1-click-follows-link`. See [Mouse References](https://www.gnu.org/software/emacs/manual/html_node/emacs/Mouse-References.html#Mouse-References) in The GNU Emacs Manual.

To set up the link so that it obeys `mouse-1-click-follows-link`, you must either (1) apply a `follow-link` text or overlay property to the link text, or (2) bind the `follow-link` event to a keymap (which can be a major mode keymap or a local keymap specified via the `keymap` text property). The value of the `follow-link` property, or the binding for the `follow-link` event, acts as a condition for the link action. This condition tells Emacs two things: the circumstances under which a `mouse-1` click should be regarded as occurring inside the link, and how to compute an action code that says what to translate the `mouse-1` click into. The link action condition can be one of the following:

`mouse-face`

If the condition is the symbol `mouse-face`, a position is inside a link if there is a non-`nil` `mouse-face` property at that position. The action code is always `t`.

For example, here is how Info mode handles `mouse-1`:

```lisp
(define-key Info-mode-map [follow-link] 'mouse-face)
```

a function

If the condition is a function, `func`, then a position `pos` is inside a link if `(func pos)` evaluates to non-`nil`. The value returned by `func` serves as the action code.

For example, here is how pcvs enables `mouse-1` to follow links on file names only:

```lisp
(define-key map [follow-link]
  (lambda (pos)
    (eq (get-char-property pos 'face) 'cvs-filename-face)))
```

anything else

If the condition value is anything else, then the position is inside a link and the condition itself is the action code. Clearly, you should specify this kind of condition only when applying the condition via a text or property overlay on the link text (so that it does not apply to the entire buffer).

The action code tells `mouse-1` how to follow the link:

a string or vector

If the action code is a string or vector, the `mouse-1` event is translated into the first element of the string or vector; i.e., the action of the `mouse-1` click is the local or global binding of that character or symbol. Thus, if the action code is `"foo"`, `mouse-1` translates into `f`. If it is `[foo]`, `mouse-1` translates into `foo`.

anything else

For any other non-`nil` action code, the `mouse-1` event is translated into a `mouse-2` event at the same position.

To define `mouse-1` to activate a button defined with `define-button-type`, give the button a `follow-link` property. The property value should be a link action condition, as described above. See [Buttons](Buttons.html). For example, here is how Help mode handles `mouse-1`:

```lisp
(define-button-type 'help-xref
  'follow-link t
  'action #'help-button-action)
```

To define `mouse-1` on a widget defined with `define-widget`, give the widget a `:follow-link` property. The property value should be a link action condition, as described above. For example, here is how the `link` widget specifies that a `mouse-1` click shall be translated to `RET`:

```lisp
(define-widget 'link 'item
  "An embedded link."
  :button-prefix 'widget-link-prefix
  :button-suffix 'widget-link-suffix
  :follow-link "\C-m"
  :help-echo "Follow the link."
  :format "%[%t%]")
```

### Function: **mouse-on-link-p** *pos*

This function returns non-`nil` if position `pos` in the current buffer is on a link. `pos` can also be a mouse event location, as returned by `event-start` (see [Accessing Mouse](Accessing-Mouse.html)).
