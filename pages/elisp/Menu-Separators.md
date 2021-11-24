

#### 22.17.1.3 Menu Separators

A menu separator is a kind of menu item that doesn’t display any text—instead, it divides the menu into subparts with a horizontal line. A separator looks like this in the menu keymap:

```lisp
(menu-item separator-type)
```

where `separator-type` is a string starting with two or more dashes.

In the simplest case, `separator-type` consists of only dashes. That specifies the default kind of separator. (For compatibility, `""` and `-` also count as separators.)

Certain other values of `separator-type` specify a different style of separator. Here is a table of them:

*   `"--no-line"`
*   `"--space"`

An extra vertical space, with no actual line.

`"--single-line"`

A single line in the menu’s foreground color.

`"--double-line"`

A double line in the menu’s foreground color.

`"--single-dashed-line"`

A single dashed line in the menu’s foreground color.

`"--double-dashed-line"`

A double dashed line in the menu’s foreground color.

`"--shadow-etched-in"`

A single line with a 3D sunken appearance. This is the default, used separators consisting of dashes only.

`"--shadow-etched-out"`

A single line with a 3D raised appearance.

`"--shadow-etched-in-dash"`

A single dashed line with a 3D sunken appearance.

`"--shadow-etched-out-dash"`

A single dashed line with a 3D raised appearance.

`"--shadow-double-etched-in"`

Two lines with a 3D sunken appearance.

`"--shadow-double-etched-out"`

Two lines with a 3D raised appearance.

`"--shadow-double-etched-in-dash"`

Two dashed lines with a 3D sunken appearance.

`"--shadow-double-etched-out-dash"`

Two dashed lines with a 3D raised appearance.

You can also give these names in another style, adding a colon after the double-dash and replacing each single dash with capitalization of the following word. Thus, `"--:singleLine"`, is equivalent to `"--single-line"`.

You can use a longer form to specify keywords such as `:enable` and `:visible` for a menu separator:

`(menu-item separator-type nil . item-property-list)`

For example:

```lisp
(menu-item "--" nil :visible (boundp 'foo))
```

Some systems and display toolkits don’t really handle all of these separator types. If you use a type that isn’t supported, the menu displays a similar kind of separator that is supported.
