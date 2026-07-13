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