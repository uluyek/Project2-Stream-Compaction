CUDA Stream Compaction
======================

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 2**

* Keyu Lu
* Tested on: Windows 10, Dell Oman, NVIDIA GeForce RTX 2060

### Overview 




### Extra Credit




### Performance Analysis 



### Output
'''

****************
** SCAN TESTS **
****************
    [  47  47  12  36  22  37   4  29   4  49  43   6  47 ...  14   0 ]
==== cpu scan, power-of-two ====
   elapsed time: 9.7648ms    (std::chrono Measured)
    [  47  94 106 142 164 201 205 234 238 287 330 336 383 ... 102697394 102697394 ]
==== cpu scan, non-power-of-two ====
   elapsed time: 9.7802ms    (std::chrono Measured)
    [  47  94 106 142 164 201 205 234 238 287 330 336 383 ... 102697316 102697361 ]
    passed
==== naive scan, power-of-two ====
   elapsed time: 404.064ms    (CUDA Measured)
    passed
==== naive scan, non-power-of-two ====
   elapsed time: 368.749ms    (CUDA Measured)
    passed
==== work-efficient scan, power-of-two ====
   elapsed time: 170.563ms    (CUDA Measured)
    passed
==== work-efficient scan, non-power-of-two ====
   elapsed time: 171.039ms    (CUDA Measured)
    passed
==== work-Hard scan, power-of-two ====
   elapsed time: 32.9126ms    (CUDA Measured)
    passed
==== work-Hard scan, non-power-of-two ====
   elapsed time: 35.5858ms    (CUDA Measured)
    passed
==== thrust scan, power-of-two ====
   elapsed time: 23.2094ms    (CUDA Measured)
    passed
==== thrust scan, non-power-of-two ====
   elapsed time: 13.2728ms    (CUDA Measured)
    passed

*****************************
** STREAM COMPACTION TESTS **
*****************************
    [   1   0   0   0   0   0   1   3   1   0   3   2   2 ...   3   0 ]
==== cpu compact without scan, power-of-two ====
   elapsed time: 15.1644ms    (std::chrono Measured)
    [   1   1   3   1   3   2   2   1   1   3   1   2   1 ...   2   1 ]
    passed
==== cpu compact without scan, non-power-of-two ====
   elapsed time: 14.5539ms    (std::chrono Measured)
    [   1   1   3   1   3   2   2   1   1   3   1   2   1 ...   3   2 ]
    passed
==== cpu compact with scan ====
   elapsed time: 34.3418ms    (std::chrono Measured)
    [   1   1   3   1   3   2   2   1   1   3   1   2   1 ...   2   1 ]
    passed
==== work-efficient compact, power-of-two ====
   elapsed time: 187.802ms    (CUDA Measured)
    passed
==== work-efficient compact, non-power-of-two ====
   elapsed time: 177.168ms    (CUDA Measured)
    passed
==== cpu radix sort ***********
 ====
Passed
   elapsed time: 51.8793ms    (CPU Measured)
Press any key to continue . . .
'''

