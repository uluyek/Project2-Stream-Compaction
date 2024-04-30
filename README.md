CUDA Stream Compaction
======================

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 2**

* Keyu Lu
* Tested on: Windows 10, Dell Oman, NVIDIA GeForce RTX 2060

### Overview 
**My Implementations:**
 - part 1 : implement cpu version of `StreamCompaction::CPU::scan`,`StreamCompaction::CPU::compactWithoutScan`,`StreamCompaction::CPU::compactWithScan`
 - part 2 : naive gpu scan algorithm
 - part 3 :  Work-Efficient GPU Scan & Stream Compaction
 - part 4 :  Using Thrust's Implementation
 - part 5 : explain the reason "Why is My GPU Approach So Slow?" (Extra Credit)
 - Part 6 : Radix Sort  (cpu version of Radix Sort) (Extra Credit)
 - Part 7 : Optimize the code so as to avoid warp divergence  (Extra Credit)
   
   **Note**
 - For the code of Part 7, I added two files `hard.cu` and `hard.h`

### Performance Analysis 


### output (array size 2^22)
```
****************
** SCAN TESTS **
****************
    [  41  23  34  46  30  27  10   8  18   9  43  34  13 ...  30   0 ]
==== cpu scan, power-of-two ====
   elapsed time: 5.8957ms    (std::chrono Measured)
    [  41  64  98 144 174 201 211 219 237 246 289 323 336 ... 102714549 102714549 ]
==== cpu scan, non-power-of-two ====
   elapsed time: 5.875ms    (std::chrono Measured)
    [  41  64  98 144 174 201 211 219 237 246 289 323 336 ... 102714441 102714490 ]
    passed
==== naive scan, power-of-two ====
   elapsed time: 127.275ms    (CUDA Measured)
    passed
==== naive scan, non-power-of-two ====
   elapsed time: 128.733ms    (CUDA Measured)
    passed
==== work-efficient scan, power-of-two ====
   elapsed time: 87.0237ms    (CUDA Measured)
    passed
==== work-efficient scan, non-power-of-two ====
   elapsed time: 87.1689ms    (CUDA Measured)
    passed
==== work-Hard scan, power-of-two ====
   elapsed time: 12.4251ms    (CUDA Measured)
    passed
==== work-Hard scan, non-power-of-two ====
   elapsed time: 12.6988ms    (CUDA Measured)
    passed
==== thrust scan, power-of-two ====
   elapsed time: 6.19622ms    (CUDA Measured)
    passed
==== thrust scan, non-power-of-two ====
   elapsed time: 4.19597ms    (CUDA Measured)
    passed

*****************************
** STREAM COMPACTION TESTS **
*****************************
    [   1   3   2   2   0   2   2   3   2   3   3   0   1 ...   2   0 ]
==== cpu compact without scan, power-of-two ====
   elapsed time: 6.8114ms    (std::chrono Measured)
    [   1   3   2   2   2   2   3   2   3   3   1   1   3 ...   1   1 ]
    passed
==== cpu compact without scan, non-power-of-two ====
   elapsed time: 6.8995ms    (std::chrono Measured)
    [   1   3   2   2   2   2   3   2   3   3   1   1   3 ...   2   1 ]
    passed
==== cpu compact with scan ====
   elapsed time: 19.6372ms    (std::chrono Measured)
    [   1   3   2   2   2   2   3   2   3   3   1   1   3 ...   1   1 ]
    passed
==== work-efficient compact, power-of-two ====
   elapsed time: 86.7317ms    (CUDA Measured)
    passed
==== work-efficient compact, non-power-of-two ====
   elapsed time: 86.2505ms    (CUDA Measured)
    passed
==== cpu radix sort ***********
 ====
Passed
   elapsed time: 30.234ms    (CPU Measured)
```
