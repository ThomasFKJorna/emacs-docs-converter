

#### 1.3.6 Buffer Text Notation

Some examples describe modifications to the contents of a buffer, by showing the before and after versions of the text. These examples show the contents of the buffer in question between two lines of dashes containing the buffer name. In addition, ‘`∗`’ indicates the location of point. (The symbol for point, of course, is not part of the text in the buffer; it indicates the place *between* two characters where point is currently located.)

```lisp
---------- Buffer: foo ----------
This is the ∗contents of foo.
---------- Buffer: foo ----------

(insert "changed ")
     ⇒ nil
---------- Buffer: foo ----------
This is the changed ∗contents of foo.
---------- Buffer: foo ----------
```
