

### 9.3 Creating and Interning Symbols

To understand how symbols are created in GNU Emacs Lisp, you must know how Lisp reads them. Lisp must ensure that it finds the same symbol every time it reads the same set of characters. Failure to do so would cause complete confusion.

When the Lisp reader encounters a symbol, it reads all the characters of the name. Then it hashes those characters to find an index in a table called an *obarray*. Hashing is an efficient method of looking something up. For example, instead of searching a telephone book cover to cover when looking up Jan Jones, you start with the J’s and go from there. That is a simple version of hashing. Each element of the obarray is a *bucket* which holds all the symbols with a given hash code; to look for a given name, it is sufficient to look through all the symbols in the bucket for that name’s hash code. (The same idea is used for general Emacs hash tables, but they are a different data type; see [Hash Tables](Hash-Tables.html).)

If a symbol with the desired name is found, the reader uses that symbol. If the obarray does not contain a symbol with that name, the reader makes a new symbol and adds it to the obarray. Finding or adding a symbol with a certain name is called *interning* it, and the symbol is then called an *interned symbol*.

Interning ensures that each obarray has just one symbol with any particular name. Other like-named symbols may exist, but not in the same obarray. Thus, the reader gets the same symbols for the same names, as long as you keep reading with the same obarray.

Interning usually happens automatically in the reader, but sometimes other programs need to do it. For example, after the `M-x` command obtains the command name as a string using the minibuffer, it then interns the string, to get the interned symbol with that name.

No obarray contains all symbols; in fact, some symbols are not in any obarray. They are called *uninterned symbols*. An uninterned symbol has the same four cells as other symbols; however, the only way to gain access to it is by finding it in some other object or as the value of a variable.

Creating an uninterned symbol is useful in generating Lisp code, because an uninterned symbol used as a variable in the code you generate cannot clash with any variables used in other Lisp programs.

In Emacs Lisp, an obarray is actually a vector. Each element of the vector is a bucket; its value is either an interned symbol whose name hashes to that bucket, or 0 if the bucket is empty. Each interned symbol has an internal link (invisible to the user) to the next symbol in the bucket. Because these links are invisible, there is no way to find all the symbols in an obarray except using `mapatoms` (below). The order of symbols in a bucket is not significant.

In an empty obarray, every element is 0, so you can create an obarray with `(make-vector length 0)`. **This is the only valid way to create an obarray.** Prime numbers as lengths tend to result in good hashing; lengths one less than a power of two are also good.

**Do not try to put symbols in an obarray yourself.** This does not work—only `intern` can enter a symbol in an obarray properly.

> **Common Lisp note:** Unlike Common Lisp, Emacs Lisp does not provide for interning a single symbol in several obarrays.

Most of the functions below take a name and sometimes an obarray as arguments. A `wrong-type-argument` error is signaled if the name is not a string, or if the obarray is not a vector.

### Function: **symbol-name** *symbol*

This function returns the string that is `symbol`’s name. For example:

```lisp
(symbol-name 'foo)
     ⇒ "foo"
```

**Warning:** Changing the string by substituting characters does change the name of the symbol, but fails to update the obarray, so don’t do it!

### Function: **make-symbol** *name*

This function returns a newly-allocated, uninterned symbol whose name is `name` (which must be a string). Its value and function definition are void, and its property list is `nil`. In the example below, the value of `sym` is not `eq` to `foo` because it is a distinct uninterned symbol whose name is also ‘`foo`’.

```lisp
(setq sym (make-symbol "foo"))
     ⇒ foo
(eq sym 'foo)
     ⇒ nil
```

### Function: **gensym** *\&optional prefix*

This function returns a symbol using `make-symbol`, whose name is made by appending `gensym-counter` to `prefix`. The prefix defaults to `"g"`.

### Function: **intern** *name \&optional obarray*

This function returns the interned symbol whose name is `name`. If there is no such symbol in the obarray `obarray`, `intern` creates a new one, adds it to the obarray, and returns it. If `obarray` is omitted, the value of the global variable `obarray` is used.

```lisp
(setq sym (intern "foo"))
     ⇒ foo
(eq sym 'foo)
     ⇒ t

(setq sym1 (intern "foo" other-obarray))
     ⇒ foo
(eq sym1 'foo)
     ⇒ nil
```

> **Common Lisp note:** In Common Lisp, you can intern an existing symbol in an obarray. In Emacs Lisp, you cannot do this, because the argument to `intern` must be a string, not a symbol.

### Function: **intern-soft** *name \&optional obarray*

This function returns the symbol in `obarray` whose name is `name`, or `nil` if `obarray` has no symbol with that name. Therefore, you can use `intern-soft` to test whether a symbol with a given name is already interned. If `obarray` is omitted, the value of the global variable `obarray` is used.

The argument `name` may also be a symbol; in that case, the function returns `name` if `name` is interned in the specified obarray, and otherwise `nil`.

```lisp
(intern-soft "frazzle")        ; No such symbol exists.
     ⇒ nil
(make-symbol "frazzle")        ; Create an uninterned one.
     ⇒ frazzle
```

```lisp
(intern-soft "frazzle")        ; That one cannot be found.
     ⇒ nil
```

```lisp
(setq sym (intern "frazzle"))  ; Create an interned one.
     ⇒ frazzle
```

```lisp
(intern-soft "frazzle")        ; That one can be found!
     ⇒ frazzle
```

```lisp
(eq sym 'frazzle)              ; And it is the same one.
     ⇒ t
```

### Variable: **obarray**

This variable is the standard obarray for use by `intern` and `read`.

### Function: **mapatoms** *function \&optional obarray*

This function calls `function` once with each symbol in the obarray `obarray`. Then it returns `nil`. If `obarray` is omitted, it defaults to the value of `obarray`, the standard obarray for ordinary symbols.

```lisp
(setq count 0)
     ⇒ 0
(defun count-syms (s)
  (setq count (1+ count)))
     ⇒ count-syms
(mapatoms 'count-syms)
     ⇒ nil
count
     ⇒ 1871
```

See `documentation` in [Accessing Documentation](Accessing-Documentation.html), for another example using `mapatoms`.

### Function: **unintern** *symbol obarray*

This function deletes `symbol` from the obarray `obarray`. If `symbol` is not actually in the obarray, `unintern` does nothing. If `obarray` is `nil`, the current obarray is used.

If you provide a string instead of a symbol as `symbol`, it stands for a symbol name. Then `unintern` deletes the symbol (if any) in the obarray which has that name. If there is no such symbol, `unintern` does nothing.

If `unintern` does delete a symbol, it returns `t`. Otherwise it returns `nil`.
