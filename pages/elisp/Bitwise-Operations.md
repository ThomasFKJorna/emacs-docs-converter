

### 3.8 Bitwise Operations on Integers

In a computer, an integer is represented as a binary number, a sequence of *bits* (digits which are either zero or one). Conceptually the bit sequence is infinite on the left, with the most-significant bits being all zeros or all ones. A bitwise operation acts on the individual bits of such a sequence. For example, *shifting* moves the whole sequence left or right one or more places, reproducing the same pattern moved over.

The bitwise operations in Emacs Lisp apply only to integers.

### Function: **ash** *integer1 count*

`ash` (*arithmetic shift*) shifts the bits in `integer1` to the left `count` places, or to the right if `count` is negative. Left shifts introduce zero bits on the right; right shifts discard the rightmost bits. Considered as an integer operation, `ash` multiplies `integer1` by 2\*\*`count`, and then converts the result to an integer by rounding downward, toward minus infinity.

Here are examples of `ash`, shifting a pattern of bits one place to the left and to the right. These examples show only the low-order bits of the binary pattern; leading bits all agree with the highest-order bit shown. As you can see, shifting left by one is equivalent to multiplying by two, whereas shifting right by one is equivalent to dividing by two and then rounding toward minus infinity.

```lisp
(ash 7 1) ⇒ 14
;; Decimal 7 becomes decimal 14.
…000111
     ⇒
…001110
```

```lisp
```

```lisp
(ash 7 -1) ⇒ 3
…000111
     ⇒
…000011
```

```lisp
```

```lisp
(ash -7 1) ⇒ -14
…111001
     ⇒
…110010
```

```lisp
```

```lisp
(ash -7 -1) ⇒ -4
…111001
     ⇒
…111100
```

Here are examples of shifting left or right by two bits:

```lisp
                  ;         binary values
(ash 5 2)         ;   5  =  …000101
     ⇒ 20         ;      =  …010100
(ash -5 2)        ;  -5  =  …111011
     ⇒ -20        ;      =  …101100
```

```lisp
(ash 5 -2)
     ⇒ 1          ;      =  …000001
```

```lisp
(ash -5 -2)
     ⇒ -2         ;      =  …111110
```

### Function: **lsh** *integer1 count*

`lsh`, which is an abbreviation for *logical shift*, shifts the bits in `integer1` to the left `count` places, or to the right if `count` is negative, bringing zeros into the vacated bits. If `count` is negative, then `integer1` must be either a fixnum or a positive bignum, and `lsh` treats a negative fixnum as if it were unsigned by subtracting twice `most-negative-fixnum` before shifting, producing a nonnegative result. This quirky behavior dates back to when Emacs supported only fixnums; nowadays `ash` is a better choice.

As `lsh` behaves like `ash` except when `integer1` and `count1` are both negative, the following examples focus on these exceptional cases. These examples assume 30-bit fixnums.

```lisp
                 ;      binary values
(ash -7 -1)      ; -7 = …111111111111111111111111111001
     ⇒ -4        ;    = …111111111111111111111111111100
(lsh -7 -1)
     ⇒ 536870908 ;    = …011111111111111111111111111100
```

```lisp
(ash -5 -2)      ; -5 = …111111111111111111111111111011
     ⇒ -2        ;    = …111111111111111111111111111110
(lsh -5 -2)
     ⇒ 268435454 ;    = …001111111111111111111111111110
```

### Function: **logand** *\&rest ints-or-markers*

This function returns the bitwise AND of the arguments: the `n`th bit is 1 in the result if, and only if, the `n`th bit is 1 in all the arguments.

For example, using 4-bit binary numbers, the bitwise AND of 13 and 12 is 12: 1101 combined with 1100 produces 1100. In both the binary numbers, the leftmost two bits are both 1 so the leftmost two bits of the returned value are both 1. However, for the rightmost two bits, each is 0 in at least one of the arguments, so the rightmost two bits of the returned value are both 0.

Therefore,

```lisp
(logand 13 12)
     ⇒ 12
```

If `logand` is not passed any argument, it returns a value of -1. This number is an identity element for `logand` because its binary representation consists entirely of ones. If `logand` is passed just one argument, it returns that argument.

```lisp
                   ;        binary values

(logand 14 13)     ; 14  =  …001110
                   ; 13  =  …001101
     ⇒ 12         ; 12  =  …001100
```

```lisp
```

```lisp
(logand 14 13 4)   ; 14  =  …001110
                   ; 13  =  …001101
                   ;  4  =  …000100
     ⇒ 4          ;  4  =  …000100
```

```lisp
```

```lisp
(logand)
     ⇒ -1         ; -1  =  …111111
```

### Function: **logior** *\&rest ints-or-markers*

This function returns the bitwise inclusive OR of its arguments: the `n`th bit is 1 in the result if, and only if, the `n`th bit is 1 in at least one of the arguments. If there are no arguments, the result is 0, which is an identity element for this operation. If `logior` is passed just one argument, it returns that argument.

```lisp
                   ;        binary values

(logior 12 5)      ; 12  =  …001100
                   ;  5  =  …000101
     ⇒ 13         ; 13  =  …001101
```

```lisp
```

```lisp
(logior 12 5 7)    ; 12  =  …001100
                   ;  5  =  …000101
                   ;  7  =  …000111
     ⇒ 15         ; 15  =  …001111
```

### Function: **logxor** *\&rest ints-or-markers*

This function returns the bitwise exclusive OR of its arguments: the `n`th bit is 1 in the result if, and only if, the `n`th bit is 1 in an odd number of the arguments. If there are no arguments, the result is 0, which is an identity element for this operation. If `logxor` is passed just one argument, it returns that argument.

```lisp
                   ;        binary values

(logxor 12 5)      ; 12  =  …001100
                   ;  5  =  …000101
     ⇒ 9          ;  9  =  …001001
```

```lisp
```

```lisp
(logxor 12 5 7)    ; 12  =  …001100
                   ;  5  =  …000101
                   ;  7  =  …000111
     ⇒ 14         ; 14  =  …001110
```

### Function: **lognot** *integer*

This function returns the bitwise complement of its argument: the `n`th bit is one in the result if, and only if, the `n`th bit is zero in `integer`, and vice-versa. The result equals -1 - `integer`.

```lisp
(lognot 5)
     ⇒ -6
;;  5  =  …000101
;; becomes
;; -6  =  …111010
```

### Function: **logcount** *integer*

This function returns the *Hamming weight* of `integer`: the number of ones in the binary representation of `integer`. If `integer` is negative, it returns the number of zero bits in its two’s complement binary representation. The result is always nonnegative.

```lisp
(logcount 43)     ;  43 = …000101011
     ⇒ 4
(logcount -43)    ; -43 = …111010101
     ⇒ 3
```
