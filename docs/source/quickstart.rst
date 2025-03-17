Quickstart
=========
.. _Libraries:

The toolchain this guide is focusing on is AdaptiveCpp.
Follow the installation instructions in the "Installation" section for Ubuntu.
If AdaptiveCpp is installed correctly, acpp will have SYCL libraries automatically linked.
For other compilers use -fsycl (only tested on Intel icpx).

.. _HelloWorld:

Since SYCL is meant for parallel computation, Vector addition will be used.

Sample Code:

.. code-block:: cpp
    #include <sycl/sycl.hpp>
    #include <iostream>
    #include <vector>
    constexpr size_t N = 10;
    int main()
    {
        sycl::queue q{sycl::default_selector_v};
        std::cout << "Running on device: "
                << q.get_device().get_info<sycl::info::device::name>()
                << std::endl;
        float *a = sycl::malloc_device<float>(N, q);
        float *b = sycl::malloc_device<float>(N, q);
        float *c = sycl::malloc_device<float>(N, q);
        std::vector<float> h_a(N), h_b(N), h_c(N);
        for (size_t i = 0; i < N; i++)
        {
            h_a[i] = static_cast<float>(i);
            h_b[i] = static_cast<float>(i * 2);
        }
        q.memcpy(a, h_a.data(), sizeof(float) * N).wait();
        q.memcpy(b, h_b.data(), sizeof(float) * N).wait();
        q.parallel_for(sycl::range<1>(N), [=](sycl::id<1> idx)
                    { c[idx] = a[idx] + b[idx]; })
            .wait();
        q.memcpy(h_c.data(), c, sizeof(float) * N).wait();
        std::cout << "Vector A: ";
        for (size_t i = 0; i < N; i++)
        {
            std::cout << h_a[i] << " ";
        }
        std::cout << std::endl;
        std::cout << "Vector B: ";
        for (size_t i = 0; i < N; i++)
        {
            std::cout << h_b[i] << " ";
        }
        std::cout << std::endl;
        std::cout << "Result (A + B): ";
        for (size_t i = 0; i < N; i++)
        {
            std::cout << h_c[i] << " ";
        }
        std::cout << std::endl;
        sycl::free(a, q);
        sycl::free(b, q);
        sycl::free(c, q);
        return 0;
    }

Sample Output
------------

.. code-block:: console

    Running on device: AdaptiveCpp OpenMP host device
    Vector A: 0 1 2 3 4 5 6 7 8 9
    Vector B: 0 2 4 6 8 10 12 14 16 18
    Result (A + B): 0 3 6 9 12 15 18 21 24 27
    

.. _Code Explanation:

.. code-block:: cpp
    #include <sycl/sycl.hpp>
    #include <iostream>
    #include <vector>
    constexpr size_t N = 10;

Using online resources, there will be some references to CL/sycl.hpp and others to sycl/sycl.hpp.
The version CL/sycl.hpp should be considered legacy and not used anymore. 
Both sycl and CL should supply all the necessary function prototypes.

.. code-block:: cpp
    sycl::queue q{sycl::default_selector_v};
    std::cout << "Running on device: "
            << q.get_device().get_info<sycl::info::device::name>()
            << std::endl;

Q is what is mainly used. The sycl devices that can be compiled for depends on