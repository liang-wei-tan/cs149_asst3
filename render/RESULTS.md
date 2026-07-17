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


### Iteration 2: Naive way which runs 1 at a time using CUDA RTX3080
passes but not the intended solutions. Takes forever to run. No parallelism at all.
./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [174.4321, 175.2603, 175.5887]
[rgb] Reference times:  [0.118, 0.118, 0.1163]

Running scene: rand10k...
[rand10k] Correctness passed!


### Iteration 3: Parallelizing at pixel level. RTX3080
Implementaiton is correct but unsure if there is regression due to too many threads/pixels.
Still not fully parallelizing due to processing circle 1 at a time. 

./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [0.1423, 0.1441, 0.1417]
[rgb] Reference times:  [0.1183, 0.1199, 0.1181]

Running scene: rand10k...
[rand10k] Correctness passed!
[rand10k] Student times:  [72.1443, 71.9852, 72.0972]
[rand10k] Reference times:  [1.285, 1.2842, 1.2863]

Running scene: rand100k...
[rand100k] Correctness passed!
[rand100k] Student times:  [696.9347, 699.7481, 698.8885]
[rand100k] Reference times:  [11.0942, 10.9878, 10.9748]

Running scene: pattern...
[pattern] Correctness passed!
[pattern] Student times:  [5.2207, 5.2571, 5.2984]
[pattern] Reference times:  [0.1832, 0.1823, 0.1809]

Running scene: snowsingle...
[snowsingle] Correctness passed!
[snowsingle] Student times:  [276.3594, 272.6792, 285.5226]
[snowsingle] Reference times:  [7.7745, 7.7734, 7.7714]

Running scene: biglittle...
[biglittle] Correctness passed!
[biglittle] Student times:  [125.9654, 125.6786, 125.6802]
[biglittle] Reference times:  [6.3252, 6.3221, 6.3219]

Running scene: rand1M...
[rand1M] Correctness passed!

### Iteration 4: 
Compute dependency by iterating to element in front to identify latest depdency
compute so long as dependency is completed. Faster but results doesn't seem right

./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [0.3925, 0.3859, 0.3867]
[rgb] Reference times:  [0.0985, 0.0977, 0.0964]

Running scene: rand10k...
[rand10k] Correctness failed ... Check ./logs/correctness_rand10k.log
[rand10k] Student times:  [4.9392, 4.9489, 4.9682]
[rand10k] Reference times:  [0.9483, 0.9848, 0.9573]

Running scene: rand100k...
[rand100k] Correctness failed ... Check ./logs/correctness_rand100k.log
[rand100k] Student times:  [137.2426, 137.5571, 137.3884]
[rand100k] Reference times:  [8.6014, 8.616, 8.6138]

Running scene: pattern...
[pattern] Correctness passed!
[pattern] Student times:  [0.3826, 0.3849, 0.4414]
[pattern] Reference times:  [0.1621, 0.1632, 0.1566]

Running scene: snowsingle...
[snowsingle] Correctness failed ... Check ./logs/correctness_snowsingle.log
[snowsingle] Student times:  [18.0004, 17.9517, 17.9982]
[snowsingle] Reference times:  [5.5412, 5.5361, 5.536]

Running scene: biglittle...
[biglittle] Correctness failed ... Check ./logs/correctness_biglittle.log
[biglittle] Student times:  [296.1432, 296.4168, 296.4652]
[biglittle] Reference times:  [4.9616, 4.9538, 4.9793]

Running scene: rand1M...
[rand1M] Correctness failed ... Check ./logs/correctness_rand1M.log
[rand1M] Student times:  [639.0724, 635.7351, 627.7335]
[rand1M] Reference times:  [57.2145, 57.2186, 57.2111]

Running scene: micro2M...
[micro2M] Correctness failed ... Check ./logs/correctness_micro2M.log
[micro2M] Student times:  [647.3263, 652.2673, 650.5043]
[micro2M] Reference times:  [109.0251, 108.8806, 108.9531]
------------
Score table:
------------
--------------------------------------------------------------------------
| Scene Name      | Ref Time (T_ref) | Your Time (T)   | Score           |
--------------------------------------------------------------------------
| rgb             | 0.0964           | 0.3859          | 4               |
| rand10k         | 0.9483           | (F)             | 0               |
| rand100k        | 8.6014           | (F)             | 0               |
| pattern         | 0.1566           | 0.3826          | 5               |
| snowsingle      | 5.536            | (F)             | 0               |
| biglittle       | 4.9538           | (F)             | 0               |
| rand1M          | 57.2111          | (F)             | 0               |
| micro2M         | 108.8806         | (F)             | 0               |
--------------------------------------------------------------------------
|                                    | Total score:    | 9/72            |
--------------------------------------------------------------------------