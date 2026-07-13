### Iteration 1
Added pixels of a circle into a array and later process them. Required dynamically allocating memeory on heap which leads to failures when data size is big. 

./render -r cuda rgb 
Rendering to 1024x1024 image
Loaded scene with 3 circles
---------------------------------------------------------
Initializing CUDA for CudaRenderer
Found 1 CUDA devices
Device 0: NVIDIA GeForce RTX 3060
   SMs:        28
   Global mem: 11921 MB
   CUDA Cap:   8.6
---------------------------------------------------------

Running benchmark, 1 frames, beginning at frame 0 ...
Dumping frames to output_xxx.ppm
Copying image data from device
Wrote image file output_0000.ppm
Clear:    0.0832 ms
Advance:  0.0010 ms
Render:   510.4248 ms
Total:    510.5090 ms
File IO:  59.6312 ms

Overall:  0.5701 sec (note units are seconds)

When running 
 ./render -r cuda rand10k  

 i get this error
 CUDA Error: an illegal memory access was encountered


### Iteration 2: Naive way which runs 1 at a time using CUDA
passes but not the intended solutions. Takes forever to run. No parallelism at all.

### Iteration 3: Parallelizing at pixel level. 
Implementaiton is correct but unsure if there is regression due to too many threads/pixels.
Still not fully parallelizing due to processing circle 1 at a time. 

./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [0.2345, 0.2459, 0.2412]
[rgb] Reference times:  [0.1932, 0.1999, 0.1951]

Running scene: rand10k...
[rand10k] Correctness passed!
[rand10k] Student times:  [84.8095, 85.9897, 86.0491]
[rand10k] Reference times:  [3.0072, 3.0127, 3.0081]

Running scene: rand100k...
[rand100k] Correctness passed!
[rand100k] Student times:  [848.7297, 855.5512, 855.3382]
[rand100k] Reference times:  [28.8694, 26.7312, 26.663]

Running scene: pattern...
[pattern] Correctness passed!
[pattern] Student times:  [5.0272, 5.0571, 4.9631]
[pattern] Reference times:  [0.3724, 0.3808, 0.3778]

Running scene: snowsingle...
[snowsingle] Correctness passed!
[snowsingle] Student times:  [220.0828, 222.3333, 218.7204]
[snowsingle] Reference times:  [18.559, 18.5684, 18.5549]

Running scene: biglittle...
[biglittle] Correctness passed!
[biglittle] Student times:  [218.105, 219.0696, 217.8286]
[biglittle] Reference times:  [15.328, 15.3384, 15.3449]

Running scene: rand1M...
[rand1M] Correctness passed!
... takes forever. 