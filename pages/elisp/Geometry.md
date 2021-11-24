

#### 29.4.4 Geometry

Here’s how to examine the data in an X-style window geometry specification:

### Function: **x-parse-geometry** *geom*

The function `x-parse-geometry` converts a standard X window geometry string to an alist that you can use as part of the argument to `make-frame`.

The alist describes which parameters were specified in `geom`, and gives the values specified for them. Each element looks like `(parameter . value)`. The possible `parameter` values are `left`, `top`, `width`, and `height`.

For the size parameters, the value must be an integer. The position parameter names `left` and `top` are not totally accurate, because some values indicate the position of the right or bottom edges instead. The `value` possibilities for the position parameters are: an integer, a list `(+ pos)`, or a list `(- pos)`; as previously described (see [Position Parameters](Position-Parameters.html)).

Here is an example:

```lisp
(x-parse-geometry "35x70+0-0")
     ⇒ ((height . 70) (width . 35)
         (top - 0) (left . 0))
```
