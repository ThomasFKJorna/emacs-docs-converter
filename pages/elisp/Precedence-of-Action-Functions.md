

#### 28.13.5 Precedence of Action Functions

From the past subsections we already know that `display-buffer` must be supplied with a number of display actions (see [Choosing Window](Choosing-Window.html)) in order to display a buffer. In a completely uncustomized Emacs, these actions are specified by `display-buffer-fallback-action` in the following order of precedence: Reuse a window, pop up a new window on the same frame, use a window previously showing the buffer, use some window and pop up a new frame. (Note that the remaining actions named by `display-buffer-fallback-action` are void in an uncustomized Emacs).

Consider the following form:

```lisp
(display-buffer (get-buffer-create "*foo*"))
```

Evaluating this form in the buffer `*scratch*` of an uncustomized Emacs session will usually fail to reuse a window that shows `*foo*` already, but succeed in popping up a new window. Evaluating the same form again will now not cause any visible changes—`display-buffer` reused the window already showing `*foo*` because that action was applicable and had the highest precedence among all applicable actions.

Popping up a new window will fail if there is not enough space on the selected frame. In an uncustomized Emacs it typically fails when there are already two windows on a frame. For example, if you now type `C-x 1` followed by `C-x 2` and evaluate the form once more, `*foo*` should show up in the lower window—`display-buffer` just used “some” window. If, before typing `C-x 2` you had typed `C-x o`, `*foo*` would have been shown in the upper window because “some” window stands for the “least recently used” window and the selected window has been least recently used if and only if it is alone on its frame.

Let’s assume you did not type `C-x o` and `*foo*` is shown in the lower window. Type `C-x o` to get there followed by `C-x left` and evaluate the form again. This should display `*foo*` in the same, lower window because that window had already shown `*foo*` previously and was therefore chosen instead of some other window.

So far we have only observed the default behavior in an uncustomized Emacs session. To see how this behavior can be customized, let’s consider the option `display-buffer-base-action`. It provides a very coarse customization which conceptually affects the display of *any* buffer. It can be used to supplement the actions supplied by `display-buffer-fallback-action` by reordering them or by adding actions that are not present there but fit more closely the user’s editing practice. However, it can also be used to change the default behavior in a more profound way.

Let’s consider a user who, as a rule, prefers to display buffers on another frame. Such a user might provide the following customization:

```lisp
(customize-set-variable
 'display-buffer-base-action
 '((display-buffer-reuse-window display-buffer-pop-up-frame)
   (reusable-frames . 0)))
```

This setting will cause `display-buffer` to first try to find a window showing the buffer on a visible or iconified frame and, if no such frame exists, pop up a new frame. You can observe this behavior on a graphical system by typing `C-x 1` in the window showing `*scratch*` and evaluating our canonical `display-buffer` form. This will usually create (and give focus to) a new frame whose root window shows `*foo*`. Iconify that frame and evaluate the canonical form again: `display-buffer` will reuse the window on the new frame (usually raising the frame and giving it focus too).

Only if creating a new frame fails, `display-buffer` will apply the actions supplied by `display-buffer-fallback-action` which means to again try reusing a window, popping up a new window and so on. A trivial way to make frame creation fail is supplied by the following form:

```lisp
(let ((pop-up-frame-function 'ignore))
  (display-buffer (get-buffer-create "*foo*")))
```

We will forget about that form immediately after observing that it fails to create a new frame and uses a fallback action instead.

Note that `display-buffer-reuse-window` appears redundant in the customization of `display-buffer-base-action` because it is already part of `display-buffer-fallback-action` and should be tried there anyway. However, that would fail because due to the precedence of `display-buffer-base-action` over `display-buffer-fallback-action`, at that time `display-buffer-pop-up-frame` would have already won the race. In fact, this:

```lisp
(customize-set-variable
 'display-buffer-base-action
 '(display-buffer-pop-up-frame (reusable-frames . 0)))
```

would cause `display-buffer` to *always* pop up a new frame which is probably not what our user wants.

So far, we have only shown how *users* can customize the default behavior of `display-buffer`. Let us now see how *applications* can change the course of `display-buffer`. The canonical way to do that is to use the `action` argument of `display-buffer` or a function that calls it, like, for example, `pop-to-buffer` (see [Switching Buffers](Switching-Buffers.html)).

Suppose an application wants to display `*foo*` preferably below the selected window (to immediately attract the attention of the user to the new window) or, if that fails, in a window at the bottom of the frame. It could do that with a call like this:

```lisp
(display-buffer
 (get-buffer-create "*foo*")
 '((display-buffer-below-selected display-buffer-at-bottom)))
```

In order to see how this new, modified form works, delete any frame showing `*foo*`, type `C-x 1` followed by `C-x 2` in the window showing `*scratch*`, and subsequently evaluate that form. `display-buffer` should split the upper window, and show `*foo*` in the new window. Alternatively, if after `C-x 2` you had typed `C-x o`, `display-buffer` would have split the window at the bottom instead.

Suppose now that, before evaluating the new form, you have made the selected window as small as possible, for example, by evaluating the form `(fit-window-to-buffer)` in that window. In that case, `display-buffer` would have failed to split the selected window and would have split the frame’s root window instead, effectively displaying `*foo*` at the bottom of the frame.

