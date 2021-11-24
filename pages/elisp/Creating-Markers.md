

### 31.3 Functions that Create Markers

When you create a new marker, you can make it point nowhere, or point to the present position of point, or to the beginning or end of the accessible portion of the buffer, or to the same place as another given marker.

The next four functions all return markers with insertion type `nil`. See [Marker Insertion Types](Marker-Insertion-Types.html).

### Function: **make-marker**

This function returns a newly created marker that does not point anywhere.

```lisp
(make-marker)
     ⇒ #<marker in no buffer>
```

### Function: **point-marker**

This function returns a new marker that points to the present position of point in the current buffer. See [Point](Point.html). For an example, see `copy-marker`, below.

### Function: **point-min-marker**

This function returns a new marker that points to the beginning of the accessible portion of the buffer. This will be the beginning of the buffer unless narrowing is in effect. See [Narrowing](Narrowing.html).

### Function: **point-max-marker**

This function returns a new marker that points to the end of the accessible portion of the buffer. This will be the end of the buffer unless narrowing is in effect. See [Narrowing](Narrowing.html).

Here are examples of this function and `point-min-marker`, shown in a buffer containing a version of the source file for the text of this chapter.

```lisp
(point-min-marker)
     ⇒ #<marker at 1 in markers.texi>
(point-max-marker)
     ⇒ #<marker at 24080 in markers.texi>
```

```lisp
```

```lisp
(narrow-to-region 100 200)
     ⇒ nil
```

```lisp
(point-min-marker)
     ⇒ #<marker at 100 in markers.texi>
```

```lisp
(point-max-marker)
     ⇒ #<marker at 200 in markers.texi>
```

### Function: **copy-marker** *\&optional marker-or-integer insertion-type*

If passed a marker as its argument, `copy-marker` returns a new marker that points to the same place and the same buffer as does `marker-or-integer`. If passed an integer as its argument, `copy-marker` returns a new marker that points to position `marker-or-integer` in the current buffer.

The new marker’s insertion type is specified by the argument `insertion-type`. See [Marker Insertion Types](Marker-Insertion-Types.html).

```lisp
(copy-marker 0)
     ⇒ #<marker at 1 in markers.texi>
```

```lisp
```

```lisp
(copy-marker 90000)
     ⇒ #<marker at 24080 in markers.texi>
```

An error is signaled if `marker` is neither a marker nor an integer.

Two distinct markers are considered `equal` (even though not `eq`) to each other if they have the same position and buffer, or if they both point nowhere.

```lisp
(setq p (point-marker))
     ⇒ #<marker at 2139 in markers.texi>
```

```lisp
```

```lisp
(setq q (copy-marker p))
     ⇒ #<marker at 2139 in markers.texi>
```

```lisp
```

```lisp
(eq p q)
     ⇒ nil
```

```lisp
```

```lisp
(equal p q)
     ⇒ t
```
