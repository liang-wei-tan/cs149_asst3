## Thruster results (N=64)
./cudaScan -t
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 12037 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Array size: 64
Thrust GPU time: 0.027 ms
Scan outputs are correct!

## My implementation of exclusive scan (N=64)
./cudaScan
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 12037 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Array size: 64
Student GPU time: 0.089 ms
Scan outputs are correct!

Results are 3x faster

## Thruster (N = 100000000)
./cudaScan -n 100000000 -t
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 12037 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Array size: 100000000
Thrust GPU time: 3.226 ms
Scan outputs are correct!


## My implementation of exlcusive scan  (N = 100000000)
./cudaScan -n 100000000 
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 12037 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Array size: 100000000
Student GPU time: 36.884 ms
Scan outputs are correct!

Thruster 10x faster

