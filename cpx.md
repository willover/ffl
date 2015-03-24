### Module Description ###
The cpx module implements complex numbers. A complex number consists of
two float numbers on the stack: first the real part of the complex number
and second the imaginary part of the number.

### Module Words ###
#### Complex Structure ####
**cpx%** ( -- n )
> Get the required space for a cpx variable
#### Complex variable creation, initialisation and destruction ####
**cpx-init** ( cpx -- )
> Initialise to the zero complex number
**cpx-create** ( "`<`spaces`>`name" -- ; -- cpx )
> Create a named complex number variable in the dictionary
**cpx-new** ( -- cpx )
> Create a new complex number variable on the heap
**cpx-free** ( cpx -- )
> Free the complex number variable from the heap
#### Calculation module words ####
**cpx+add** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) -- [r5](https://code.google.com/p/ffl/source/detail?r=5) [r6](https://code.google.com/p/ffl/source/detail?r=6) )
> Add the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 to [r3](https://code.google.com/p/ffl/source/detail?r=3)+jr4
**cpx+sub** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) -- [r5](https://code.google.com/p/ffl/source/detail?r=5) [r6](https://code.google.com/p/ffl/source/detail?r=6) )
> Subtract the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 from the number [r3](https://code.google.com/p/ffl/source/detail?r=3)+jr4
**cpx+mul** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) -- [r5](https://code.google.com/p/ffl/source/detail?r=5) [r6](https://code.google.com/p/ffl/source/detail?r=6) )
> Multiply the complex numbers [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 with [r3](https://code.google.com/p/ffl/source/detail?r=3)+jr4
**cpx+rmul** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) -- [r4](https://code.google.com/p/ffl/source/detail?r=4) [r5](https://code.google.com/p/ffl/source/detail?r=5) )
> Multiply the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 with the real number [r3](https://code.google.com/p/ffl/source/detail?r=3)
**cpx+imul** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) -- [r4](https://code.google.com/p/ffl/source/detail?r=4) [r5](https://code.google.com/p/ffl/source/detail?r=5) )
> Multiply the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 with the imaginary number [r3](https://code.google.com/p/ffl/source/detail?r=3)
**cpx+div** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) -- [r5](https://code.google.com/p/ffl/source/detail?r=5) [r6](https://code.google.com/p/ffl/source/detail?r=6) )
> Divide the complex number [r3](https://code.google.com/p/ffl/source/detail?r=3)+jr4 by number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+conj** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Conjugate the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+nrm** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) )
> Calculate the square of the modulus of the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+abs** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) )
> Calculate the modulus of the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+sqrt** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the square root for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+exp** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the exponent function for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+ln** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the natural logarithm for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+sin** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the trigonometric functions sine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+cos** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the trigonometric functions cosine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+tan** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the trigonometric functions tangent for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+asin** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the inverse trigonometric function sine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+acos** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the inverse trigonometric function cosine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+atan** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the inverse trigonometric function tangent for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+sinh** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the hyperbolic function sine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+cosh** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the hyperbolic function cosine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+tanh** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the hyperbolic function tangent for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+asinh** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the inverse hyperbolic function sine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+acosh** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the inverse hyperbolic function cosine for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
**cpx+atanh** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Calculate the inverse hyperbolic function tangent for the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2
#### Conversion module words ####
**cpx+to-string** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- ; -- c-addr u )
> Convert the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 to a string, using precision and PAD
**cpx+to-polar** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Convert the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 to polar notation with radius [r3](https://code.google.com/p/ffl/source/detail?r=3) and theta [r4](https://code.google.com/p/ffl/source/detail?r=4)
**cpx+from-polar** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) )
> Convert the polar radius [r1](https://code.google.com/p/ffl/source/detail?r=1), theta [r2](https://code.google.com/p/ffl/source/detail?r=2) to complex number [r3](https://code.google.com/p/ffl/source/detail?r=3)+jr4
#### Compare module words ####
**cpx+equal?** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) [r3](https://code.google.com/p/ffl/source/detail?r=3) [r4](https://code.google.com/p/ffl/source/detail?r=4) -- ; -- flag )
> Check if the complex numbers [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 and [r3](https://code.google.com/p/ffl/source/detail?r=3)+jr4 are [true](true.md) equal
#### Variable words ####
**cpx-re@** ( F: -- r ; cpx -- )
> Get the real part of the complex number
**cpx-im@** ( F: -- r ; cpx -- )
> Get the imaginary part of the complex number
**cpx-get** ( F: -- [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) ; cpx -- )
> Get the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 from the complex variable
**cpx-set** ( F: [r1](https://code.google.com/p/ffl/source/detail?r=1) [r2](https://code.google.com/p/ffl/source/detail?r=2) -- ; cpx -- )
> Set the complex number [r1](https://code.google.com/p/ffl/source/detail?r=1)+jr2 in the complex variable
**cpx^move** ( cpx2 cpx1 -- )
> Move complex2 in complex1
**cpx^equal?** ( cpx2 cpx1 -- flag )
> Check if complex2 is [true](true.md) equal to complex1
**cpx-dump** ( cpx -- )
> Dump the complex variable


---

Generated by **ofcfrth-0.10.0**