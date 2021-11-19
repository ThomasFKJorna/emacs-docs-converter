

Next: [Face Functions](Face-Functions.html), Previous: [Displaying Faces](Displaying-Faces.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.12.5 Face Remapping

The variable `face-remapping-alist` is used for buffer-local or global changes in the appearance of a face. For instance, it is used to implement the `text-scale-adjust` command (see [Text Scale](https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Scale.html#Text-Scale) in The GNU Emacs Manual).

*   Variable: **face-remapping-alist**

    The value of this variable is an alist whose elements have the form `(face . remapping)`. This causes Emacs to display any text having the face `face` with `remapping`, rather than the ordinary definition of `face`.

    `remapping` may be any face spec suitable for a `face` text property: either a face (i.e., a face name or a property list of attribute/value pairs), or a list of faces. For details, see the description of the `face` text property in [Special Properties](Special-Properties.html). `remapping` serves as the complete specification for the remapped face—it replaces the normal definition of `face`, instead of modifying it.

    If `face-remapping-alist` is buffer-local, its local value takes effect only within that buffer. If `face-remapping-alist` includes faces applicable only to certain windows, by using the `(:filtered (:window param val) spec)`, that face takes effect only in windows that match the filter conditions (see [Special Properties](Special-Properties.html)). To turn off face filtering temporarily, bind `face-filters-always-match` to a non-`nil` value, then all face filters will match any window.

    Note: face remapping is non-recursive. If `remapping` references the same face name `face`, either directly or via the `:inherit` attribute of some other face in `remapping`, that reference uses the normal definition of `face`. For instance, if the `mode-line` face is remapped using this entry in `face-remapping-alist`:

    ```lisp
    (mode-line italic mode-line)
    ```

    then the new definition of the `mode-line` face inherits from the `italic` face, and the *normal* (non-remapped) definition of `mode-line` face.

The following functions implement a higher-level interface to `face-remapping-alist`. Most Lisp code should use these functions instead of setting `face-remapping-alist` directly, to avoid trampling on remappings applied elsewhere. These functions are intended for buffer-local remappings, so they all make `face-remapping-alist` buffer-local as a side-effect. They manage `face-remapping-alist` entries of the form

```lisp
  (face relative-spec-1 relative-spec-2 ... base-spec)
```

where, as explained above, each of the `relative-spec-N` and `base-spec` is either a face name, or a property list of attribute/value pairs. Each of the *relative remapping* entries, `relative-spec-N`, is managed by the `face-remap-add-relative` and `face-remap-remove-relative` functions; these are intended for simple modifications like changing the text size. The *base remapping* entry, `base-spec`, has the lowest priority and is managed by the `face-remap-set-base` and `face-remap-reset-base` functions; it is intended for major modes to remap faces in the buffers they control.

*   Function: **face-remap-add-relative** *face \&rest specs*

    This function adds the face spec in `specs` as relative remappings for face `face` in the current buffer. The remaining arguments, `specs`, should form either a list of face names, or a property list of attribute/value pairs.

    The return value is a Lisp object that serves as a cookie; you can pass this object as an argument to `face-remap-remove-relative` if you need to remove the remapping later.

    ```lisp
    ;; Remap the 'escape-glyph' face into a combination
    ;; of the 'highlight' and 'italic' faces:
    (face-remap-add-relative 'escape-glyph 'highlight 'italic)

    ;; Increase the size of the 'default' face by 50%:
    (face-remap-add-relative 'default :height 1.5)
    ```

<!---->

*   Function: **face-remap-remove-relative** *cookie*

    This function removes a relative remapping previously added by `face-remap-add-relative`. `cookie` should be the Lisp object returned by `face-remap-add-relative` when the remapping was added.

<!---->

*   Function: **face-remap-set-base** *face \&rest specs*

    This function sets the base remapping of `face` in the current buffer to `specs`. If `specs` is empty, the default base remapping is restored, similar to calling `face-remap-reset-base` (see below); note that this is different from `specs` containing a single value `nil`, which has the opposite result (the global definition of `face` is ignored).

    This overwrites the default `base-spec`, which inherits the global face definition, so it is up to the caller to add such inheritance if so desired.

<!---->

*   Function: **face-remap-reset-base** *face*

    This function sets the base remapping of `face` to its default value, which inherits from `face`’s global definition.

Next: [Face Functions](Face-Functions.html), Previous: [Displaying Faces](Displaying-Faces.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
