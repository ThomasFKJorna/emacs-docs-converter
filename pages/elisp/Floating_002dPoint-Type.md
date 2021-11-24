

#### 2.4.2 Floating-Point Type

Floating-point numbers are the computer equivalent of scientific notation; you can think of a floating-point number as a fraction together with a power of ten. The precise number of significant figures and the range of possible exponents is machine-specific; Emacs uses the C data type `double` to store the value, and internally this records a power of 2 rather than a power of 10.

The printed representation for floating-point numbers requires either a decimal point (with at least one digit following), an exponent, or both. For example, ‘`1500.0`’, ‘`+15e2`’, ‘`15.0e+2`’, ‘`+1500000e-3`’, and ‘`.15e4`’ are five ways of writing a floating-point number whose value is 1500. They are all equivalent.

See [Numbers](Numbers.html), for more information.
