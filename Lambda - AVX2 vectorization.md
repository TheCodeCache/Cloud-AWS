# Using AVX2 vectorization in Lambda — 

Advanced Vector Extensions 2 (AVX2) is a vectorization extension to the Intel x86 instruction set  
that can perform single instruction multiple data (SIMD) instructions over vectors of 256 bits.  
For vectorizable algorithms with highly parallelizable operation, using AVX2 can enhance CPU performance,  
resulting in lower latencies and higher throughput.  

**Usecase:**  
Use the AVX2 instruction set for `compute-intensive` workloads  
such as `machine learning` inferencing, multimedia processing, scientific simulations,  
and financial modeling applications.  

To use AVX2 with your Lambda function,  
make sure that your function code is accessing AVX2-optimized code.  

**Enable AVX2 for Intel MKL** -  
Intel MKL is a library of optimized math operations that implicitly use AVX2 instructions  
when the compute platform supports them.  
Frameworks such as PyTorch build with Intel MKL by default, so you don't need to enable AVX2.  

Some libraries, such as TensorFlow,  
provide options in their build process to specify Intel MKL optimization.  
For example, with TensorFlow, use the `--config=mkl` option.  

You can also build popular scientific Python libraries, such as `SciPy` and `NumPy`, with Intel MKL  
To building these libraries with Intel MKL, use this [link](https://www.intel.com/content/www/us/en/developer/articles/technical/numpyscipy-with-intel-mkl.html)  

**AVX2 support in other languages** -  

`Python` users generally use SciPy and NumPy libraries for compute-intensive workloads.  
You can compile these libraries to enable AVX2, or you can use the Intel MKL-enabled versions of the libraries.  

`Java`'s JIT compiler can auto-vectorize your code to run with AVX2 instructions.  
For information about detecting vectorized code, check out [Code vectorization](https://cr.openjdk.java.net/~vlivanov/talks/2019_CodeOne_MTE_Vectors.pdf)  
in the JVM presentation on the OpenJDK website.  

**Reference:**  
1. https://docs.aws.amazon.com/lambda/latest/dg/runtimes-avx2.html

