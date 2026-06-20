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

## Comparison with reference time. 

./checker.py scan
Test: scan

--------------
Running tests:
--------------

Element Count: 1000000
Correctness passed!
Student Time: 0.736
Ref Time: 0.944

Element Count: 10000000
Correctness passed!
Student Time: 5.204
Ref Time: 6.454

Element Count: 20000000
Correctness passed!
Student Time: 9.917
Ref Time: 11.897

Element Count: 40000000
Correctness passed!
Student Time: 19.393
Ref Time: 22.831

-------------------------
Scan Score Table:
-------------------------
-------------------------------------------------------------------------
| Element Count   | Ref Time        | Student Time    | Score           |
-------------------------------------------------------------------------
| 1000000         | 0.944           | 0.736           | 1.25            |
| 10000000        | 6.454           | 5.204           | 1.25            |
| 20000000        | 11.897          | 9.917           | 1.25            |
| 40000000        | 22.831          | 19.393          | 1.25            |
-------------------------------------------------------------------------
|                                   | Total score:    | 5.0/5.0         |
-------------------------------------------------------------------------