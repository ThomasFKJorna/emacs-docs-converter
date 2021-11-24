

#### 39.17.6 SVG Images

SVG (Scalable Vector Graphics) is an XML format for specifying images. If your Emacs build has SVG support, you can create and manipulate these images with the following functions from the `svg.el` library.

### Function: **svg-create** *width height \&rest args*

Create a new, empty SVG image with the specified dimensions. `args` is an argument plist with you can specify following:

*   `:stroke-width`

    The default width (in pixels) of any lines created.

*   `:stroke`

    The default stroke color on any lines created.

This function returns an *SVG object*, a Lisp data structure that specifies an SVG image, and all the following functions work on that structure. The argument `svg` in the following functions specifies such an SVG object.

### Function: **svg-gradient** *svg id type stops*

Create a gradient in `svg` with identifier `id`. `type` specifies the gradient type, and can be either `linear` or `radial`. `stops` is a list of percentage/color pairs.

The following will create a linear gradient that goes from red at the start, to green 25% of the way, to blue at the end:

```lisp
(svg-gradient svg "gradient1" 'linear
              '((0 . "red") (25 . "green") (100 . "blue")))
```

The gradient created (and inserted into the SVG object) can later be used by all functions that create shapes.

All the following functions take an optional list of keyword parameters that alter the various attributes from their default values. Valid attributes include:

### `:stroke-width`

The width (in pixels) of lines drawn, and outlines around solid shapes.

### `:stroke-color`

The color of lines drawn, and outlines around solid shapes.

### `:fill-color`

The color used for solid shapes.

### `:id`

The identified of the shape.

### `:gradient`

If given, this should be the identifier of a previously defined gradient object.

### `:clip-path`

Identifier of a clip path.

### Function: **svg-rectangle** *svg x y width height \&rest args*

Add to `svg` a rectangle whose upper left corner is at position `x`/`y` and whose size is `width`/`height`.

```lisp
(svg-rectangle svg 100 100 500 500 :gradient "gradient1")
```

### Function: **svg-circle** *svg x y radius \&rest args*

Add to `svg` a circle whose center is at `x`/`y` and whose radius is `radius`.

### Function: **svg-ellipse** *svg x y x-radius y-radius \&rest args*

Add to `svg` an ellipse whose center is at `x`/`y`, and whose horizontal radius is `x-radius` and the vertical radius is `y-radius`.

### Function: **svg-line** *svg x1 y1 x2 y2 \&rest args*

Add to `svg` a line that starts at `x1`/`y1` and extends to `x2`/`y2`.

### Function: **svg-polyline** *svg points \&rest args*

Add to `svg` a multiple-segment line (a.k.a. “polyline”) that goes through `points`, which is a list of X/Y position pairs.

```lisp
(svg-polyline svg '((200 . 100) (500 . 450) (80 . 100))
              :stroke-color "green")
```

### Function: **svg-polygon** *svg points \&rest args*

Add a polygon to `svg` where `points` is a list of X/Y pairs that describe the outer circumference of the polygon.

```lisp
(svg-polygon svg '((100 . 100) (200 . 150) (150 . 90))
             :stroke-color "blue" :fill-color "red")
```

### Function: **svg-path** *svg commands \&rest args*

