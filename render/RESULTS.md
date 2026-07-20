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

### Iteration 5: change back to dynamically computing dependency before any exeuction. 
Correct now but very slow. Perhaps iterating through all circles dymaically to check for depndency is too slow. Also copying a large array back to device to check completeness is also quite slow. 
./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [0.366, 0.3717, 0.3782]
[rgb] Reference times:  [0.0913, 0.104, 0.0987]

Running scene: rand10k...
[rand10k] Correctness passed!
[rand10k] Student times:  [44.368, 44.3907, 44.3914]
[rand10k] Reference times:  [0.9462, 0.9404, 0.9469]

Running scene: rand100k...
[rand100k] Correctness passed!
[rand100k] Student times:  [3903.6096, 3907.3975, 3904.9482]
[rand100k] Reference times:  [8.6073, 8.613, 8.6103]

Running scene: pattern...
[pattern] Correctness passed!
[pattern] Student times:  [0.6635, 0.6814, 0.6637]
[pattern] Reference times:  [0.1536, 0.1563, 0.1583]

Running scene: snowsingle...
[snowsingle] Correctness passed!
[snowsingle] Student times:  [441.5667, 442.5308, 441.5508]
[snowsingle] Reference times:  [5.5429, 5.543, 5.538]

Running scene: biglittle...
[biglittle] Correctness passed!
[biglittle] Student times:  [676.0049, 676.1458, 676.0125]
[biglittle] Reference times:  [4.9781, 4.9662, 4.9594]

### Iteration 7 Pixel level parallelism
Much faster
./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [0.2122, 0.2127, 0.2168]
[rgb] Reference times:  [0.1993, 0.1972, 0.1922]

Running scene: rand10k...
[rand10k] Correctness passed!
[rand10k] Student times:  [2.9748, 2.968, 2.979]
[rand10k] Reference times:  [3.0037, 3.0263, 3.007]

Running scene: rand100k...
[rand100k] Correctness passed!
[rand100k] Student times:  [29.1365, 29.1321, 29.1442]
[rand100k] Reference times:  [28.8535, 28.8656, 28.8618]

Running scene: pattern...
[pattern] Correctness passed!
[pattern] Student times:  [0.3549, 0.3557, 0.3527]
[pattern] Reference times:  [0.358, 0.3659, 0.3594]

Running scene: snowsingle...
[snowsingle] Correctness passed!
[snowsingle] Student times:  [9.0834, 9.0873, 9.0861]
[snowsingle] Reference times:  [18.5373, 18.5428, 18.5441]

Running scene: biglittle...
[biglittle] Correctness passed!
[biglittle] Student times:  [33.541, 33.5225, 33.5007]
[biglittle] Reference times:  [16.4629, 16.4485, 16.4699]

Running scene: rand1M...
[rand1M] Correctness passed!
[rand1M] Student times:  [105.201, 103.4609, 103.466]
[rand1M] Reference times:  [180.3849, 180.9225, 180.9018]

Running scene: micro2M...
[micro2M] Correctness passed!
[micro2M] Student times:  [174.4362, 174.4585, 174.4665]
[micro2M] Reference times:  [346.0611, 346.1787, 346.0944]
------------
Score table:
------------
--------------------------------------------------------------------------
| Scene Name      | Ref Time (T_ref) | Your Time (T)   | Score           |
--------------------------------------------------------------------------
| rgb             | 0.1922           | 0.2122          | 9               |
| rand10k         | 3.0037           | 2.968           | 9               |
| rand100k        | 28.8535          | 29.1321         | 9               |
| pattern         | 0.358            | 0.3527          | 9               |
| snowsingle      | 18.5373          | 9.0834          | 9               |
| biglittle       | 16.4485          | 33.5007         | 6               |
| rand1M          | 180.3849         | 103.4609        | 9               |
| micro2M         | 346.0611         | 174.4362        | 9               |
--------------------------------------------------------------------------
|                                    | Total score:    | 69/72           |
--------------------------------------------------------------------------

### Iteration 8 decrease block size to 16 by 16
Performance is a bit worse.
./checker.py 

Running scene: rgb...
[rgb] Correctness passed!
[rgb] Student times:  [0.1646, 0.1648, 0.1609]
[rgb] Reference times:  [0.1948, 0.1911, 0.2044]

Running scene: rand10k...
[rand10k] Correctness passed!
[rand10k] Student times:  [3.5683, 3.5625, 3.5569]
[rand10k] Reference times:  [3.0006, 3.0091, 3.0077]

Running scene: rand100k...
[rand100k] Correctness passed!
[rand100k] Student times:  [34.5234, 34.5184, 34.5175]
[rand100k] Reference times:  [28.8254, 28.8273, 28.8537]

Running scene: pattern...
[pattern] Correctness passed!
[pattern] Student times:  [0.4088, 0.3995, 0.4071]
[pattern] Reference times:  [0.3829, 0.3767, 0.3842]

Running scene: snowsingle...
[snowsingle] Correctness passed!
[snowsingle] Student times:  [19.5662, 19.5594, 19.5862]
[snowsingle] Reference times:  [18.5458, 18.5323, 18.5397]

Running scene: biglittle...
[biglittle] Correctness passed!
[biglittle] Student times:  [32.3366, 32.3575, 32.3394]
[biglittle] Reference times:  [16.5166, 16.5013, 16.4797]

Running scene: rand1M...
[rand1M] Correctness passed!
[rand1M] Student times:  [199.8981, 198.4788, 199.4487]
[rand1M] Reference times:  [180.49, 180.4947, 180.4928]

Running scene: micro2M...
[micro2M] Correctness passed!
[micro2M] Student times:  [363.3832, 363.3372, 366.2342]
[micro2M] Reference times:  [347.0796, 347.0821, 347.084]
------------
Score table:
------------
--------------------------------------------------------------------------
| Scene Name      | Ref Time (T_ref) | Your Time (T)   | Score           |
--------------------------------------------------------------------------
| rgb             | 0.1911           | 0.1609          | 9               |
| rand10k         | 3.0006           | 3.5569          | 9               |
| rand100k        | 28.8254          | 34.5175         | 9               |
| pattern         | 0.3767           | 0.3995          | 9               |
| snowsingle      | 18.5323          | 19.5594         | 9               |
| biglittle       | 16.4797          | 32.3366         | 6               |
| rand1M          | 180.49           | 198.4788        | 9               |
| micro2M         | 347.0796         | 363.3372        | 9               |
--------------------------------------------------------------------------
|                                    | Total score:    | 69/72           |
--------------------------------------------------------------------------