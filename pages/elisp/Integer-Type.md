

#### 2.4.1 Integer Type

Under the hood, there are two kinds of integers—small integers, called *fixnums*, and large integers, called *bignums*.

The range of values for a fixnum depends on the machine. The minimum range is -536,870,912 to 536,870,911 (30 bits; i.e., -2\*\*29 to 2\*\*29 - 1) but many machines provide a wider range.

Bignums can have arbitrary precision. Operations that overflow a fixnum will return a bignum instead.

All numbers can be compared with `eql` or `=`; fixnums can also be compared with `eq`. To test whether an integer is a fixnum or a bignum, you can compare it to `most-negative-fixnum` and `most-positive-fixnum`, or you can use the convenience predicates `fixnump` and `bignump` on any object.

The read syntax for integers is a sequence of (base ten) digits with an optional sign at the beginning and an optional period at the end. The printed representation produced by the Lisp interpreter never has a leading ‘`+`’ or a final ‘`.`’.

```lisp
-1               ; The integer -1.
1                ; The integer 1.
1.               ; Also the integer 1.
+1               ; Also the integer 1.
```

See [Numbers](Numbers.html), for more information.