Add the outline of a shape to `svg` according to `commands`, see [SVG Path Commands](#SVG-Path-Commands).

Coordinates by default are absolute. To use coordinates relative to the last position, or – initially – to the origin, set the attribute `:relative` to `t`. This attribute can be specified for the function or for individual commands. If specified for the function, then all commands use relative coordinates by default. To make an individual command use absolute coordinates, set `:relative` to `nil`.

```lisp
(svg-path svg
	  '((moveto ((100 . 100)))
	    (lineto ((200 . 0) (0 . 200) (-200 . 0)))
	    (lineto ((100 . 100)) :relative nil))
	  :stroke-color "blue"
	  :fill-color "lightblue"
	  :relative t)
```

### Function: **svg-text** *svg text \&rest args*

Add the specified `text` to `svg`.

```lisp
(svg-text
 svg "This is a text"
 :font-size "40"
 :font-weight "bold"
 :stroke "black"
 :fill "white"
 :font-family "impact"
 :letter-spacing "4pt"
 :x 300
 :y 400
 :stroke-width 1)
```

### Function: **svg-embed** *svg image image-type datap \&rest args*

Add an embedded (raster) image to `svg`. If `datap` is `nil`, `image` should be a file name; otherwise it should be a string containing the image data as raw bytes. `image-type` should be a MIME image type, for instance `"image/jpeg"`.

```lisp
(svg-embed svg "~/rms.jpg" "image/jpeg" nil
           :width "100px" :height "100px"
           :x "50px" :y "75px")
```

### Function: **svg-clip-path** *svg \&rest args*

Add a clipping path to `svg`. If applied to a shape via the `:clip-path` property, parts of that shape which lie outside of the clipping path are not drawn.

```lisp
(let ((clip-path (svg-clip-path svg :id "foo")))
  (svg-circle clip-path 200 200 175))
(svg-rectangle svg 50 50 300 300
               :fill-color "red"
               :clip-path "url(#foo)")
```

### Function: **svg-node** *svg tag \&rest args*

Add the custom node `tag` to `svg`.

```lisp
(svg-node svg
          'rect
          :width 300 :height 200 :x 50 :y 100 :fill-color "green")
```

### Function: **svg-remove** *svg id*

Remove the element with identifier `id` from the `svg`.

### Function: **svg-image** *svg*

Finally, the `svg-image` takes an SVG object as its argument and returns an image object suitable for use in functions like `insert-image`.

Here’s a complete example that creates and inserts an image with a circle:

```lisp
(let ((svg (svg-create 400 400 :stroke-width 10)))
  (svg-gradient svg "gradient1" 'linear '((0 . "red") (100 . "blue")))
  (svg-circle svg 200 200 100 :gradient "gradient1"
                  :stroke-color "green")
  (insert-image (svg-image svg)))
```

#### SVG Path Commands

*SVG paths* allow creation of complex images by combining lines, curves, arcs, and other basic shapes. The functions described below allow invoking SVG path commands from a Lisp program.

### Command: **moveto** *points*

Move the pen to the first point in `points`. Additional points are connected with lines. `points` is a list of X/Y coordinate pairs. Subsequent `moveto` commands represent the start of a new *subpath*.

```lisp
(svg-path svg '((moveto ((200 . 100) (100 . 200) (0 . 100))))
          :fill "white" :stroke "black")
```

### Command: **closepath**

End the current subpath by connecting it back to its initial point. A line is drawn along the connection.

```lisp
(svg-path svg '((moveto ((200 . 100) (100 . 200) (0 . 100)))
                (closepath)
                (moveto ((75 . 125) (100 . 150) (125 . 125)))
                (closepath))
          :fill "red" :stroke "black")
```

### Command: **lineto** *points*

Draw a line from the current point to the first element in `points`, a list of X/Y position pairs. If more than one point is specified, draw a polyline.

```lisp
(svg-path svg '((moveto ((200 . 100)))
                (lineto ((100 . 200) (0 . 100))))
          :fill "yellow" :stroke "red")
```

### Command: **horizontal-lineto** *x-coordinates*

Draw a horizontal line from the current point to the first element in `x-coordinates`. Specifying multiple coordinates is possible, although usually this doesn’t make sense.

```lisp
(svg-path svg '((moveto ((100 . 200)))
                (horizontal-lineto (300)))
          :stroke "green")
```

### Command: **vertical-lineto** *y-coordinates*

Draw vertical lines.

```lisp
(svg-path svg '((moveto ((200 . 100)))
                (vertical-lineto (300)))
          :stroke "green")
```

### Command: **curveto** *coordinate-sets*

Using the first element in `coordinate-sets`, draw a cubic Bézier curve from the current point. If there are multiple coordinate sets, draw a polybezier. Each coordinate set is a list of the form `(x1 y1 x2 y2 x y)`, where (`x`, `y`) is the curve’s end point. (`x1`, `y1`) and (`x2`, `y2`) are control points at the beginning and at the end, respectively.

```lisp
(svg-path svg '((moveto ((100 . 100)))
                (curveto ((200 100 100 200 200 200)
                          (300 200 0 100 100 100))))
          :fill "transparent" :stroke "red")
```

### Command: **smooth-curveto** *coordinate-sets*

Using the first element in `coordinate-sets`, draw a cubic Bézier curve from the current point. If there are multiple coordinate sets, draw a polybezier. Each coordinate set is a list of the form `(x2 y2 x y)`, where (`x`, `y`) is the curve’s end point and (`x2`, `y2`) is the corresponding control point. The first control point is the reflection of the second control point of the previous command relative to the current point, if that command was `curveto` or `smooth-curveto`. Otherwise the first control point coincides with the current point.

```lisp
(svg-path svg '((moveto ((100 . 100)))
                (curveto ((200 100 100 200 200 200)))
                (smooth-curveto ((0 100 100 100))))
          :fill "transparent" :stroke "blue")
```

### Command: **quadratic-bezier-curveto** *coordinate-sets*

Using the first element in `coordinate-sets`, draw a quadratic Bézier curve from the current point. If there are multiple coordinate sets, draw a polybezier. Each coordinate set is a list of the form `(x1 y1 x y)`, where (`x`, `y`) is the curve’s end point and (`x1`, `y1`) is the control point.

```lisp
(svg-path svg '((moveto ((200 . 100)))
                (quadratic-bezier-curveto ((300 100 300 200)))
                (quadratic-bezier-curveto ((300 300 200 300)))
                (quadratic-bezier-curveto ((100 300 100 200)))
                (quadratic-bezier-curveto ((100 100 200 100))))
          :fill "transparent" :stroke "pink")
```

### Command: **smooth-quadratic-bezier-curveto** *coordinate-sets*

Using the first element in `coordinate-sets`, draw a quadratic Bézier curve from the current point. If there are multiple coordinate sets, draw a polybezier. Each coordinate set is a list of the form `(x y)`, where (`x`, `y`) is the curve’s end point. The control point is the reflection of the control point of the previous command relative to the current point, if that command was `quadratic-bezier-curveto` or `smooth-quadratic-bezier-curveto`. Otherwise the control point coincides with the current point.

```lisp
(svg-path svg '((moveto ((200 . 100)))
                (quadratic-bezier-curveto ((300 100 300 200)))
                (smooth-quadratic-bezier-curveto ((200 300)))
                (smooth-quadratic-bezier-curveto ((100 200)))
                (smooth-quadratic-bezier-curveto ((200 100))))
          :fill "transparent" :stroke "lightblue")
```

### Command: **elliptical-arc** *coordinate-sets*

Using the first element in `coordinate-sets`, draw an elliptical arc from the current point. If there are multiple coordinate sets, draw a sequence of elliptical arcs. Each coordinate set is a list of the form `(rx ry x y)`, where (`x`, `y`) is the end point of the ellipse, and (`rx`, `ry`) are its radii. Attributes may be appended to the list:

*   `:x-axis-rotation`

    The angle in degrees by which the x-axis of the ellipse is rotated relative to the x-axis of the current coordinate system.

*   `:large-arc`

    If set to `t`, draw an arc sweep greater than or equal to 180 degrees. Otherwise, draw an arc sweep smaller than or equal to 180 degrees.

*   `:sweep`

    If set to `t`, draw an arc in *positive angle direction*. Otherwise, draw it in *negative angle direction*.

```lisp
(svg-path svg '((moveto ((200 . 250)))
                (elliptical-arc ((75 75 200 350))))
          :fill "transparent" :stroke "red")
(svg-path svg '((moveto ((200 . 250)))
                (elliptical-arc ((75 75 200 350 :large-arc t))))
          :fill "transparent" :stroke "green")
(svg-path svg '((moveto ((200 . 250)))
                (elliptical-arc ((75 75 200 350 :sweep t))))
          :fill "transparent" :stroke "blue")
(svg-path svg '((moveto ((200 . 250)))
                (elliptical-arc ((75 75 200 350 :large-arc t
                                     :sweep t))))
          :fill "transparent" :stroke "gray")
(svg-path svg '((moveto ((160 . 100)))
                (elliptical-arc ((40 100 80 0)))
                (elliptical-arc ((40 100 -40 -70
                                     :x-axis-rotation -120)))
                (elliptical-arc ((40 100 -40 70
                                     :x-axis-rotation -240))))
          :stroke "pink" :fill "lightblue"
          :relative t)
```
