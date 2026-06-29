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



### Comparison with reference time for find repeats
Implemented version where I initialized another array to hold output results, then finally, copy the results to output results via a device_to_device copy


./checker.py find_repeats
Test: find_repeats

--------------
Running tests:
--------------

Element Count: 1000000
Correctness passed!
Student Time: 0.866
Ref Time: 1.278

Element Count: 10000000
Correctness passed!
Student Time: 5.947
Ref Time: 8.674

Element Count: 20000000
Correctness passed!
Student Time: 11.289
Ref Time: 15.726

Element Count: 40000000
Correctness passed!
Student Time: 21.953
Ref Time: 28.822

-------------------------
Find_repeats Score Table:
-------------------------
-------------------------------------------------------------------------
| Element Count   | Ref Time        | Student Time    | Score           |
-------------------------------------------------------------------------
| 1000000         | 1.278           | 0.866           | 1.25            |
| 10000000        | 8.674           | 5.947           | 1.25            |
| 20000000        | 15.726          | 11.289          | 1.25            |
| 40000000        | 28.822          | 21.953          | 1.25            |
-------------------------------------------------------------------------
|                                   | Total score:    | 5.0/5.0         |
-------------------------------------------------------------------------

### Reduce copy by creating a temporary array
Results show not much difference from previous solution

./checker.py find_repeats
Test: find_repeats

--------------
Running tests:
--------------

Element Count: 1000000
Correctness passed!
Student Time: 0.822
Ref Time: 1.256

Element Count: 10000000
Correctness passed!
Student Time: 5.886
Ref Time: 8.605

Element Count: 20000000
Correctness passed!
Student Time: 11.329
Ref Time: 14.871

Element Count: 40000000
Correctness passed!
Student Time: 22.155
Ref Time: 28.662

-------------------------
Find_repeats Score Table:
-------------------------
-------------------------------------------------------------------------
| Element Count   | Ref Time        | Student Time    | Score           |
-------------------------------------------------------------------------
| 1000000         | 1.256           | 0.822           | 1.25            |
| 10000000        | 8.605           | 5.886           | 1.25            |
| 20000000        | 14.871          | 11.329          | 1.25            |
| 40000000        | 28.662          | 22.155          | 1.25            |
-------------------------------------------------------------------------
|                                   | Total score:    | 5.0/5.0         |
-------------------------------------------------------------------------


### Setting block size to 16
Doing this hurts performance since it limits parallelism
 ./cudaScan -m find_repeats -n 10000000
---------------------------------------------------------
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 11910 MB
   CUDA Cap:   8.6
---------------------------------------------------------
Array size: 10000000
Student GPU time: 10.416 ms
Find_repeats outputs are correct!

