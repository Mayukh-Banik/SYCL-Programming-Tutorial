Basic Linear Algebra Subprograms (BLAS) coding in SYCL
=======

.. _Introduction:

BLAS is an API reference that underpins NumPy among other numerical libraries.
It involves Vector-Vector, Vector-Matrix, and Matrix-Matrix operations.
Further knowledge can be gleamed from official BLAS api reference sheets (https://netlib.org/blas/).

Code setup for all functions:

.. code-block:: cpp

    #include <sycl/sycl.hpp>


---------
AXPY
---------

AXPY is for Y = A * X + Y in Vector notation, or For i = 0 -> N : Y[i] = A * X[i] + Y[i].

Using templates, and a rudimentary unoptimized version.

.. code-block:: cpp
    template <typename A, typename B, typename C>
    void axpy(int N, A alpha, B* X, incX = 1, C* Y, incY = 1, sycl::queue q = sycl::queue())
    {
        q.submit(
        [&](sycl::handler &handler)
        {
            handler.parallel_for(sycl::range<1>(N), [=](sycl::id<1> i){
                int64_t Xindex = i * incX;
                int64_t Yindex = i * incY;
                Y[Yindex] = ((C) alpha * (C) X[Xindex]) + Y[Yindex]; });
        });
    }

Line 1:
```
    template <typename A, typename B, typename C>
```
Self-Explanatory, this is for type parameters in C++ to be determined at compile time.
It is hard coded for different types in perfomant BLAS implementations, but again this is quick and dirty.

Line 2:
```
void axpy(int N, A alpha, B* X, incX = 1, C* Y, incY = 1, sycl::queue q = sycl::queue())
```
N is the number of indices to go over (in CUDA, the number of threads to launch).
Alpha is the scalar factor for X.
X and Y are pointers to the raw data value. Two things to note, it is assumed in this implementation that they are both
on the same device (sycl::queue). The other is that the pointers are allocated with a variety of: sycl::malloc_*<type>.
