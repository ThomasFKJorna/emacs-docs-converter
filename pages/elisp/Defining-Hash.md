<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Other Hash](Other-Hash.html), Previous: [Hash Access](Hash-Access.html), Up: [Hash Tables](Hash-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 8.3 Defining Hash Comparisons

You can define new methods of key lookup by means of `define-hash-table-test`. In order to use this feature, you need to understand how hash tables work, and what a *hash code* means.

You can think of a hash table conceptually as a large array of many slots, each capable of holding one association. To look up a key, `gethash` first computes an integer, the hash code, from the key. It can reduce this integer modulo the length of the array, to produce an index in the array. Then it looks in that slot, and if necessary in other nearby slots, to see if it has found the key being sought.

Thus, to define a new method of key lookup, you need to specify both a function to compute the hash code from a key, and a function to compare two keys directly. The two functions should be consistent with each other: that is, two keys’ hash codes should be the same if the keys compare as equal. Also, since the two functions can be called at any time (such as by the garbage collector), the functions should be free of side effects and should return quickly, and their behavior should depend on only on properties of the keys that do not change.

*   Function: **define-hash-table-test** *name test-fn hash-fn*

    This function defines a new hash table test, named `name`.

    After defining `name` in this way, you can use it as the `test` argument in `make-hash-table`. When you do that, the hash table will use `test-fn` to compare key values, and `hash-fn` to compute a hash code from a key value.

    The function `test-fn` should accept two arguments, two keys, and return non-`nil` if they are considered the same.

    The function `hash-fn` should accept one argument, a key, and return an integer that is the hash code of that key. For good results, the function should use the whole range of fixnums for hash codes, including negative fixnums.

    The specified functions are stored in the property list of `name` under the property `hash-table-test`; the property value’s form is `(test-fn hash-fn)`.

<!---->

*   Function: **sxhash-equal** *obj*

    This function returns a hash code for Lisp object `obj`. This is an integer that reflects the contents of `obj` and the other Lisp objects it points to.

    If two objects `obj1` and `obj2` are `equal`, then `(sxhash-equal obj1)` and `(sxhash-equal obj2)` are the same integer.

    If the two objects are not `equal`, the values returned by `sxhash-equal` are usually different, but not always; once in a rare while, by luck, you will encounter two distinct-looking objects that give the same result from `sxhash-equal`.

    **Common Lisp note:** In Common Lisp a similar function is called `sxhash`. Emacs provides this name as a compatibility alias for `sxhash-equal`.

<!---->

*   Function: **sxhash-eq** *obj*

    This function returns a hash code for Lisp object `obj`. Its result reflects identity of `obj`, but not its contents.

    If two objects `obj1` and `obj2` are `eq`, then `(sxhash-eq obj1)` and `(sxhash-eq obj2)` are the same integer.

<!---->

*   Function: **sxhash-eql** *obj*

    This function returns a hash code for Lisp object `obj` suitable for `eql` comparison. I.e. it reflects identity of `obj` except for the case where the object is a bignum or a float number, in which case a hash code is generated for the value.

    If two objects `obj1` and `obj2` are `eql`, then `(sxhash-eql obj1)` and `(sxhash-eql obj2)` are the same integer.

This example creates a hash table whose keys are strings that are compared case-insensitively.

    (defun case-fold-string= (a b)
      (eq t (compare-strings a nil nil b nil nil t)))
    (defun case-fold-string-hash (a)
      (sxhash-equal (upcase a)))

    (define-hash-table-test 'case-fold
      'case-fold-string= 'case-fold-string-hash)

    (make-hash-table :test 'case-fold)

Here is how you could define a hash table test equivalent to the predefined test value `equal`. The keys can be any Lisp object, and equal-looking objects are considered the same key.

    (define-hash-table-test 'contents-hash 'equal 'sxhash-equal)

    (make-hash-table :test 'contents-hash)

Lisp programs should *not* rely on hash codes being preserved between Emacs sessions, as the implementation of the hash functions uses some details of the object storage that can change between sessions and between different architectures.

Next: [Other Hash](Other-Hash.html), Previous: [Hash Access](Hash-Access.html), Up: [Hash Tables](Hash-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
