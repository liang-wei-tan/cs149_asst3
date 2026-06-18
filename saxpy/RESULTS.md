## Results from assignment 1 which uses ISPC
N = 20M
[saxpy ispc]:           [4.166] ms      [71.543] GB/s   [9.602] GFLOPS
[saxpy task ispc]:      [5.243] ms      [56.838] GB/s   [7.629] GFLOPS

## GPU results
N = 100M
./cudaSaxpy 
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 12037 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Running 3 timing tests:
Effective BW by CUDA saxpy: 190.157 ms          [5.877 GB/s]
Kernel execution time: 3.830 ms
Effective BW by CUDA saxpy: 192.229 ms          [5.814 GB/s]
Kernel execution time: 3.629 ms
Effective BW by CUDA saxpy: 190.404 ms          [5.870 GB/s]
Kernel execution time: 3.662 ms

N = 20M
./cudaSaxpy 
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 12037 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Running 3 timing tests:
Effective BW by CUDA saxpy: 32.343 ms           [6.911 GB/s]
Kernel execution time: 0.991 ms
Effective BW by CUDA saxpy: 32.394 ms           [6.900 GB/s]
Kernel execution time: 0.793 ms
Effective BW by CUDA saxpy: 33.072 ms           [6.759 GB/s]
Kernel execution time: 0.786 ms