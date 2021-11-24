

#### 13.11.4 Adapting code using the old defadvice

A lot of code uses the old `defadvice` mechanism, which is largely made obsolete by the new `advice-add`, whose implementation and semantics is significantly simpler.

An old piece of advice such as:

```lisp
(defadvice previous-line (before next-line-at-end
                                 (&optional arg try-vscroll))
  "Insert an empty line when moving up from the top line."
  (if (and next-line-add-newlines (= arg 1)
           (save-excursion (beginning-of-line) (bobp)))
      (progn
        (beginning-of-line)
        (newline))))
```

could be translated in the new advice mechanism into a plain function:

```lisp
(defun previous-line--next-line-at-end (&optional arg try-vscroll)
  "Insert an empty line when moving up from the top line."
  (if (and next-line-add-newlines (= arg 1)
           (save-excursion (beginning-of-line) (bobp)))
      (progn
        (beginning-of-line)
        (newline))))
```

Obviously, this does not actually modify `previous-line`. For that the old advice needed:

```lisp
(ad-activate 'previous-line)
```

whereas the new advice mechanism needs:

```lisp
(advice-add 'previous-line :before #'previous-line--next-line-at-end)
```

Note that `ad-activate` had a global effect: it activated all pieces of advice enabled for that specified function. If you wanted to only activate or deactivate a particular piece, you needed to *enable* or *disable* it with `ad-enable-advice` and `ad-disable-advice`. The new mechanism does away with this distinction.

Around advice such as:

```lisp
(defadvice foo (around foo-around)
  "Ignore case in `foo'."
  (let ((case-fold-search t))
    ad-do-it))
(ad-activate 'foo)
```

could translate into:

```lisp
(defun foo--foo-around (orig-fun &rest args)
  "Ignore case in `foo'."
  (let ((case-fold-search t))
    (apply orig-fun args)))
(advice-add 'foo :around #'foo--foo-around)
```

Regarding the advice’s *class*, note that the new `:before` is not quite equivalent to the old `before`, because in the old advice you could modify the function’s arguments (e.g., with `ad-set-arg`), and that would affect the argument values seen by the original function, whereas in the new `:before`, modifying an argument via `setq` in the advice has no effect on the arguments seen by the original function. When porting `before` advice which relied on this behavior, you’ll need to turn it into new `:around` or `:filter-args` advice instead.

Similarly old `after` advice could modify the returned value by changing `ad-return-value`, whereas new `:after` advice cannot, so when porting such old `after` advice, you’ll need to turn it into new `:around` or `:filter-return` advice instead.