In either case, evaluating the new form a second time should reuse the window already showing `*foo*` since both functions supplied by the `action` argument try to reuse such a window first.

By setting the `action` argument, an application effectively overrules any customization of `display-buffer-base-action`. Our user can now either accept the choice of the application, or redouble by customizing the option `display-buffer-alist` as follows:

```lisp
(customize-set-variable
 'display-buffer-alist
 '(("\\*foo\\*"
    (display-buffer-reuse-window display-buffer-pop-up-frame))))
```

Trying this with the new, modified form above in a configuration that does not show `*foo*` anywhere, will display `*foo*` on a separate frame, completely ignoring the `action` argument of `display-buffer`.

Note that we didn’t care to specify a `reusable-frames` action alist entry in our specification of `display-buffer-alist`. `display-buffer` always takes the first one it finds—in our case the one specified by `display-buffer-base-action`. If we wanted to use a different specification, for example, to exclude iconified frames showing `*foo*` from the list of reusable ones, we would have to specify that separately, however:

```lisp
(customize-set-variable
 'display-buffer-alist
 '(("\\*foo\\*"
    (display-buffer-reuse-window display-buffer-pop-up-frame)
    (reusable-frames . visible))))
```

If you try this, you will notice that repeated attempts to display `*foo*` will succeed to reuse a frame only if that frame is visible.

The above example would allow the conclusion that users customize `display-buffer-alist` for the sole purpose to overrule the `action` argument chosen by applications. Such a conclusion would be incorrect. `display-buffer-alist` is the standard option for users to direct the course of display of specific buffers in a preferred way regardless of whether the display is also guided by an `action` argument.

We can, however, reasonably conclude that customizing `display-buffer-alist` differs from customizing `display-buffer-base-action` in two major aspects: it is stronger because it overrides the `action` argument of `display-buffer`, and it allows to explicitly specify the affected buffers. In fact, displaying other buffers is not affected in any way by a customization for `*foo*`. For example,

```lisp
(display-buffer (get-buffer-create "*bar*"))
```

continues being governed by the settings of `display-buffer-base-action` and `display-buffer-fallback-action` only.

We could stop with our examples here but Lisp programs still have an ace up their sleeves which they can use to overrule any customization of `display-buffer-alist`. It’s the variable `display-buffer-overriding-action` which they can bind around `display-buffer` calls as follows:

```lisp
(let ((display-buffer-overriding-action
       '((display-buffer-same-window))))
  (display-buffer
   (get-buffer-create "*foo*")
   '((display-buffer-below-selected display-buffer-at-bottom))))
```

Evaluating this form will usually display `*foo*` in the selected window regardless of the `action` argument and any user customizations. (Usually, an application will not bother to also provide an `action` argument. Here it just serves to illustrate the fact that it gets overridden.)

It might be illustrative to look at the list of action functions `display-buffer` would have tried to display `*foo*` with the customizations we provided here. The list (including comments explaining who added this and the subsequent elements) is:

```lisp
(display-buffer-same-window  ;; `display-buffer-overriding-action'
 display-buffer-reuse-window ;; `display-buffer-alist'
 display-buffer-pop-up-frame
 display-buffer-below-selected ;; ACTION argument
 display-buffer-at-bottom
 display-buffer-reuse-window ;; `display-buffer-base-action'
 display-buffer-pop-up-frame
 display-buffer--maybe-same-window ;; `display-buffer-fallback-action'
 display-buffer-reuse-window
 display-buffer--maybe-pop-up-frame-or-window
 display-buffer-in-previous-window
 display-buffer-use-some-window
 display-buffer-pop-up-frame)
```

Note that among the internal functions listed here, `display-buffer--maybe-same-window` is effectively ignored while `display-buffer--maybe-pop-up-frame-or-window` actually runs `display-buffer-pop-up-window`.

The action alist passed in each function call is:

```lisp
((reusable-frames . visible)
 (reusable-frames . 0))
```

which shows that we have used the second specification of `display-buffer-alist` above, overriding the specification supplied by `display-buffer-base-action`. Suppose our user had written that as

```lisp
(customize-set-variable
 'display-buffer-alist
 '(("\\*foo\\*"
    (display-buffer-reuse-window display-buffer-pop-up-frame)
    (inhibit-same-window . t)
    (reusable-frames . visible))))
```

In this case the `inhibit-same-window` alist entry will successfully invalidate the `display-buffer-same-window` specification from `display-buffer-overriding-action` and `display-buffer` will show `*foo*` on another frame. To make `display-buffer-overriding-action` more robust in this regard, the application would have to specify an appropriate `inhibit-same-window` entry too, for example, as follows:

```lisp
(let ((display-buffer-overriding-action
       '(display-buffer-same-window (inhibit-same-window . nil))))
  (display-buffer (get-buffer-create "*foo*")))
```

This last example shows that while the precedence order of action functions is fixed, as described in [Choosing Window](Choosing-Window.html), an action alist entry specified by a display action ranked lower in that order can affect the execution of a higher ranked display action.
