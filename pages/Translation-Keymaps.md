

Next: [Key Binding Commands](Key-Binding-Commands.html), Previous: [Remapping Commands](Remapping-Commands.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.14 Keymaps for Translating Sequences of Events

When the `read-key-sequence` function reads a key sequence (see [Key Sequence Input](Key-Sequence-Input.html)), it uses *translation keymaps* to translate certain event sequences into others. The translation keymaps are `input-decode-map`, `local-function-key-map`, and `key-translation-map` (in order of priority).

Translation keymaps have the same structure as other keymaps, but are used differently: they specify translations to make while reading key sequences, rather than bindings for complete key sequences. As each key sequence is read, it is checked against each translation keymap. If one of the translation keymaps binds `k` to a vector `v`, then whenever `k` appears as a sub-sequence *anywhere* in a key sequence, that sub-sequence is replaced with the events in `v`.

For example, VT100 terminals send `ESC O P` when the keypad key `PF1` is pressed. On such terminals, Emacs must translate that sequence of events into a single event `pf1`. This is done by binding `ESC O P` to `[pf1]` in `input-decode-map`. Thus, when you type `C-c PF1` on the terminal, the terminal emits the character sequence `C-c ESC O P`, and `read-key-sequence` translates this back into `C-c PF1` and returns it as the vector `[?\C-c pf1]`.

Translation keymaps take effect only after Emacs has decoded the keyboard input (via the input coding system specified by `keyboard-coding-system`). See [Terminal I/O Encoding](Terminal-I_002fO-Encoding.html).

*   Variable: **input-decode-map**

    This variable holds a keymap that describes the character sequences sent by function keys on an ordinary character terminal.

    The value of `input-decode-map` is usually set up automatically according to the terminal’s Terminfo or Termcap entry, but sometimes those need help from terminal-specific Lisp files. Emacs comes with terminal-specific files for many common terminals; their main purpose is to make entries in `input-decode-map` beyond those that can be deduced from Termcap and Terminfo. See [Terminal-Specific](Terminal_002dSpecific.html).

<!---->

*   Variable: **local-function-key-map**

    This variable holds a keymap similar to `input-decode-map` except that it describes key sequences which should be translated to alternative interpretations that are usually preferred. It applies after `input-decode-map` and before `key-translation-map`.

    Entries in `local-function-key-map` are ignored if they conflict with bindings made in the minor mode, local, or global keymaps. I.e., the remapping only applies if the original key sequence would otherwise not have any binding.

    `local-function-key-map` inherits from `function-key-map`. The latter should only be altered if you want the binding to apply in all terminals, so using the former is almost always preferred.

<!---->

*   Variable: **key-translation-map**

    This variable is another keymap used just like `input-decode-map` to translate input events into other events. It differs from `input-decode-map` in that it goes to work after `local-function-key-map` is finished rather than before; it receives the results of translation by `local-function-key-map`.

    Just like `input-decode-map`, but unlike `local-function-key-map`, this keymap is applied regardless of whether the input key-sequence has a normal binding. Note however that actual key bindings can have an effect on `key-translation-map`, even though they are overridden by it. Indeed, actual key bindings override `local-function-key-map` and thus may alter the key sequence that `key-translation-map` receives. Clearly, it is better to avoid this type of situation.

    The intent of `key-translation-map` is for users to map one character set to another, including ordinary characters normally bound to `self-insert-command`.

You can use `input-decode-map`, `local-function-key-map`, and `key-translation-map` for more than simple aliases, by using a function, instead of a key sequence, as the translation of a key. Then this function is called to compute the translation of that key.

The key translation function receives one argument, which is the prompt that was specified in `read-key-sequence`—or `nil` if the key sequence is being read by the editor command loop. In most cases you can ignore the prompt value.

If the function reads input itself, it can have the effect of altering the event that follows. For example, here’s how to define `C-c h` to turn the character that follows into a Hyper character:

```lisp
(defun hyperify (prompt)
  (let ((e (read-event)))
    (vector (if (numberp e)
                (logior (ash 1 24) e)
              (if (memq 'hyper (event-modifiers e))
                  e
                (add-event-modifier "H-" e))))))

(defun add-event-modifier (string e)
  (let ((symbol (if (symbolp e) e (car e))))
    (setq symbol (intern (concat string
                                 (symbol-name symbol))))
    (if (symbolp e)
        symbol
      (cons symbol (cdr e)))))

(define-key local-function-key-map "\C-ch" 'hyperify)
```

#### 22.14.1 Interaction with normal keymaps

The end of a key sequence is detected when that key sequence either is bound to a command, or when Emacs determines that no additional event can lead to a sequence that is bound to a command.

This means that, while `input-decode-map` and `key-translation-map` apply regardless of whether the original key sequence would have a binding, the presence of such a binding can still prevent translation from taking place. For example, let us return to our VT100 example above and add a binding for `C-c ESC` to the global map; now when the user hits `C-c PF1` Emacs will fail to decode `C-c ESC O P` into `C-c PF1` because it will stop reading keys right after `C-x ESC`, leaving `O P` for later. This is in case the user really hit `C-c ESC`, in which case Emacs should not sit there waiting for the next key to decide whether the user really pressed `ESC` or `PF1`.

For that reason, it is better to avoid binding commands to key sequences where the end of the key sequence is a prefix of a key translation. The main such problematic suffixes/prefixes are `ESC`, `M-O` (which is really `ESC O`) and `M-[` (which is really `ESC [`).

Next: [Key Binding Commands](Key-Binding-Commands.html), Previous: [Remapping Commands](Remapping-Commands.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
